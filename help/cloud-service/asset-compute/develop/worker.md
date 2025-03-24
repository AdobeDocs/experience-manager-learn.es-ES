---
title: Desarrollo de un trabajador de Asset Compute
description: Los trabajadores de Asset Compute son la base de los proyectos de Asset Compute, ya que proporcionan una funcionalidad personalizada que organiza el trabajo realizado en un recurso para crear una nueva representación.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 451
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 0%

---

# Desarrollo de un trabajador de Asset Compute

Los trabajadores de Asset Compute son la base de un proyecto de Asset Compute, ya que proporcionan una funcionalidad personalizada que organiza el trabajo realizado en un recurso para crear una nueva representación.

El proyecto de Asset Compute genera automáticamente un trabajador simple que copia el binario original del recurso en una representación con nombre, sin ninguna transformación. En este tutorial modificaremos este trabajador para crear una representación más interesante, para ilustrar el poder de los trabajadores de Asset Compute.

Crearemos un trabajador de Asset Compute que genera una nueva representación de imagen horizontal, que cubre el espacio vacío a la izquierda y a la derecha de la representación del recurso con una versión borrosa. La anchura, altura y desenfoque de la representación final se parametriza.

## Flujo lógico de una invocación de Asset Compute Worker

Los trabajadores de Asset Compute implementan el contrato de API de trabajo de Asset Compute SDK, en la función `renditionCallback(...)`, que conceptualmente es:

+ __Entrada:__ Parámetros binarios y de perfil de procesamiento originales de un recurso de AEM
+ __Salida:__ Una o más representaciones se agregarán al recurso de AEM

![Flujo lógico de trabajo de Asset Compute](./assets/worker/logical-flow.png)

1. El servicio de creación de AEM invoca el trabajador de Asset Compute, proporcionando el binario original __(1a)__ del recurso (parámetro `source`) y __(1b)__ cualquier parámetro definido en el perfil de procesamiento (parámetro `rendition.instructions`).
1. Asset Compute SDK organiza la ejecución de la función `renditionCallback(...)` del trabajador de metadatos de Asset Compute personalizado y genera una nueva representación binaria basada en el binario original del recurso __(1a)__ y en cualquier parámetro __(1b)__.

   + En este tutorial, la representación se crea &quot;en proceso&quot;, lo que significa que el trabajador compone la representación, pero el binario de origen se puede enviar a otras API de servicio web para la generación de representaciones también.

1. El trabajador de Asset Compute guarda los datos binarios de la nueva representación en `rendition.path`.
1. Los datos binarios escritos en `rendition.path` se transportan a través de Asset Compute SDK al servicio de AEM Author y se exponen como __(4a)__ una representación de texto y __(4b)__ persistieron en el nodo de metadatos del recurso.

El diagrama anterior articula las preocupaciones del desarrollador de Asset Compute y el flujo lógico a la invocación del trabajador de Asset Compute. Para los curiosos, los [detalles internos de la ejecución de Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) están disponibles, pero solo se puede depender de los contratos de API de SDK de Asset Compute públicos.

## Anatomía de un trabajador

Todos los empleados de Asset Compute siguen la misma estructura básica y contrato de entrada/salida.

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

## Abrir el archivo index.js de trabajador

![index.js generado automáticamente](./assets/worker/autogenerated-index-js.png)

1. Asegúrese de que el proyecto de Asset Compute esté abierto en el código VS.
1. Vaya a la carpeta `/actions/worker`
1. Abrir el archivo `index.js`

Este es el archivo JavaScript de trabajo que modificaremos en este tutorial.

## Instalación e importación de módulos npm compatibles

