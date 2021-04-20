---
title: Desarrollo de un trabajador de Asset compute
description: Los assets computes son el núcleo de los proyectos de Asset compute, ya que proporcionan una funcionalidad personalizada que organiza el trabajo realizado en un recurso para crear una nueva representación.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: 1e5d8171832ec6b26969a8485ae970e295962828
workflow-type: tm+mt
source-wordcount: '1429'
ht-degree: 0%

---


# Desarrollo de un trabajador de Asset compute

Los assets computes son el núcleo de un proyecto de Asset compute, ya que proporcionan una funcionalidad personalizada que organiza el trabajo realizado en un recurso para crear una nueva representación.

El proyecto de Asset compute genera automáticamente un programa de trabajo simple que copia el binario original del recurso en una representación con nombre, sin ninguna transformación. En este tutorial modificaremos este programa de trabajo para hacer una representación más interesante, para ilustrar el poder de los trabajadores del Asset compute.

Se creará un programa de trabajo de Asset compute que generará una nueva representación de imagen horizontal, que cubrirá el espacio vacío a la izquierda y a la derecha de la representación de recursos con una versión borrosa del recurso. Se parametrizará la anchura, la altura y el desenfoque de la representación final.

## Flujo lógico de una invocación de trabajador de Asset compute

Los trabajadores de asset compute implementan el contrato de la API de trabajo del SDK de Asset compute, en la función `renditionCallback(...)`, que conceptualmente es:

+ __Entrada:__ el binario original de un recurso de AEM y los parámetros de perfil de procesamiento
+ __Salida:__ una o más representaciones que se agregarán al recurso AEM

![Flujo lógico del trabajador de asset compute](./assets/worker/logical-flow.png)

1. El servicio Autor de AEM invoca al trabajador de Asset compute, proporcionando el __(1a)__ binario original del recurso (`source` parámetro) y __(1b)__ cualquier parámetro definido en el perfil de procesamiento (`rendition.instructions` parámetro).
1. El SDK de Asset compute organiza la ejecución de la función `renditionCallback(...)` del trabajador de metadatos de Asset compute personalizado, generando una nueva representación binaria, basada en el binario __(1a)__ original del recurso y en cualquier parámetro __(1b)__.

   + En este tutorial, la representación se crea &quot;en proceso&quot;, lo que significa que el trabajador compone la representación, aunque el binario de origen se puede enviar a otras API de servicio web para la generación de la representación también.

1. El programa de trabajo de Asset compute guarda los datos binarios de la nueva representación en `rendition.path`.
1. Los datos binarios escritos en `rendition.path` se transportan a través del SDK de Asset compute al servicio de creación de AEM y se exponen como __(4a)__ una representación de texto y __(4b)__ persisten en el nodo de metadatos del recurso.

El diagrama anterior articula las preocupaciones de cara al desarrollador del Asset compute y el flujo lógico para invocar al trabajador del Asset compute. Para los curiosos, los [detalles internos de la ejecución de Asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html) están disponibles, sin embargo, solo se puede depender de los contratos públicos de la API del SDK de Asset compute.

## Anatomía de un trabajador

Todos los trabajadores del Asset compute siguen la misma estructura básica y el mismo contrato de entrada y salida.

```javascript
'use strict';

// Any npm module imports used by the worker
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

/**
Exports the worker implemented by a custom rendition callback function, which parametrizes the input/output contract for the worker.
 + `source` represents the asset's original binary used as the input for the worker.
 + `rendition` represents the worker's output, which is the creation of a new asset rendition.
 + `params` are optional parameters, which map to additional key/value pairs, including a sub `auth` object that contains Adobe I/O access credentials.
**/
exports.main = worker(async (source, rendition, params) => {
    // Perform any necessary source (input) checks
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        // Throw appropriate errors whenever an erring condition is met
        throw new SourceCorruptError('source file is empty');
    }

    // Access any custom parameters provided via the Processing Profile configuration
    let param1 = rendition.instructions.exampleParam;

    /** 
    Perform all work needed to transform the source into the rendition.
    
    The source data can be accessed:
        + In the worker via a file available at `source.path`
        + Or via a presigned GET URL at `source.url`
    **/
    if (success) {
        // A successful worker must write some data back to `renditions.path`. 
        // This example performs a trivial 1:1 copy of the source binary to the rendition
        await fs.copyFile(source.path, rendition.path);
    } else {
        // Upon failure an Asset Compute Error (exported by @adobe/asset-compute-commons) should be thrown.
        throw new GenericError("An error occurred!", "example-worker");
    }
});

/**
Optionally create helper classes or functions the worker's rendition callback function invokes to help organize code.

Code shared across workers, or to complex to be managed in a single file, can be broken out across supporting JavaScript files in the project and imported normally into the worker. 
**/
function customHelperFunctions() { ... }
```

## Apertura del archivo de trabajo index.js

![Index.js generado automáticamente](./assets/worker/autogenerated-index-js.png)

1. Asegúrese de que el proyecto de Asset compute esté abierto en el código VS
1. Vaya a la carpeta `/actions/worker`
1. Abra el archivo `index.js`

