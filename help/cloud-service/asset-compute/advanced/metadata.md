---
title: Desarrollo de un trabajador de metadatos de Asset compute
description: Aprenda a crear un Asset compute de trabajo de metadatos que derive los colores más utilizados en un recurso de imagen y escriba los nombres de los colores de nuevo en los metadatos del recurso en AEM.
feature: Asset Compute Microservices
topics: metadata, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 1%

---

# Desarrollo de un trabajador de metadatos de Asset compute

Los Assets computes personalizados pueden producir XMP datos (XML) que se envían de vuelta a AEM y se almacenan como metadatos en un recurso.

Los casos de uso comunes incluyen:

+ Integraciones con sistemas de terceros, como un PIM (sistema de gestión de la información del producto), en las que se deben recuperar y almacenar metadatos adicionales en el recurso
+ Integraciones con servicios de Adobe, como Content y Commerce AI para aumentar los metadatos de recursos con atributos adicionales de aprendizaje automático
+ Obtener metadatos del recurso de su binario y almacenarlo como metadatos de recurso en AEM as a Cloud Service

## Qué hará

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

En este tutorial, crearemos un programa de trabajo de metadatos de Asset compute que deriva los colores más utilizados en un recurso de imagen y escribe los nombres de los colores en los metadatos del recurso en AEM. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar cómo se pueden utilizar los trabajadores de Asset compute para escribir metadatos en los recursos de AEM as a Cloud Service.

## Flujo lógico de una invocación de trabajador de metadatos de Asset compute

La invocación de los trabajadores de metadatos de Asset compute es casi idéntica a la de [trabajadores generadores de representaciones binarias](../develop/worker.md), con la diferencia principal de que el tipo devuelto es una representación XMP (XML) cuyos valores también se escriben en los metadatos del recurso.

Los trabajadores de asset compute implementan el contrato de la API de trabajo del SDK de Asset compute en el `renditionCallback(...)` , que es conceptualmente:

+ __Entrada:__ El binario original de un recurso AEM y los parámetros de perfil de procesamiento
+ __Salida:__ Una representación XMP (XML) persistió en el recurso AEM como una representación y en los metadatos del recurso

![Flujo lógico del trabajador de metadatos de asset compute](./assets/metadata/logical-flow.png)

1. El servicio Autor de AEM invoca al trabajador de metadatos de Asset compute, proporcionando el __(1 bis)__ binario original, y __(1 ter)__ cualquier parámetro definido en el perfil de procesamiento.
1. El SDK de Asset compute organiza la ejecución del asistente de metadatos de Asset compute personalizado `renditionCallback(...)` , derivando una representación XMP (XML), basada en el binario del recurso __(1 bis)__ y cualquier parámetro de perfil de procesamiento __(1 ter)__.
1. El Asset compute de trabajo guarda la representación XMP (XML) en `rendition.path`.
1. Los datos XMP (XML) escritos en `rendition.path` se transporta a través del SDK de Asset compute al servicio de AEM Author y lo expone como __(4 bis)__ una representación de texto y __(4 ter)__ persistió en el nodo de metadatos del recurso.

## Configurar manifest.yml{#manifest}

Todos los Assets computes deben estar registrados en la [manifest.yml](../develop/manifest.md).

Abra el `manifest.yml` y añada una entrada de trabajador que configure el nuevo trabajador, en este caso `metadata-colors`.

