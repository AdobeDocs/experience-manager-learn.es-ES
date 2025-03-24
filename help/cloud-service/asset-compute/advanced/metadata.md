---
title: Desarrollo de un trabajador de metadatos de Asset Compute
description: Obtenga información sobre cómo crear un asistente de metadatos de Asset Compute que derive los colores más utilizados en un recurso de imagen y escriba los nombres de los colores en los metadatos del recurso en AEM.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 432
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# Desarrollo de un trabajador de metadatos de Asset Compute

Los trabajadores personalizados de Asset Compute pueden producir datos XMP (XML) que se envían de vuelta a AEM y se almacenan como metadatos en un recurso.

Los casos de uso comunes incluyen:

+ Integraciones con sistemas de terceros, como un PIM (sistema de administración de información del producto), donde se deben recuperar y almacenar metadatos adicionales en el recurso
+ Integraciones con servicios de Adobe, como contenido e IA de Commerce, para aumentar los metadatos de los recursos con atributos de aprendizaje automático adicionales
+ Obtener metadatos sobre el recurso a partir de su binario y almacenarlos como metadatos de recurso en AEM as a Cloud Service

## Qué va a hacer

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

En este tutorial crearemos un asistente de metadatos de Asset Compute que deriva los colores más utilizados en un recurso de imagen y escribe los nombres de los colores en los metadatos del recurso en AEM. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar cómo se pueden utilizar los trabajadores de Asset Compute para escribir metadatos en los recursos de AEM as a Cloud Service.

## Flujo lógico de una invocación de Asset Compute metadata worker

La invocación de los trabajadores de metadatos de Asset Compute es casi idéntica a la de [trabajadores que generan representaciones binarias](../develop/worker.md), con la diferencia principal de que el tipo de valor devuelto es una representación XMP (XML) cuyos valores también se escriben en los metadatos del recurso.

Los trabajadores de Asset Compute implementan el contrato de API de trabajo de Asset Compute SDK, en la función `renditionCallback(...)`, que conceptualmente es:

+ __Entrada:__ Parámetros binarios y de perfil de procesamiento originales de un recurso de AEM
+ __Salida:__ Una representación de XMP (XML) persistió en el recurso de AEM como una representación y en los metadatos del recurso

![Flujo lógico del trabajador de metadatos de Asset Compute](./assets/metadata/logical-flow.png)

1. El servicio de creación de AEM invoca el trabajador de metadatos de Asset Compute, proporcionando el binario original __(1a)__ del recurso y __(1b)__ cualquier parámetro definido en el perfil de procesamiento.
1. Asset Compute SDK organiza la ejecución de la función `renditionCallback(...)` del trabajador de metadatos de Asset Compute personalizado, derivando una representación de XMP (XML), basada en el binario __(1a)__ del recurso y en cualquier parámetro del perfil de procesamiento __(1b)__.
1. El trabajador de Asset Compute guarda la representación de XMP (XML) en `rendition.path`.
1. Los datos de XMP (XML) escritos en `rendition.path` se transportan a través de Asset Compute SDK al servicio de AEM Author y se exponen como __(4a)__ una representación de texto y __(4b)__ persistieron en el nodo de metadatos del recurso.

## Configuración de manifest.yml{#manifest}

Todos los trabajadores de Asset Compute deben estar registrados en [manifest.yml](../develop/manifest.md).

Abra `manifest.yml` del proyecto y agregue una entrada de trabajo que configure el nuevo trabajador, en este caso `metadata-colors`.

_Recuerde que `.yml` distingue entre espacios en blanco._

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

