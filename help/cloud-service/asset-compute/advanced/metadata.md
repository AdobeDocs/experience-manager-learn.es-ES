---
title: Desarrollo de un trabajador de metadatos de Asset compute
description: Obtenga información sobre cómo crear un asistente de metadatos de Asset compute AEM que derive los colores más utilizados en un recurso de imagen y escriba los nombres de los colores de nuevo en los metadatos del recurso en el archivo de metadatos de la.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 440
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# Desarrollo de un trabajador de metadatos de Asset compute

Los trabajadores de Asset compute XMP AEM personalizadas pueden producir datos (XML) de la que se envían de vuelta a los recursos y se almacenan como metadatos en un recurso.

Los casos de uso comunes incluyen:

+ Integraciones con sistemas de terceros, como un PIM (sistema de administración de información del producto), donde se deben recuperar y almacenar metadatos adicionales en el recurso
+ Integraciones con los servicios de Adobe, como la inteligencia artificial aplicada al contenido y al comercio para aumentar los metadatos de los recursos con atributos de aprendizaje automático adicionales.
+ AEM Derivar metadatos sobre el recurso a partir de su binario y almacenarlos como metadatos de recurso en as a Cloud Service

## Qué va a hacer

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

En este tutorial crearemos un asistente de metadatos de Asset compute AEM que deriva los colores más utilizados en un recurso de imagen y escribe los nombres de los colores de nuevo en los metadatos del recurso en el formato de metadatos. Aunque el propio trabajo es básico, este tutorial lo utiliza para explorar cómo se pueden utilizar los trabajadores de Asset compute AEM para escribir metadatos en recursos en as a Cloud Service de la.

## Flujo lógico de una invocación del trabajador de metadatos de Asset compute

La invocación de los trabajadores de metadatos de Asset compute es casi idéntica a la de [trabajadores de generación de representación binaria](../develop/worker.md)XMP , con la diferencia principal de que el tipo de valor devuelto es una representación de (XML) cuyos valores también se escriben en los metadatos del recurso.

Los trabajadores de asset compute implementan el contrato de API de trabajo del SDK de Asset compute, en el `renditionCallback(...)` función, que conceptualmente es:

+ __Entrada:__ AEM Parámetros binarios y de perfil de procesamiento originales de un recurso de
+ __Salida:__ XMP AEM Se ha mantenido una representación del recurso (XML) para el recurso de la como una representación y para los metadatos del recurso

![Flujo lógico del trabajador de metadatos de asset compute](./assets/metadata/logical-flow.png)

1. AEM El servicio de creación de recursos invoca al trabajador de metadatos de la Asset compute, proporcionando el __(1 bis)__ binario original, y __(1 ter)__ cualquier parámetro definido en el perfil de procesamiento.
1. El SDK de Asset compute organiza la ejecución del trabajo de metadatos de Asset compute personalizado `renditionCallback(...)` XMP función, derivando una representación de tipo (XML), basada en el binario del recurso __(1 bis)__ y cualquier parámetro del perfil de procesamiento __(1 ter)__.
1. El trabajador de Asset compute XMP guarda la representación de la (XML) en `rendition.path`.
1. XMP Los datos (XML) de la aplicación escritos en `rendition.path` se transporta a través del SDK de Asset compute AEM al servicio de autor de la documentación de, y lo expone como: __(4 bis)__ una representación de texto y __(4 ter)__ persistió en el nodo de metadatos del recurso.

## Configuración de manifest.yml{#manifest}

Todos los trabajadores de la Asset compute deben estar registrados en el [manifest.yml](../develop/manifest.md).

Abra el del proyecto `manifest.yml` y agregue una entrada de trabajador que configure el nuevo trabajador, en este caso `metadata-colors`.

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