Este es el archivo JavaScript de trabajo que modificaremos en este tutorial.

## Instalación e importación compatibles con los módulos npm

Al estar basados en Node.js, los proyectos de Asset compute se benefician del sólido [ecosistema del módulo npm](https://npmjs.com). Para aprovechar los módulos npm primero debemos instalarlos en nuestro proyecto de Asset compute.

En este programa de trabajo, aprovechamos [jimp](https://www.npmjs.com/package/jimp) para crear y manipular la imagen de representación directamente en el código de Node.js.

>[!WARNING]
>
>No todos los módulos npm para la manipulación de recursos son compatibles con Asset compute. Los módulos npm que dependen de la existencia de aplicaciones como ImageMagick u otras bibliotecas dependientes del sistema operativo no son compatibles. Es mejor limitar el uso de módulos npm solo de JavaScript.

1. Abra la línea de comandos en la raíz del proyecto de Asset compute (esto se puede hacer en el código VS a través de __Terminal > Nuevo terminal__) y ejecute el comando:

   ```
   $ npm install jimp
   ```

1. Importe el módulo `jimp` en el código de trabajo para que se pueda utilizar mediante el objeto JavaScript `Jimp`.
Actualice las directivas `require` en la parte superior del `index.js` del trabajador para importar el objeto `Jimp` del módulo `jimp`:

   ```javascript
   'use strict';
   
   const Jimp = require('jimp');
   const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
   const fs = require('fs').promises;
   
   exports.main = worker(async (source, rendition, params) => {
       // Check handle a corrupt input source
       const stats = await fs.stat(source.path);
       if (stats.size === 0) {
           throw new SourceCorruptError('source file is empty');
       }
   
       // Do work here
   });
   ```

## Parámetros de lectura

Los trabajadores de asset compute pueden leer en parámetros que se pueden pasar a través de perfiles de procesamiento definidos en AEM como servicio de autor Cloud Service. Los parámetros se pasan al trabajador a través del objeto `rendition.instructions`.

Se pueden leer accediendo a `rendition.instructions.<parameterName>` en el código de trabajo.

Aquí leeremos los `SIZE`, `BRIGHTNESS` y `CONTRAST` de la representación configurable, proporcionando valores predeterminados si no se ha proporcionado ninguno a través del perfil de procesamiento. Tenga en cuenta que `renditions.instructions` se pasan como cadenas cuando se invocan desde AEM como perfiles de procesamiento de Cloud Service, por lo que debe asegurarse de que se transformen en los tipos de datos correctos en el código de trabajo.

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    // Processing Profiles pass in instructions as Strings, so make sure to parse to correct data types
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    // Do work here
}
```

## Generando errores{#errors}

Los trabajadores del asset compute pueden encontrar situaciones que producen errores. El SDK de Asset compute de Adobe proporciona [un conjunto de errores predefinidos](https://github.com/adobe/asset-compute-commons#asset-compute-errors) que se pueden generar cuando se encuentran estas situaciones. Si no se aplica ningún tipo de error específico, se puede utilizar el `GenericError` o se puede definir un `ClientErrors` personalizado específico.

Antes de comenzar a procesar la representación, compruebe que todos los parámetros son válidos y compatibles en el contexto de este trabajador:

+ Asegúrese de que los parámetros de instrucciones de representación para `SIZE`, `CONTRAST` y `BRIGHTNESS` sean válidos. Si no es así, genere un error personalizado `RenditionInstructionsError`.
   + En la parte inferior de este archivo se define una clase personalizada `RenditionInstructionsError` que amplía `ClientError`. El uso de un error personalizado específico es útil cuando [escribe pruebas](../test-debug/test.md) para el trabajador.

```javascript
'use strict';

const Jimp = require('jimp');
// Import the Asset Compute SDK provided `ClientError` 
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        // Ensure size is within allowable bounds
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Do work here
}

// Create a new ClientError to handle invalid rendition.instructions values
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        // Provide a:
        // + message: describing the nature of this erring condition
        // + name: the name of the error; usually same as class name
        // + reason: a short, searchable, unique error token that identifies this error
        super(message, "RenditionInstructionsError", "rendition_instructions_error");

        // Capture the strack trace
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Creación de la representación

Con los parámetros leídos, saneados y validados, el código se escribe para generar la representación. El pseudocódigo para la generación de representación es el siguiente:

1. Cree un nuevo lienzo `renditionImage` en dimensiones cuadradas especificadas mediante el parámetro `size`.
1. Crear un objeto `image` a partir del binario del recurso de origen
1. Utilice la biblioteca __Jimp__ para transformar la imagen:
   + Recortar la imagen original en un cuadrado centrado
   + Cortar un círculo desde el centro de la imagen &quot;cuadrada&quot;
   + Escala para ajustarse a las dimensiones definidas por el valor del parámetro `SIZE`
   + Ajuste el contraste en función del valor del parámetro `CONTRAST`
   + Ajustar el brillo en función del valor del parámetro `BRIGHTNESS`