Al estar basados en Node.js, los proyectos de Asset Compute se benefician del robusto [ecosistema de módulos npm](https://npmjs.com). Para aprovechar los módulos npm, primero debemos instalarlos en nuestro proyecto de Asset Compute.

En este trabajador, aprovechamos [jimp](https://www.npmjs.com/package/jimp) para crear y manipular la imagen de representación directamente en el código de Node.js.

>[!WARNING]
>
>Asset Compute no admite todos los módulos npm para la manipulación de recursos. No se admiten módulos npm que dependan de la existencia de aplicaciones como ImageMagick u otras bibliotecas dependientes del sistema operativo. Se recomienda limitar el uso de módulos npm solo para JavaScript.

1. Abra la línea de comandos en la raíz del proyecto de Asset Compute (esto se puede hacer en VS Code mediante __Terminal > Nuevo terminal__) y ejecute el comando:

   ```
   $ npm install jimp
   ```

1. Importe el módulo `jimp` en el código de trabajador para que pueda utilizarse mediante el objeto JavaScript `Jimp`.
Actualice las directivas `require` en la parte superior de `index.js` del trabajador para importar el objeto `Jimp` desde el módulo `jimp`:

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

## Leer parámetros

Los trabajadores de Asset Compute pueden leer los parámetros que se pueden pasar a través de los perfiles de procesamiento definidos en el servicio de AEM as a Cloud Service Author. Los parámetros se pasan al trabajador a través del objeto `rendition.instructions`.

Se pueden leer accediendo a `rendition.instructions.<parameterName>` en el código del trabajador.

Aquí leeremos `SIZE`, `BRIGHTNESS` y `CONTRAST` de la representación configurable, y proporcionaremos los valores predeterminados si no se ha proporcionado ninguno a través del perfil de procesamiento. Tenga en cuenta que `renditions.instructions` se pasan como cadenas cuando se invocan desde Perfiles de procesamiento de AEM as a Cloud Service, por lo que debe asegurarse de que se transforman en los tipos de datos correctos en el código de trabajo.

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

## Generación de errores{#errors}

Los trabajadores de Asset Compute pueden encontrar situaciones que resulten en errores. Adobe Asset Compute SDK proporciona [un conjunto de errores predefinidos](https://github.com/adobe/asset-compute-commons#asset-compute-errors) que se pueden producir cuando se encuentran estas situaciones. Si no se aplica ningún tipo de error específico, se puede usar `GenericError` o se puede definir un `ClientErrors` personalizado específico.

Antes de comenzar a procesar la representación, asegúrese de que todos los parámetros son válidos y compatibles en el contexto de este trabajador:

+ Asegúrese de que los parámetros de instrucción de representación de `SIZE`, `CONTRAST` y `BRIGHTNESS` sean válidos. Si no es así, genera un error personalizado `RenditionInstructionsError`.
   + En la parte inferior de este archivo se define una clase `RenditionInstructionsError` personalizada que amplía `ClientError`. El uso de un error personalizado específico es útil cuando [se escriben pruebas](../test-debug/test.md) para el trabajador.

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

Con los parámetros leídos, saneados y validados, el código se escribe para generar la representación. El pseudocódigo para la generación de representaciones es el siguiente:

1. Crear un nuevo lienzo `renditionImage` en dimensiones cuadradas especificadas mediante el parámetro `size`.
1. Crear un objeto `image` a partir del binario del recurso de origen
1. Use la biblioteca __Jimp__ para transformar la imagen:
   + Recortar la imagen original en un cuadrado centrado
   + Cortar un círculo desde el centro de la imagen al cuadrado
   + Escalar para ajustarse a las dimensiones definidas por el valor del parámetro `SIZE`
   + Ajustar contraste en función del valor del parámetro `CONTRAST`
   + Ajustar brillo en función del valor del parámetro `BRIGHTNESS`
1. Coloque el elemento `image` transformado en el centro de `renditionImage`, que tiene un fondo transparente
1. Escriba el compuesto `renditionImage` en `rendition.path` para que se pueda guardar de nuevo en AEM como una representación de recursos.

Este código emplea las [API Jimp](https://github.com/oliver-moran/jimp#jimp) para realizar estas transformaciones de imagen.

Los trabajadores de Asset Compute deben finalizar su trabajo sincrónicamente y se debe volver a escribir completamente en `rendition.path` antes de que finalice `renditionCallback` del trabajador. Esto requiere que las llamadas a funciones asincrónicas se hagan sincrónicas mediante el operador `await`. Si no está familiarizado con las funciones asincrónicas de JavaScript y con cómo ejecutarlas de manera sincrónica, familiarícese con [el operador de espera de JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

El trabajador `index.js` terminado debe tener un aspecto similar al siguiente:

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

Ahora que el código de trabajo está completo y se registró y configuró anteriormente en [manifest.yml](./manifest.md), se puede ejecutar usando la herramienta de desarrollo de Asset Compute local para ver los resultados.

1. Desde la raíz del proyecto de Asset Compute
1. Ejecutar `aio app run`
1. Espere a que la herramienta de desarrollo de Asset Compute se abra en una nueva ventana
1. En la lista desplegable __Seleccionar un archivo...__, seleccione una imagen de muestra para procesar
   + Seleccione un archivo de imagen de muestra para utilizarlo como binario del recurso de origen
   + Si todavía no existe, pulse __(+)__ a la izquierda, cargue un archivo de [imagen de muestra](../assets/samples/sample-file.jpg) y actualice la ventana del explorador de Herramientas de desarrollo
1. Actualice `"name": "rendition.png"` como este trabajador para generar un PNG transparente.
   + Tenga en cuenta que este parámetro &quot;name&quot; solo se utiliza para la herramienta de desarrollo y no debe depender de.

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
1. La sección __Representaciones__ obtiene una vista previa de la representación generada. Pulse la vista previa de la representación para descargar la representación completa

   ![Representación PNG predeterminada](./assets/worker/default-rendition.png)

### Ejecutar el trabajador con parámetros

Los parámetros, pasados a través de configuraciones de Perfil de procesamiento, se pueden simular en las herramientas de desarrollo de Asset Compute proporcionándolos como pares clave/valor en el parámetro de representación JSON.

>[!WARNING]
>
>Durante el desarrollo local, los valores se pueden pasar utilizando varios tipos de datos, cuando se pasan desde AEM como perfiles de procesamiento de Cloud Service como cadenas, por lo que asegúrese de que se analizan los tipos de datos correctos si es necesario.
> Por ejemplo, la función `crop(width, height)` de Jimp requiere que sus parámetros sean de `int`. Si `parseInt(rendition.instructions.size)` no se analiza en un valor int, la llamada a `jimp.crop(SIZE, SIZE)` produce un error porque los parámetros son de tipo &quot;String&quot; no compatibles.

Nuestro código acepta parámetros para:

+ `size` define el tamaño de la representación (altura y anchura como números enteros)
+ `contrast` define el ajuste de contraste, debe estar entre -1 y 1, como flotante
+ `brightness` define el ajuste brillante, debe estar entre -1 y 1, como flotante

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

1. Pulse __Ejecutar__ de nuevo
1. Pulse la vista previa de la representación para descargar y revisar la representación generada. Observe sus dimensiones y cómo se han cambiado el contraste y el brillo en comparación con la representación predeterminada.

   ![Representación PNG parametrizada](./assets/worker/parameterized-rendition.png)

1. Cargue otras imágenes en el menú desplegable __archivo Source__ e intente ejecutar el trabajador con otros parámetros.

## Worker index.js en Github

El `index.js` final está disponible en Github en:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Solución de problemas

+ [Representación parcialmente dibujada/dañada](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