_Recordar `.yml` distingue entre espacios en blanco._

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` apunta a la implementación del trabajador creada en la variable [paso siguiente](#metadata-worker). Asigne un nombre semántico a los trabajadores (por ejemplo, la variable `actions/worker/index.js` puede haber sido mejor nombrado `actions/rendition-circle/index.js`), tal y como se muestran en la variable [URL del trabajador](#deploy) y también determinan el [nombre de la carpeta del grupo de pruebas del trabajador](#test).

La variable `limits` y `require-adobe-auth` se configuran de forma discreta por trabajador. En este trabajador, `512 MB` de memoria se asigna a medida que el código inspecciona (potencialmente) los grandes datos de imagen binaria. El otro `limits` se eliminan para utilizar valores predeterminados.

## Desarrollo de un trabajador de metadatos{#metadata-worker}

Cree un nuevo archivo JavaScript de trabajo de metadatos en el proyecto de Asset compute en la ruta [manifest.yml definido para el nuevo trabajador](#manifest), en `/actions/metadata-colors/index.js`

### Instalación de módulos npm

Instale los módulos npm adicionales ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors)y [color-namer](https://www.npmjs.com/package/color-namer)) que se utiliza en este programa de trabajo de Asset compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Código de trabajo de metadatos

Este trabajador se parece mucho a la variable [trabajador generador de representaciones](../develop/worker.md), la diferencia principal es que escribe XMP datos (XML) en el `rendition.path` para volver a guardarse en AEM.


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces are automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## Ejecute el trabajador de metadatos localmente{#development-tool}

Una vez completado el código de trabajo, se puede ejecutar mediante la herramienta de desarrollo de Asset compute local.

Como nuestro proyecto de Asset compute contiene dos trabajadores (el anterior [representación de círculo](../develop/worker.md) y `metadata-colors` trabajador), [Herramienta de desarrollo de assets computes](../develop/development-tool.md) la definición de perfil enumera los perfiles de ejecución para ambos trabajadores. La segunda definición de perfil apunta a la nueva `metadata-colors` de trabajo.

![Representación de metadatos XML](./assets/metadata/metadata-rendition.png)

1. Desde la raíz del proyecto de Asset compute
1. Ejecutar `aio app run` para iniciar la herramienta de desarrollo de Asset compute
1. En el __Seleccionar un archivo...__ desplegable, elija un [imagen de ejemplo](../assets/samples/sample-file.jpg) para procesar
1. En la segunda configuración de definición de perfil, que señala a la variable `metadata-colors` trabajador, actualizar `"name": "rendition.xml"` a medida que este trabajador genera una representación XMP (XML). De forma opcional, agregue un `colorsFamily` parámetro (valores admitidos) `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```

1. Toque __Ejecutar__ y esperar a que se genere la representación XML
   + Dado que ambos trabajadores se enumeran en la definición del perfil, se generarán ambas representaciones. De forma opcional, la definición de perfil superior que señala a la variable [trabajador de representación de círculo](../develop/worker.md) se puede eliminar para evitar ejecutarlo desde la herramienta de desarrollo.
1. La variable __Representaciones__ previsualiza la representación generada. Toque . `rendition.xml` para descargarlo y abrirlo en el código VS (o su editor de texto/XML favorito) para revisarlo.

## Probar el trabajador{#test}

Los trabajadores de metadatos se pueden probar utilizando la variable [el mismo marco de pruebas de Asset compute que las representaciones binarias](../test-debug/test.md). La única diferencia es que `rendition.xxx` en el caso de prueba debe ser la representación XMP (XML) esperada.

1. Cree la siguiente estructura en el proyecto de Asset compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Utilice la variable [archivo de muestra](../assets/samples/sample-file.jpg) como caso de prueba `file.jpg`.
3. Añada el siguiente JSON al `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Tenga en cuenta que `"fmt": "xml"` es necesario para solicitar al grupo de prueba que genere un `.xml` representación basada en texto.

4. Proporcione el XML esperado en la variable `rendition.xml` archivo. Esto puede obtenerse mediante:
   + Ejecución del archivo de entrada de prueba mediante la herramienta de desarrollo y guardado de la representación XML (validada) fuera.

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Ejecutar `aio app test` desde la raíz del proyecto de Asset compute para ejecutar todos los grupos de pruebas.

### Implementar el programa de trabajo en Adobe I/O Runtime{#deploy}

Para invocar este nuevo trabajador de metadatos de AEM Assets, debe implementarse en Adobe I/O Runtime mediante el comando :

```
$ aio app deploy
```

![implementación de aplicaciones de aio](./assets/metadata/aio-app-deploy.png)

Tenga en cuenta que esto implementará todos los trabajadores del proyecto. Consulte la [instrucciones de implementación no abreviadas](../deploy/runtime.md) para saber cómo implementar en espacios de trabajo de fase y producción.

### Integración con Perfiles de procesamiento de AEM{#processing-profile}