`function` señala a la implementación de trabajador creada en el [siguiente paso](#metadata-worker). Asigne un nombre semántico a los trabajadores (por ejemplo, `actions/worker/index.js` podría haberse denominado mejor `actions/rendition-circle/index.js`), tal como se muestra en la [URL del trabajador](#deploy) y también determine el nombre de la carpeta del grupo de pruebas de [worker](#test).

`limits` y `require-adobe-auth` se configuran de forma discreta por trabajador. En este trabajador, se asigna `512 MB` de memoria a medida que el código inspecciona (potencialmente) datos de imágenes binarias grandes. Los otros `limits` se han eliminado para utilizar los valores predeterminados.

## Desarrollo de un trabajador de metadatos{#metadata-worker}

Cree un nuevo archivo JavaScript de trabajo de metadatos en el proyecto de Asset Compute en la ruta [manifest.yml definido para el nuevo trabajador](#manifest), en `/actions/metadata-colors/index.js`

### Instalación de módulos npm

Instale los módulos npm adicionales ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors) y [color-namer](https://www.npmjs.com/package/color-namer)) que se utilizan en este trabajo de Asset Compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Código de trabajo de metadatos

Este trabajador se parece mucho al [trabajador que genera la representación](../develop/worker.md), la diferencia principal es que escribe datos de XMP (XML) en `rendition.path` para volver a guardarse en AEM.


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

Una vez completado el código de trabajo, se puede ejecutar con la herramienta de desarrollo de Asset Compute local.

Dado que nuestro proyecto de Asset Compute contiene dos trabajadores (la [representación circular](../develop/worker.md) anterior y este trabajador `metadata-colors`), la definición de perfil [Asset Compute Development Tool](../develop/development-tool.md) enumera los perfiles de ejecución de ambos trabajadores. La segunda definición de perfil apunta al nuevo trabajador `metadata-colors`.

![Representación de metadatos XML](./assets/metadata/metadata-rendition.png)

1. Desde la raíz del proyecto de Asset Compute
1. Ejecutar `aio app run` para iniciar la herramienta de desarrollo de Asset Compute
1. En la lista desplegable __Seleccionar un archivo...__, elija una [imagen de muestra](../assets/samples/sample-file.jpg) para procesar
1. En la segunda configuración de definición de perfil, que apunta al trabajador `metadata-colors`, actualice `"name": "rendition.xml"` a medida que este trabajador genere una representación XMP (XML). Opcionalmente, agregue un parámetro `colorsFamily` (valores compatibles `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

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

1. Pulse __Ejecutar__ y espere a que se genere la representación XML
   + Dado que ambos trabajadores se enumeran en la definición del perfil, se generarán ambas representaciones. Opcionalmente, se puede eliminar la definición del perfil superior que señala al [trabajador de representación circular](../develop/worker.md) para evitar ejecutarlo desde la herramienta de desarrollo.
1. La sección __Representaciones__ obtiene una vista previa de la representación generada. Pulse `rendition.xml` para descargarlo y ábralo en VS Code (o su editor de texto/XML favorito) para revisarlo.

## Prueba del trabajador{#test}

Los trabajadores de metadatos se pueden probar con el [mismo marco de pruebas de Asset Compute que las representaciones binarias](../test-debug/test.md). La única diferencia es que el archivo `rendition.xxx` en el caso de prueba debe ser la representación de XMP (XML) esperada.

1. Cree la siguiente estructura en el proyecto de Asset Compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Use el [archivo de muestra](../assets/samples/sample-file.jpg) como `file.jpg` del caso de prueba.
3. Agregue el siguiente JSON a `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Tenga en cuenta que `"fmt": "xml"` es necesario para indicar al grupo de pruebas que genere una representación basada en texto de `.xml`.

4. Proporcione el XML esperado en el archivo `rendition.xml`. Esto se puede obtener mediante:
   + Ejecutar el archivo de entrada de prueba mediante la herramienta de desarrollo y guardar la representación XML (validada).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Ejecute `aio app test` desde la raíz del proyecto de Asset Compute para ejecutar todos los grupos de pruebas.

### Implementación del trabajador en Adobe I/O Runtime{#deploy}

Para invocar este nuevo trabajador de metadatos desde los AEM Assets, debe implementarse en Adobe I/O Runtime mediante el comando:

```
$ aio app deploy
```

![implementación de aplicación aio](./assets/metadata/aio-app-deploy.png)

Tenga en cuenta que esto implementará todos los trabajadores del proyecto. Revise las [instrucciones de implementación sin abreviar](../deploy/runtime.md) para ver cómo implementar en los espacios de trabajo de ensayo y producción.

### Integración con perfiles de procesamiento de AEM{#processing-profile}

Invoque el trabajador desde AEM creando un nuevo servicio de perfil de procesamiento personalizado, o modificando uno existente, que invoque este trabajador implementado.

![Perfil de procesamiento](./assets/metadata/processing-profile.png)

1. Inicie sesión en el servicio de AEM as a Cloud Service Author como __administrador de AEM__
1. Vaya a __Herramientas > Assets > Perfiles de procesamiento__
1. __Crear__ un nuevo perfil de procesamiento o __editar__ y uno existente
1. Pulse la pestaña __Personalizar__ y luego pulse __Agregar nuevo__
1. Defina el nuevo servicio
   + __Crear representación de metadatos__: cambiar a activo
   + __Punto final:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Dirección URL del trabajador obtenida durante la [implementación](#deploy) o mediante el comando `aio app get-url`. Asegúrese de que la dirección URL apunta al espacio de trabajo correcto en función del entorno de AEM as a Cloud Service.
   + __Parámetros de servicio__
      + Pulse __Agregar parámetro__
         + Clave: `colorFamily`
         + Valor: `pantone`
            + Valores compatibles: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Tipos MIME__
      + __Incluye:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Estos son los únicos tipos MIME admitidos por los módulos npm de terceros utilizados para derivar los colores.
      + __Exclusiones:__ `Leave blank`
1. Pulse __Guardar__ en la parte superior derecha
1. Aplicar el perfil de procesamiento a una carpeta de AEM Assets si aún no lo ha hecho

### Actualizar el esquema de metadatos{#metadata-schema}

Para revisar los metadatos de colores, asigne dos nuevos campos del esquema de metadatos de la imagen a las nuevas propiedades de datos de metadatos que rellena el trabajador.

![Esquema de metadatos](./assets/metadata/metadata-schema.png)

1. En el servicio de AEM Author, vaya a __Herramientas > Assets > Esquemas de metadatos__
1. Vaya a __default__, seleccione y edite __image__, y agregue campos de formulario de solo lectura para exponer los metadatos de color generados
1. Agregar __texto de una sola línea__
   + __Etiqueta de campo__: `Colors Family`
   + __Asignar a propiedad__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Reglas > Campo > Deshabilitar edición__: Comprobado
1. Agregar __texto con varios valores__
   + __Etiqueta de campo__: `Colors`
   + __Asignar a propiedad__: `./jcr:content/metadata/wknd:colors`
1. Pulse __Guardar__ en la parte superior derecha

## Procesando recursos

![Detalles del recurso](./assets/metadata/asset-details.png)

1. En el servicio de AEM Author, vaya a __Assets > Archivos__
1. Vaya a la carpeta o subcarpeta a la que se aplica el perfil de procesamiento
1. Cargue una imagen nueva (JPEG, PNG, GIF o SVG) en la carpeta o vuelva a procesar las imágenes existentes usando el [Perfil de procesamiento](#processing-profile) actualizado
1. Cuando finalice el procesamiento, seleccione el recurso y pulse __propiedades__ en la barra de acciones superior para mostrar sus metadatos
1. Revise los `Colors Family` y `Colors` [campos de metadatos](#metadata-schema) para los metadatos escritos desde el trabajador de metadatos personalizado de Asset Compute.

Con los metadatos de color escritos en los metadatos del recurso, en el recurso `[dam:Asset]/jcr:content/metadata`, estos metadatos se indexan con una mayor capacidad de detección de recursos mediante estos términos mediante la búsqueda, y se pueden volver a escribir en el archivo binario del recurso si, después, se invoca el flujo de trabajo __DAM Metadata Writeback__ en él.

### Representación de metadatos en AEM Assets

![archivo de representación de metadatos de AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

El archivo XMP real generado por el trabajador de metadatos de Asset Compute también se almacena como una representación discreta en el recurso. Este archivo no se utiliza generalmente, sino que se utilizan los valores aplicados al nodo de metadatos del recurso, pero la salida XML sin procesar del trabajador está disponible en AEM.

## código de trabajo metadata-colors en Github

El `metadata-colors/index.js` final está disponible en Github en:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

El grupo de pruebas `test/asset-compute/metadata-colors` final está disponible en Github en:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