`function` señala a la implementación de worker creada en el [paso siguiente](#metadata-worker). Asigne un nombre semántico a los trabajadores (por ejemplo, la variable `actions/worker/index.js` podría haber sido mejor nombrado `actions/rendition-circle/index.js`), como se muestra en la [URL del trabajador](#deploy) y también determine el [nombre de carpeta del grupo de pruebas del trabajador](#test).

El `limits` y `require-adobe-auth` se configuran de forma discreta por trabajador. En este trabajador, `512 MB` La cantidad de memoria se asigna a medida que el código inspecciona (potencialmente) datos de imagen binaria de gran tamaño. El otro `limits` se han eliminado para utilizar los valores predeterminados.

## Desarrollo de un trabajador de metadatos{#metadata-worker}

Cree un nuevo archivo JavaScript de trabajo de metadatos en el proyecto de Asset compute en la ruta [manifest.yml definido para el nuevo trabajador](#manifest), en `/actions/metadata-colors/index.js`

### Instalación de módulos npm

Instale los módulos npm adicionales ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors), y [denominador de color](https://www.npmjs.com/package/color-namer)) que se utiliza en este trabajador de Asset compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Código de trabajo de metadatos

Este trabajador tiene un aspecto muy similar al [trabajador que genera la representación](../develop/worker.md)XMP , la principal diferencia es que escribe datos de tipo XML (XML) en la interfaz de usuario de, que contiene datos de tipo `rendition.path` AEM para ser salvado de vuelta a la.


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

Una vez completado el código de trabajo, se puede ejecutar mediante la herramienta de desarrollo de Assets computes local.

Debido a que nuestro proyecto de Asset compute contiene dos empleados (el anterior [representación circular](../develop/worker.md) y esto `metadata-colors` worker), el [Herramienta de desarrollo de asset compute](../develop/development-tool.md) la definición del perfil enumera los perfiles de ejecución para ambos trabajadores. La segunda definición de perfil apunta a la nueva `metadata-colors` trabajador.

![Representación de metadatos XML](./assets/metadata/metadata-rendition.png)

1. Desde la raíz del proyecto de Asset compute
1. Ejecutar `aio app run` para iniciar la herramienta de desarrollo de Asset compute
1. En el __Seleccionar un archivo...__ desplegable, elija un [imagen de muestra](../assets/samples/sample-file.jpg) para procesar
1. En la segunda configuración de definición de perfil, que apunta a la variable `metadata-colors` trabajador, actualizar `"name": "rendition.xml"` XMP ya que este trabajador genera una representación de tipo (XML). Opcionalmente, agregue un `colorsFamily` parámetro (valores admitidos) `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

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

1. Tocar __Ejecutar__ y espere a que se genere la representación XML
   + Dado que ambos trabajadores se enumeran en la definición del perfil, se generarán ambas representaciones. De forma opcional, la definición del perfil superior que señala a [trabajador de representación circular](../develop/worker.md) se puede eliminar para evitar ejecutarlo desde la herramienta de desarrollo.
1. El __Representaciones__ previsualiza la representación generada. Pulse el botón `rendition.xml` para descargarlo y abrirlo en VS Code (o su editor de texto/XML favorito) para revisarlo.

## Prueba del trabajador{#test}

Los trabajadores de metadatos se pueden probar con la variable [mismo marco de pruebas de Asset compute que las representaciones binarias](../test-debug/test.md). La única diferencia es la `rendition.xxx` XMP en el caso de prueba debe ser la representación esperada (XML) de la.

1. Cree la siguiente estructura en el proyecto de Asset compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Utilice el [archivo de muestra](../assets/samples/sample-file.jpg) como el caso de prueba `file.jpg`.
3. Agregue el siguiente JSON a `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Tenga en cuenta `"fmt": "xml"` es necesario para indicar al grupo de pruebas que genere un `.xml` representación basada en texto.

4. Proporcione el XML esperado en la `rendition.xml` archivo. Esto se puede obtener mediante:
   + Ejecutar el archivo de entrada de prueba mediante la herramienta de desarrollo y guardar la representación XML (validada).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Ejecutar `aio app test` desde la raíz del proyecto de Asset compute para ejecutar todos los grupos de pruebas.

### Implementación del trabajador en Adobe I/O Runtime{#deploy}

Para invocar este nuevo trabajador de metadatos desde AEM Assets, debe implementarse en Adobe I/O Runtime mediante el comando:

```
$ aio app deploy
```

![implementación de aplicación aio](./assets/metadata/aio-app-deploy.png)

Tenga en cuenta que esto implementará todos los trabajadores del proyecto. Revise la [instrucciones de implementación no abreviadas](../deploy/runtime.md) para obtener información sobre cómo implementar en espacios de trabajo de fase y producción.

### AEM Integración con Perfiles de procesamiento de{#processing-profile}

AEM Invoque el trabajador desde la creación de un nuevo servicio de perfil de procesamiento personalizado, o modifique uno existente, que invoque este trabajador implementado.

![Perfil de procesamiento](./assets/metadata/processing-profile.png)

1. AEM Inicie sesión en el servicio de autor as a Cloud Service de como __AEM Administrador de__
1. Vaya a __Herramientas > Recursos > Perfiles de procesamiento__
1. __Crear__ una nueva, o __editar__ y existentes, Perfil de procesamiento
1. Pulse el botón __Personalizado__ y pulse __Añadir nuevo__
1. Defina el nuevo servicio
   + __Crear representación de metadatos__: Cambiar a activo
   + __Punto final:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Dirección URL del trabajador obtenida durante el proceso de [implementar](#deploy) o mediante el comando `aio app get-url`. AEM Asegúrese de que la dirección URL apunta al espacio de trabajo correcto en función del entorno as a Cloud Service de la.
   + __Parámetros de servicio__
      + Tocar __Añadir parámetro__
         + Clave: `colorFamily`
         + Valor: `pantone`
            + Valores compatibles: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Tipos MIME__
      + __Incluye:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Estos son los únicos tipos MIME admitidos por los módulos npm de terceros utilizados para derivar los colores.
      + __Excluye:__ `Leave blank`
1. Tocar __Guardar__ en la parte superior derecha
1. Aplicar el perfil de procesamiento a una carpeta de AEM Assets si aún no lo ha hecho

### Actualizar el esquema de metadatos{#metadata-schema}

Para revisar los metadatos de colores, asigne dos nuevos campos del esquema de metadatos de la imagen a las nuevas propiedades de datos de metadatos que rellena el trabajador.

![Esquema de metadatos](./assets/metadata/metadata-schema.png)

1. AEM En el servicio Autor de la, vaya a __Herramientas > Recursos > Esquemas de metadatos__
1. Vaya a __predeterminado__ y seleccione y edite __imagen__ y agregue campos de formulario de solo lectura para exponer los metadatos de color generados
1. Añadir un __Texto de línea única__
   + __Etiqueta de campo__: `Colors Family`
   + __Asignar a la propiedad__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Reglas > Campo > Deshabilitar edición__: Comprobado
1. Añadir un __Texto con varios valores__
   + __Etiqueta de campo__: `Colors`
   + __Asignar a la propiedad__: `./jcr:content/metadata/wknd:colors`
1. Tocar __Guardar__ en la parte superior derecha

## Procesando recursos

![Detalles del recurso](./assets/metadata/asset-details.png)

1. AEM En el servicio Autor de la, vaya a __Recursos > Archivos__
1. Vaya a la carpeta o subcarpeta a la que se aplica el perfil de procesamiento
1. Cargue una imagen nueva (JPEG, PNG, GIF o SVG) en la carpeta o vuelva a procesar las imágenes existentes con el [Perfil de procesamiento](#processing-profile)
1. Una vez completado el procesamiento, seleccione el recurso y pulse __propiedades__ en la barra de acciones superior para mostrar sus metadatos
1. Revise la `Colors Family` y `Colors` [campos de metadatos](#metadata-schema) para los metadatos escritos desde el trabajador de metadatos de Asset compute personalizado.

Con los metadatos de color escritos en los metadatos del recurso, en la variable `[dam:Asset]/jcr:content/metadata` recurso, estos metadatos se indexan con una mayor capacidad de detección de recursos mediante estos términos mediante búsqueda e incluso se pueden escribir en el binario del recurso si es así __Reescritura de metadatos DAM__ flujo de trabajo se invoca en él.

### Representación de metadatos en AEM Assets

![Archivo de representación de metadatos AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

XMP El archivo de datos generado por el trabajador de metadatos de Asset compute también se almacena como una representación discreta en el recurso. AEM Este archivo no se utiliza generalmente, sino que se utilizan los valores aplicados al nodo de metadatos del recurso, pero la salida XML sin procesar del trabajador está disponible en el modo de ejecución de la.

## código de trabajo metadata-colors en Github

La final `metadata-colors/index.js` está disponible en Github en:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

La final `test/asset-compute/metadata-colors` El conjunto de pruebas está disponible en Github en:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