Invoque el trabajador de AEM creando un nuevo servicio de perfil de procesamiento personalizado existente que invoque a este trabajador implementado o modificándolo.

![Perfil de procesamiento](./assets/metadata/processing-profile.png)

1. Inicie sesión en AEM servicio de Autor as a Cloud Service como __Administrador AEM__
1. Vaya a __Herramientas > Assets > Perfiles de procesamiento__
1. __Crear__ un nuevo, o __editar__ y existente, perfil de procesamiento
1. Toque . __Personalizado__ y pulse __Agregar nuevo__
1. Definir el nuevo servicio
   + __Crear representación de metadatos__: Alternar a activo
   + __Punto final:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Esta es la dirección URL del trabajador obtenida durante el [implementar](#deploy) o utilizando el comando `aio app get-url`. Asegúrese de que la dirección URL señale al espacio de trabajo correcto en función del entorno as a Cloud Service AEM.
   + __Parámetros de servicio__
      + Toque __Agregar parámetro__
         + Clave: `colorFamily`
         + Value: `pantone`
            + Valores compatibles: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Tipos MIME__
      + __Incluye:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Estos son los únicos tipos MIME admitidos por los módulos npm de terceros utilizados para derivar los colores.
      + __Excluye:__ `Leave blank`
1. Toque __Guardar__ en la parte superior derecha
1. Aplicar el perfil de procesamiento a una carpeta de AEM Assets si aún no lo ha hecho

### Actualizar el esquema de metadatos{#metadata-schema}

Para revisar los metadatos de colores, asigne dos campos nuevos en el esquema de metadatos de la imagen a las nuevas propiedades de datos de metadatos que rellena el trabajador.

![Esquema de metadatos](./assets/metadata/metadata-schema.png)

1. En el servicio Autor de AEM, vaya a __Herramientas > Assets > Esquemas de metadatos__
1. Vaya a __default__ y seleccione y edite __image__ y agregar campos de formulario de solo lectura para exponer los metadatos de color generados
1. Agregue un __Texto de una sola línea__
   + __Etiqueta de campo__: `Colors Family`
   + __Asignar a la propiedad__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Reglas > Campo > Desactivar edición__: Comprobado
1. Agregue un __Texto de varios valores__
   + __Etiqueta de campo__: `Colors`
   + __Asignar a la propiedad__: `./jcr:content/metadata/wknd:colors`
1. Toque __Guardar__ en la parte superior derecha

## Procesamiento de recursos

![Detalles del recurso](./assets/metadata/asset-details.png)

1. En el servicio Autor de AEM, vaya a __Assets > Archivos__
1. Vaya a la carpeta o subcarpeta a la que se aplica el perfil de procesamiento
1. Cargue una nueva imagen (JPEG, PNG, GIF o SVG) en la carpeta o reprocese las imágenes existentes mediante la [Perfil de procesamiento](#processing-profile)
1. Cuando termine el procesamiento, seleccione el recurso y pulse __propiedades__ en la barra de acciones superior para mostrar sus metadatos
1. Consulte la `Colors Family` y `Colors` [campos de metadatos](#metadata-schema) para los metadatos que se vuelven a escribir desde el Asset compute de trabajo de metadatos personalizado.

Con los metadatos de color escritos en los metadatos del recurso, en la variable `[dam:Asset]/jcr:content/metadata` recurso, estos metadatos se indexan con una mayor capacidad de detección de recursos mediante el uso de estos términos mediante la búsqueda, e incluso se pueden escribir en el binario del recurso si __Reescritura de metadatos DAM__ se invoca al flujo de trabajo.

### Representación de metadatos en AEM Assets

![Archivo de representación de metadatos de AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

El archivo de XMP generado por el trabajador de metadatos de Asset compute también se almacena como una representación discreta en el recurso. Normalmente, este archivo no se utiliza, sino que se utilizan los valores aplicados al nodo de metadatos del recurso, pero la salida XML sin procesar del trabajo está disponible en AEM.

## código de trabajo de metadata-colors en Github

El final `metadata-colors/index.js` está disponible en Github en:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

El final `test/asset-compute/metadata-colors` el grupo de pruebas está disponible en Github en:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
