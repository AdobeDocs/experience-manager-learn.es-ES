---
title: Desarrollar un trabajador de metadatos de Asset compute
description: Obtenga información sobre cómo crear un programa de trabajo de metadatos de Asset compute que obtiene los colores más utilizados en un recurso de imagen y escribe los nombres de los colores en los metadatos del recurso en AEM.
feature: asset-compute
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
translation-type: tm+mt
source-git-commit: c2a8e6c3ae6dcaa45816b1d3efe569126c6c1e60
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 1%

---


# Desarrollar un trabajador de metadatos de Asset compute

Los trabajadores de Assets computes personalizadas pueden producir datos XMP (XML) que se envían de vuelta a AEM y se almacenan como metadatos en un recurso.

Entre los casos de uso común se incluyen:

+ Integraciones con sistemas de terceros, como un PIM (sistema de gestión de la información del producto), en las que se deben recuperar y almacenar metadatos adicionales en el recurso
+ Integraciones con servicios de Adobe, como Content and Commerce AI para aumentar los metadatos de los recursos con atributos de aprendizaje automático adicionales
+ Derivación de metadatos sobre el recurso desde su binario y almacenamiento como metadatos de recurso en AEM como Cloud Service

## Qué hará

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

En este tutorial crearemos un programa de trabajo de metadatos de Asset compute que obtiene los colores más utilizados en un recurso de imagen y escribe los nombres de los colores en los metadatos del recurso en AEM. Aunque el propio programa de trabajo es básico, este tutorial lo utiliza para explorar cómo se pueden utilizar los trabajadores de Asset compute para escribir metadatos en los recursos de AEM como Cloud Service.

## Flujo lógico de una invocación de trabajador de metadatos de Asset compute

La invocación de los trabajadores de metadatos de Asset compute es casi idéntica a la de [generadores de representaciones binarias](../develop/worker.md), con la diferencia principal de que el tipo de devolución es una representación XMP (XML) cuyos valores también se escriben en los metadatos del recurso.

Los trabajadores de asset compute implementan el contrato de API de trabajo del SDK de Asset compute, en la función `renditionCallback(...)`, que conceptualmente es:

+ __Entrada:Parámetros binarios originales de__ un recurso AEM y Perfil de procesamiento
+ __Resultado:__ Una representación XMP (XML) persistió en el recurso AEM como una representación y en los metadatos del recurso

![Flujo lógico del trabajador de metadatos de asset compute](./assets/metadata/logical-flow.png)

1. El servicio AEM Author invoca al programa de trabajo de metadatos de la Asset compute, proporcionando el archivo binario __(1a)__ original del recurso y __(1b)__ cualquier parámetro definido en el Perfil de procesamiento.
1. El SDK de Asset compute orquesta la ejecución de la función `renditionCallback(...)` del trabajador de metadatos de la Asset compute personalizada, obteniendo una representación XMP (XML), basada en el binario __(1a)__ del recurso y en cualquier parámetro de Perfil de procesamiento __(1b)__.
1. El trabajador de Asset compute guarda la representación de XMP (XML) en `rendition.path`.
1. Los datos XMP (XML) escritos en `rendition.path` se transportan mediante el SDK de Asset compute al servicio de creación de AEM y los exponen como __(4a)__ una representación de texto y __(4b)__ persisten en el nodo de metadatos del recurso.

## Configure manifest.yml{#manifest}

Todos los trabajadores de Asset compute deben estar registrados en [manifest.yml](../develop/manifest.md).

Abra el `manifest.yml` del proyecto y agregue una entrada de trabajador que configure el nuevo trabajador, en este caso `metadata-colors`.

_Recuerde que  `.yml` distingue entre espacios en blanco._

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

