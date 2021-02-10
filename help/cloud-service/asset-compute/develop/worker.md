---
title: Desarrollar un trabajador de Asset compute
description: Los trabajadores de asset compute son el núcleo de los proyectos de Asset compute, ya que proporcionan una funcionalidad personalizada que realiza u orquesta el trabajo realizado en un recurso para crear una nueva representación.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
translation-type: tm+mt
source-git-commit: 2d7ae5e46acb25eaaf7a1a35d9bbf20f7c14042e
workflow-type: tm+mt
source-wordcount: '1421'
ht-degree: 0%

---


# Desarrollar un trabajador de Asset compute

Los trabajadores de asset compute son el núcleo de un proyecto de Asset compute, ya que proporcionan una funcionalidad personalizada que realiza u orquesta el trabajo realizado en un recurso para crear una nueva representación.

El proyecto de Asset compute genera automáticamente un programa de trabajo simple que copia el binario original del recurso en una representación con nombre, sin ninguna transformación. En este tutorial modificaremos este programa de trabajo para hacer una representación más interesante, para ilustrar el poder de los trabajadores de la Asset compute.

Crearemos un programa de trabajo de Assets computes que generará una nueva representación de imágenes horizontales, que cubrirá el espacio vacío a la izquierda y a la derecha de la representación de recursos con una versión borrosa del recurso. Se parametrizará la anchura, la altura y el desenfoque de la representación final.

## Flujo lógico de una invocación de trabajador de Asset compute

Los trabajadores de asset compute implementan el contrato de API de trabajo del SDK de Asset compute, en la función `renditionCallback(...)`, que conceptualmente es:

+ __Entrada:Parámetros binarios originales de__ un recurso AEM y Perfil de procesamiento
+ __Salida:__ una o varias representaciones que se añadirán al recurso AEM

![Flujo lógico del trabajador de asset compute](./assets/worker/logical-flow.png)

1. El servicio AEM Author invoca al programa de trabajo de Asset compute, proporcionando el __(1a)__ binario original del recurso (`source` parámetro) y __(1b)__ cualquier parámetro definido en el Perfil de procesamiento (`rendition.instructions` parámetro).
1. El SDK de Asset compute orquesta la ejecución de la función `renditionCallback(...)` del trabajador de metadatos de la Asset compute personalizada, generando una nueva representación binaria, basada en el binario __(1a)__ original del recurso y en cualquier parámetro __(1b)__.

   + En este tutorial, la representación se crea &quot;en proceso&quot;, lo que significa que el trabajador compone la representación; sin embargo, el binario de origen se puede enviar también a otras API de servicios Web para la generación de representaciones.

1. El programa de trabajo de Asset compute guarda los datos binarios de la nueva representación en `rendition.path`.
1. Los datos binarios escritos en `rendition.path` se transportan mediante el SDK de Asset compute al servicio de creación de AEM y se exponen como __(4a)__ una representación de texto y __(4b)__ persisten en el nodo de metadatos del recurso.

El diagrama anterior articula las preocupaciones de cara al desarrollador de la Asset compute y el flujo lógico hacia la invocación del trabajador de la Asset compute. Para los curiosos, los [detalles internos de la ejecución de Asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html) están disponibles, sin embargo, sólo se pueden depender de los contratos públicos de la API del SDK de Asset compute.

## Anatomía de un trabajador

Todos los trabajadores de la Asset compute siguen la misma estructura básica y el mismo contrato de entrada y salida.

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

## Abrir el archivo index.js de trabajo

![index.js generado automáticamente](./assets/worker/autogenerated-index-js.png)

1. Asegúrese de que el proyecto de Asset compute esté abierto en el código VS
1. Vaya a la carpeta `/actions/worker`
1. Abra el archivo `index.js`

Este es el archivo JavaScript de trabajo que modificaremos en este tutorial.

## Instalación e importación de módulos npm compatibles