1. Coloque el `image` transformado en el centro del `renditionImage` que tiene un fondo transparente
1. Escriba el compuesto, `renditionImage` en `rendition.path` para que se pueda guardar de nuevo en AEM como una representación de recursos.

Este código utiliza las [API de Jimp](https://github.com/oliver-moran/jimp#jimp) para realizar estas transformaciones de imagen.

Los trabajadores del asset compute deben terminar su trabajo sincrónicamente y el `rendition.path` debe escribirse completamente a antes de que el `renditionCallback` del trabajador se complete. Esto requiere que las llamadas a funciones asincrónicas se realicen sincrónicamente mediante el operador `await` . Si no está familiarizado con las funciones asincrónicas de JavaScript y con cómo ejecutarlas de forma sincrónica, familiarícese con el [operador de espera de JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

El trabajador terminado `index.js` debe tener el siguiente aspecto:

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read/parse and validate parameters
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image 
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness
    image.crop(
            image.bitmap.width < image.bitmap.height ? 0 : (image.bitmap.width - image.bitmap.height) / 2,
            image.bitmap.width < image.bitmap.height ? (image.bitmap.height - image.bitmap.width) / 2 : 0,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height
        )   
        .circle()
        .scaleToFit(SIZE, SIZE)
        .contrast(CONTRAST)
        .brightness(BRIGHTNESS);

    // Place the transformed image onto the transparent renditionImage to save as PNG
    renditionImage.composite(image, 0, 0)

    // Write the final transformed image to the asset's rendition
    await renditionImage.writeAsync(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Ejecución del trabajador

Ahora que el código de trabajo ha finalizado y se ha registrado y configurado anteriormente en [manifest.yml](./manifest.md), se puede ejecutar utilizando la herramienta de desarrollo de Assets computes local para ver los resultados.

1. Desde la raíz del proyecto de Asset compute
1. Ejecutar `aio app run`
1. Espere a que la herramienta de desarrollo de Assets computes se abra en una nueva ventana
1. En __Seleccione un archivo...Menú desplegable__, seleccione una imagen de ejemplo para procesar
   + Seleccione un archivo de imagen de ejemplo para utilizarlo como binario del recurso de origen
   + Si todavía no existe ninguna, pulse el __(+)__ a la izquierda, cargue un archivo [imagen de muestra](../assets/samples/sample-file.jpg) y actualice la ventana del explorador de las herramientas de desarrollo
1. Actualice `"name": "rendition.png"` como este trabajador para generar un PNG transparente.
   + Tenga en cuenta que este parámetro &quot;name&quot; solo se utiliza para la herramienta de desarrollo y no se debe confiar en él.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png"
           }
       ]
   }
   ```
1. Pulse __Ejecutar__ y espere a que se genere la representación
1. La sección __Representaciones__ previsualiza la representación generada. Pulse la vista previa de la representación para descargar la representación completa

   ![Representación PNG predeterminada](./assets/worker/default-rendition.png)

### Ejecutar el trabajo con parámetros

Los parámetros, pasados mediante configuraciones de perfil de procesamiento, se pueden simular en las herramientas de desarrollo de Asset compute al proporcionarlos como pares clave/valor en el parámetro de representación JSON.

>[!WARNING]
>
>Durante el desarrollo local, los valores se pueden pasar usando varios tipos de datos, cuando se pasan desde AEM como perfiles de procesamiento de Cloud Service como cadenas, por lo que asegúrese de que se analicen los tipos de datos correctos si es necesario.
> Por ejemplo, la función `crop(width, height)` de Jimp requiere que sus parámetros sean de `int`. Si `parseInt(rendition.instructions.size)` no se analiza en un int, la llamada a `jimp.crop(SIZE, SIZE)` fallará, ya que los parámetros serán de tipo &quot;String&quot; incompatible.

Nuestro código acepta parámetros para:

+ `size` define el tamaño de la representación (altura y anchura como enteros)
+ `contrast` define el ajuste de contraste, debe estar entre -1 y 1, como flotantes
+ `brightness`  define el ajuste luminoso, debe estar entre -1 y 1, como flotantes

Se leen en el trabajador `index.js` mediante:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Actualice los parámetros de representación para personalizar el tamaño, el contraste y el brillo.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png",
               "size": "450",
               "contrast": "0.30",
               "brightness": "0.15"
           }
       ]
   }
   ```

1. Toque __Ejecutar__ de nuevo
1. Pulse la vista previa de la representación para descargar y revisar la representación generada. Tenga en cuenta sus dimensiones y cómo se han cambiado el contraste y el brillo en comparación con la representación predeterminada.

   ![Representación PNG parametrizada](./assets/worker/parameterized-rendition.png)

1. Cargue otras imágenes a la lista desplegable __Source file__ e intente ejecutar el programa de trabajo en su contra con parámetros diferentes.

## Worker index.js en Github

El `index.js` final está disponible en Github en:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Solución de problemas

+ [Representación devuelta parcialmente dibujada/dañada](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