`function` apunta a la implementación de trabajador creada en el  [siguiente paso](#metadata-worker). Asigne un nombre semántico a los trabajadores (por ejemplo, `actions/worker/index.js` podría tener un nombre mejor `actions/rendition-circle/index.js`), como se muestra en la [dirección URL del trabajador](#deploy) y también determine el nombre de la carpeta del grupo de pruebas del [trabajador](#test).

Los `limits` y `require-adobe-auth` se configuran de forma discreta por trabajador. En este programa de trabajo, `512 MB` de memoria se asigna a medida que el código inspecciona (potencialmente) los datos de imagen binarios de gran tamaño. Los otros `limits` se eliminan para utilizar valores predeterminados.

## Desarrollar un trabajador de metadatos{#metadata-worker}

Cree un nuevo archivo JavaScript del programa de trabajo de metadatos en el proyecto de Asset compute en la ruta [manifest.yml definida para el nuevo programa de trabajo](#manifest), en `/actions/metadata-colors/index.js`

### Instalar módulos npm

Instale los módulos npm adicionales ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-color](https://www.npmjs.com/package/get-image-colors) y [color-namer](https://www.npmjs.com/package/color-namer)) que se utilizarán en este programa de trabajo de Asset compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Código de trabajo de metadatos

Este programa de trabajo se parece mucho al [programa de trabajo que genera representaciones](../develop/worker.md); la diferencia principal es que escribe datos XMP (XML) en el `rendition.path` para volver a AEM.


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
      // These namespaces will be automatically registered in AEM if they do not yet exist
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

## Ejecute el programa de trabajo de metadatos localmente{#development-tool}

Una vez completado el código de trabajo, se puede ejecutar con la herramienta de desarrollo de Assets computes local.

Dado que nuestro proyecto de Asset compute contiene dos trabajadores (la [representación de círculo anterior](../develop/worker.md) y este `metadata-colors` trabajador), la [herramienta de desarrollo de Assets computes](../develop/development-tool.md) definición de perfil lista perfiles de ejecución para ambos trabajadores. La segunda definición de perfil apunta al nuevo `metadata-colors` programa de trabajo.

![Representación de metadatos XML](./assets/metadata/metadata-rendition.png)

1. Desde la raíz del proyecto de Asset compute
1. Ejecutar `aio app run` para inicio de la herramienta de desarrollo de Assets computes
1. En __Seleccione un archivo...__ desplegable, elija una [imagen de muestra](../assets/samples/sample-file.jpg) para procesar
1. En la segunda configuración de definición de perfil, que apunta al `metadata-colors` programa de trabajo, actualice `"name": "rendition.xml"` a medida que este programa de trabajo genere una representación XMP (XML). Opcionalmente, agregue un parámetro `colorsFamily` (valores admitidos `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

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
1. Toque __Ejecutar__ y espere a que se genere la representación XML
   + Dado que ambos trabajadores están enumerados en la definición de perfil, se generarán ambas representaciones. Opcionalmente, la definición de perfil superior que apunta al [trabajador de representación de círculo](../develop/worker.md) puede eliminarse para evitar ejecutarla desde la herramienta de desarrollo.
1. La sección __Representaciones__ previsualización la representación generada. Toque `rendition.xml` para descargarlo y ábralo en VS Code (o en su editor de texto/XML favorito) para revisarlo.

## Probar el trabajador{#test}

Los trabajadores de metadatos se pueden probar usando el [mismo marco de pruebas de Asset compute que las representaciones binarias](../test-debug/test.md). La única diferencia es que el archivo `rendition.xxx` en el caso de prueba debe ser la representación XMP (XML) esperada.

1. Cree la siguiente estructura en el proyecto de Asset compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Utilice el [archivo de muestra](../assets/samples/sample-file.jpg) como `file.jpg` del caso de prueba.
3. Añada el siguiente JSON al `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Tenga en cuenta que `"fmt": "xml"` es necesario para indicar al grupo de pruebas que genere una representación `.xml` basada en texto.

4. Proporcione el XML esperado en el archivo `rendition.xml`. Esto puede obtenerse mediante:
   + Ejecución del archivo de entrada de prueba mediante la herramienta de desarrollo y almacenamiento de la representación XML (validada) fuera de línea.

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Ejecute `aio app test` desde la raíz del proyecto de Asset compute para ejecutar todos los grupos de pruebas.

### Implementar el programa de trabajo en Adobe I/O Runtime{#deploy}

Para invocar este nuevo programa de trabajo de metadatos de AEM Assets, debe implementarse en Adobe I/O Runtime mediante el comando:

```
$ aio app deploy
```

![implementación de aplicaciones de AIO](./assets/metadata/aio-app-deploy.png)

Tenga en cuenta que esto implementará todos los trabajadores del proyecto. Revise las [instrucciones de implementación sin abreviar](../deploy/runtime.md) para saber cómo implementar en los espacios de trabajo de fase y producción.

### Integración con Perfiles de procesamiento de AEM{#processing-profile}

Invoque al trabajador de AEM creando un nuevo servicio de Perfil de procesamiento personalizado o modificando uno existente que invoque a este trabajador implementado.

![Perfil de procesamiento](./assets/metadata/processing-profile.png)

1. Inicie sesión en AEM como Cloud Service Author como __AEM administrador__
1. Vaya a __Herramientas > Recursos > Perfiles de procesamiento__
1. ____ Crear un Perfil de procesamiento nuevo o  ____ editado y existente
1. Toque la ficha __Personalizado__ y toque __Añadir nuevo__
1. Definir el nuevo servicio
   + __Crear representación__ de metadatos: Alternar a activo
   + __Extremo:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Es la dirección URL del programa de trabajo obtenida durante la [implementación](#deploy) o mediante el comando `aio app get-url`. Asegúrese de que la dirección URL apunte al espacio de trabajo correcto según la AEM como entorno de Cloud Service.
   + __Parámetros de servicio__
      + Toque __Añadir parámetro__
         + Clave: `colorFamily`
         + Value: `pantone`
            + Valores admitidos: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`, 
   + __Tipos MIME__
      + __Incluye:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/svg`
         + Estos son los únicos tipos MIME admitidos por los módulos npm de terceros utilizados para derivar los colores.
      + __Excluye:__ `Leave blank`
1. Toque __Guardar__ en la parte superior derecha
1. Aplicar el Perfil de procesamiento a una carpeta de AEM Assets si aún no lo ha hecho

### Actualizar el Esquema de metadatos{#metadata-schema}

Para revisar los metadatos de colores, asigne dos campos nuevos en el esquema de metadatos de la imagen a las nuevas propiedades de datos de metadatos que rellena el programa de trabajo.

![Esquema de metadatos](./assets/metadata/metadata-schema.png)

1. En el servicio AEM Author, vaya a __Herramientas > Recursos > Esquemas de metadatos__
1. Vaya a __predeterminado__ y seleccione y edite __imagen__ y agregue campos de formulario de sólo lectura para exponer los metadatos de color generados
1. Añadir un __Texto de una sola línea__
   + __Etiqueta de campo__: `Colors Family`
   + __Asignar a la propiedad__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Reglas > Campo > Deshabilitar edición__: Verificado
1. Añadir un __Texto de varios valores__
   + __Etiqueta de campo__: `Colors`
   + __Asignar a la propiedad__: `./jcr:content/metadata/wknd:colors`
1. Toque __Guardar__ en la parte superior derecha

## Procesamiento de recursos

![Detalles del recurso](./assets/metadata/asset-details.png)

1. En el servicio AEM Author, vaya a __Recursos > Archivos__
1. Vaya a la carpeta o subcarpeta a la que se aplica el Perfil de procesamiento
1. Cargue una imagen nueva (JPEG, PNG, GIF o SVG) en la carpeta o vuelva a procesar las imágenes existentes mediante el [Perfil de procesamiento](#processing-profile) actualizado
1. Cuando se complete el procesamiento, seleccione el recurso y toque __propiedades__ en la barra de acciones superior para mostrar sus metadatos
1. Revise los campos de metadatos `Colors Family` y `Colors` [](#metadata-schema) para ver los metadatos que se han vuelto a escribir del programa de trabajo de metadatos de Asset compute personalizada.

Con los metadatos de color escritos en los metadatos del recurso, en el recurso `[dam:Asset]/jcr:content/metadata`, estos metadatos se indexan para aumentar la capacidad de detección de recursos utilizando estos términos mediante la búsqueda e incluso se pueden escribir en el binario del recurso si se invoca el flujo de trabajo __reescritura de metadatos DAM__.

### Representación de metadatos en AEM Assets

![Archivo de representación de metadatos de AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

El archivo de XMP real generado por el trabajador de metadatos de la Asset compute también se almacena como una representación discreta en el recurso. Normalmente, este archivo no se utiliza, sino que se utilizan los valores aplicados al nodo de metadatos del recurso, pero la salida XML sin procesar del programa de trabajo está disponible en AEM.

## código de trabajo de metadatos-colores en Github

El `metadata-colors/index.js` final está disponible en Github en:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

El grupo de pruebas final `test/asset-compute/metadata-colors` está disponible en Github en:

+ [aem-guide-wknd-asset-compute/test/asset-compute/metadata-color](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