Al estar basados en Node.js, los proyectos de Asset compute se benefician del sólido [ecosistema del módulo npm](https://npmjs.com). Para aprovechar los módulos npm primero debemos instalarlos en nuestro proyecto de Asset compute.

En este programa de trabajo, aprovechamos el [jimp](https://www.npmjs.com/package/jimp) para crear y manipular la imagen de representación directamente en el código Node.js.

>[!WARNING]
>
>No todos los módulos npm para la manipulación de recursos son compatibles con la Asset compute. no se admiten los módulos npm que dependen de la existencia de aplicaciones como ImageMagick u otras bibliotecas dependientes del sistema operativo. Es mejor limitar el uso de módulos npm solo de JavaScript.

1. Abra la línea de comandos en la raíz del proyecto de Asset compute (esto se puede hacer en Código VS mediante __Terminal > Nuevo terminal__) y ejecute el comando:

   ```
   $ npm install jimp
   ```

1. Importe el módulo `jimp` en el código de trabajo para que se pueda utilizar mediante el objeto `Jimp` JavaScript.
Actualice las directivas `require` en la parte superior del `index.js` de trabajo para importar el objeto `Jimp` desde el módulo `jimp`:

   ```javascript
   'use strict';
   
   const { Jimp } = require('jimp');
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

Los trabajadores de asset compute pueden leer parámetros que se pueden pasar mediante Perfiles de procesamiento definidos en AEM como un servicio de creación de Cloud Service. Los parámetros se pasan al programa de trabajo mediante el objeto `rendition.instructions`.

Se pueden leer accediendo a `rendition.instructions.<parameterName>` en el código de trabajo.

Aquí leeremos los `SIZE`, `BRIGHTNESS` y `CONTRAST` de la representación configurable, proporcionando valores predeterminados si no se ha proporcionado ninguno mediante el Perfil de procesamiento. Tenga en cuenta que `renditions.instructions` se pasan como cadenas cuando se invocan desde AEM como Perfiles de procesamiento de Cloud Service, por lo que debe asegurarse de que se transforman en los tipos de datos correctos en el código de trabajo.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

## Errores de emisión{#errors}

Los trabajadores de la asset compute pueden encontrar situaciones que producen errores. El SDK de Asset compute de Adobe proporciona [un conjunto de errores predefinidos](https://github.com/adobe/asset-compute-commons#asset-compute-errors) que se pueden generar cuando se encuentran estas situaciones. Si no se aplica ningún tipo de error específico, se puede utilizar `GenericError` o se puede definir un `ClientErrors` personalizado específico.

Antes de empezar a procesar la representación, compruebe que todos los parámetros son válidos y compatibles en el contexto de este programa de trabajo:

+ Asegúrese de que los parámetros de instrucción de representación para `SIZE`, `CONTRAST` y `BRIGHTNESS` son válidos. Si no es así, genere un error personalizado `RenditionInstructionsError`.
   + En la parte inferior de este archivo se define una clase personalizada `RenditionInstructionsError` que extiende `ClientError`. El uso de un error personalizado específico es útil cuando [escribe pruebas](../test-debug/test.md) para el trabajador.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

Con los parámetros leídos, saneados y validados, el código se escribe para generar la representación. El pseudocódigo para la generación de representaciones es el siguiente:

1. Cree un nuevo lienzo `renditionImage` en dimensiones cuadradas especificadas mediante el parámetro `size`.
1. Crear un objeto `image` a partir del binario del recurso de origen
1. Utilice la biblioteca __Jimp__ para transformar la imagen:
   + Recortar la imagen original en un cuadrado centrado
   + Cortar un círculo desde el centro de la imagen &quot;al cuadrado&quot;
   + Escalar para ajustarse a las dimensiones definidas por el valor del parámetro `SIZE`
   + Ajustar el contraste en función del valor del parámetro `CONTRAST`
   + Ajustar el brillo en función del valor del parámetro `BRIGHTNESS`
1. Coloque el `image` transformado en el centro del `renditionImage` que tiene un fondo transparente
1. Escriba el compuesto, `renditionImage` en `rendition.path` para que pueda volver a guardarse en AEM como una representación de recursos.

Este código emplea las [API de Jimp](https://github.com/oliver-moran/jimp#jimp) para realizar estas transformaciones de imagen.

Los trabajadores de la asset compute deben terminar su trabajo sincrónicamente, y el `rendition.path` debe escribirse completamente para que el `renditionCallback` trabajador se complete. Esto requiere que las llamadas a funciones asincrónicas se hagan sincrónicas mediante el operador `await`. Si no está familiarizado con las funciones asincrónicas de JavaScript y con cómo ejecutarlas de manera sincrónica, familiarícese con [operador de espera de JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

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

    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 10,000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image of the target size with a transparent background (0x0)
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness using Jimp
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
    renditionImage.write(rendition.path);
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

Ahora que el código de trabajo se ha completado y se ha registrado y configurado previamente en [manifest.yml](./manifest.md), se puede ejecutar usando la herramienta de desarrollo de Assets computes local para ver los resultados.

1. Desde la raíz del proyecto de Asset compute
1. Ejecutar `aio app run`
1. Espere a que la herramienta de desarrollo de Assets computes se abra en una ventana nueva
1. En __Seleccione un archivo...__, seleccione una imagen de muestra para procesar
   + Seleccione un archivo de imagen de ejemplo para utilizarlo como binario de recursos de origen
   + Si todavía no hay ninguna, toque el __(+)__ a la izquierda, cargue un archivo [de imagen de muestra](../assets/samples/sample-file.jpg) y actualice la ventana del explorador de las herramientas de desarrollo
1. Actualice `"name": "rendition.png"` como este programa de trabajo para generar un PNG transparente.
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
1. Toque __Ejecutar__ y espere a que se genere la representación
1. La sección __Representaciones__ previsualización la representación generada. Toque la previsualización de representación para descargar la representación completa

   ![Representación PNG predeterminada](./assets/worker/default-rendition.png)

### Ejecutar el trabajo con parámetros

Los parámetros, pasados a través de las configuraciones de Perfil de procesamiento, se pueden simular en las herramientas de desarrollo de Asset compute proporcionándolos como pares de clave/valor en el parámetro de representación JSON.

>[!WARNING]
>
>Durante el desarrollo local, los valores se pueden pasar usando varios tipos de datos, cuando se pasan desde AEM como Perfiles de procesamiento de Cloud Service como cadenas, por lo que debe asegurarse de que se analizan los tipos de datos correctos si es necesario.
> Por ejemplo, la función `crop(width, height)` de Jimp requiere que sus parámetros sean `int`. Si `parseInt(rendition.instructions.size)` no se analiza en un int, la llamada a `jimp.crop(SIZE, SIZE)` fallará, ya que los parámetros serán de tipo &#39;String&#39; incompatible.

Nuestro código acepta parámetros para:

+ `size` define el tamaño de la representación (altura y anchura como enteros)
+ `contrast` define el ajuste de contraste, debe estar entre -1 y 1, como flotantes
+ `brightness`  define el ajuste brillante, debe estar entre -1 y 1, como flotantes

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

1. Toque __Ejecutar__ nuevamente
1. Toque la previsualización de representación para descargar y revisar la representación generada. Observe sus dimensiones y cómo se han cambiado el contraste y el brillo en comparación con la representación predeterminada.

   ![Representación PNG parametrizada](./assets/worker/parameterized-rendition.png)

1. Cargue otras imágenes en la lista desplegable __Archivo de origen__ e intente ejecutar el programa de trabajo en su contra con parámetros diferentes.

## Worker index.js en Github

El `index.js` final está disponible en Github en:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Solución de problemas

+ [Representación devuelta parcialmente dibujada/dañada](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
