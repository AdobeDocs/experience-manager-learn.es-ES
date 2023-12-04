---
title: Desarrollo de un trabajador de Asset compute
description: Los assets computes de trabajo son el núcleo de los proyectos de Asset compute, ya que proporcionan una funcionalidad personalizada que organiza el trabajo realizado en un recurso para crear una nueva representación.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 652
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 0%

---

# Desarrollo de un trabajador de Asset compute

Los assets computes de trabajo son el núcleo de un proyecto de Asset compute, ya que proporcionan una funcionalidad personalizada que organiza el trabajo realizado en un recurso para crear una nueva representación.

El proyecto de Asset compute genera automáticamente un programa de trabajo simple que copia el archivo binario original del recurso en una representación con nombre, sin ninguna transformación. En este tutorial modificaremos este trabajador para crear una representación más interesante, para ilustrar el poder de los trabajadores de la Asset compute.

Crearemos un asistente de Asset compute que genera una nueva representación de imagen horizontal que cubre el espacio vacío a la izquierda y a la derecha de la representación del recurso con una versión borrosa. La anchura, altura y desenfoque de la representación final se parametriza.

## Flujo lógico de una invocación de trabajador de Asset compute

Los trabajadores de asset compute implementan el contrato de API de trabajo del SDK de Asset compute, en el `renditionCallback(...)` función, que conceptualmente es:

+ __Entrada:__ AEM Parámetros binarios y de perfil de procesamiento originales de un recurso de
+ __Salida:__ AEM Una o más representaciones para agregar al recurso de la

![flujo lógico del trabajador de asset compute](./assets/worker/logical-flow.png)

1. AEM El servicio de autor de la invoca al trabajador de Asset compute, proporcionando el __(1 bis)__ binario original (`source` ), y __(1 ter)__ cualquier parámetro definido en el perfil de procesamiento (`rendition.instructions` parámetro).
1. El SDK de Asset compute organiza la ejecución del trabajo de metadatos de Asset compute personalizado `renditionCallback(...)` función, generando una nueva representación binaria, basada en el binario original del recurso __(1 bis)__ y cualquier parámetro __(1 ter)__.

   + En este tutorial, la representación se crea &quot;en proceso&quot;, lo que significa que el trabajador compone la representación, pero el binario de origen se puede enviar a otras API de servicio web para la generación de representaciones también.

1. El trabajador de Asset compute guarda los datos binarios de la nueva representación en `rendition.path`.
1. Los datos binarios escritos en `rendition.path` se transporta mediante el SDK de Asset compute AEM al servicio de autor de la aplicación y se expone como __(4 bis)__ una representación de texto y __(4 ter)__ persistió en el nodo de metadatos del recurso.

El diagrama anterior articula las preocupaciones del desarrollador de la Asset compute y el flujo lógico para la invocación del trabajador de la Asset compute. Para los curiosos, la [detalles internos de la ejecución de la Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) están disponibles, pero solo se puede depender de los contratos de API del SDK de Asset compute pública.

## Anatomía de un trabajador

Todos los trabajadores de la Asset compute siguen la misma estructura básica y contrato de entrada/salida.

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

1. Asegúrese de que el proyecto de Asset compute esté abierto en el código VS
1. Vaya a `/actions/worker` carpeta
1. Abra el `index.js` archivo

Este es el archivo JavaScript de trabajo que modificaremos en este tutorial.

## Instalación e importación de módulos npm compatibles

Al estar basados en Node.js, los proyectos de Asset compute se benefician de lo robusto [ecosistema de módulo npm](https://npmjs.com). Para aprovechar los módulos npm, primero debemos instalarlos en nuestro proyecto de Asset compute.

En este trabajador, aprovechamos el [saltar](https://www.npmjs.com/package/jimp) para crear y manipular la imagen de representación directamente en el código de Node.js.

>[!WARNING]
>
>No todos los módulos npm para la manipulación de recursos son compatibles con Asset compute. No se admiten módulos npm que dependan de la existencia de aplicaciones como ImageMagick u otras bibliotecas dependientes del sistema operativo. Se recomienda limitar el uso de módulos npm solo para JavaScript.

1. Abra la línea de comandos en la raíz del proyecto de Asset compute (esto se puede hacer en VS Code mediante ). __Terminal > Nuevo terminal__) y ejecute el comando:

   ```
   $ npm install jimp
   ```

1. Importe el `jimp` en el código de trabajador para que se pueda utilizar mediante el `Jimp` Objeto JavaScript.
Actualice el `require` directivas en la parte superior de la `index.js` para importar el `Jimp` objeto de la `jimp` módulo:

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

Los trabajadores de asset compute AEM pueden leer parámetros que se pueden pasar a través de los perfiles de procesamiento definidos en el servicio de creación as a Cloud Service de. Los parámetros se pasan al trabajador a través de `rendition.instructions` objeto.

Se pueden leer accediendo a `rendition.instructions.<parameterName>` en el código de trabajador.

Aquí vamos a leer en la representación configurable de `SIZE`, `BRIGHTNESS` y `CONTRAST`, proporcionando valores predeterminados si no se ha proporcionado ninguno a través del perfil de procesamiento. Tenga en cuenta que `renditions.instructions` AEM se pasan como cadenas cuando se invocan desde perfiles de procesamiento as a Cloud Service, por lo que debe asegurarse de que se transforman en los tipos de datos correctos en el código de trabajo.

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

Los trabajadores de asset compute pueden encontrar situaciones que resulten en errores. El SDK de Asset compute de Adobe proporciona [un conjunto de errores predefinidos](https://github.com/adobe/asset-compute-commons#asset-compute-errors) que se pueden generar cuando se encuentran este tipo de situaciones. Si no se aplica ningún tipo de error específico, la variable `GenericError` se puede utilizar o una personalización específica `ClientErrors` se puede definir.

Antes de comenzar a procesar la representación, asegúrese de que todos los parámetros son válidos y compatibles en el contexto de este trabajador:

+ Asegúrese de que los parámetros de instrucción de representación para `SIZE`, `CONTRAST`, y `BRIGHTNESS` son válidos. Si no, genera un error personalizado `RenditionInstructionsError`.
   + Una personalización `RenditionInstructionsError` clase que extiende `ClientError` se define en la parte inferior de este archivo. El uso de un error personalizado específico resulta útil cuando [escribir pruebas](../test-debug/test.md) para el trabajador.

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

1. Crear un nuevo `renditionImage` lienzo en dimensiones cuadradas especificadas mediante la variable `size` parámetro.
1. Crear un `image` del binario del recurso de origen
1. Utilice el __Jimp__ para transformar la imagen:
   + Recortar la imagen original en un cuadrado centrado
   + Cortar un círculo desde el centro de la imagen al cuadrado
   + Escalar para que se ajuste a las dimensiones definidas por el `SIZE` valor de parámetro
   + Ajuste el contraste en función de `CONTRAST` valor de parámetro
   + Ajuste el brillo en función del `BRIGHTNESS` valor de parámetro
1. Coloque el transformado `image` en el centro del `renditionImage` con fondo transparente
1. Escriba el compuesto, `renditionImage` hasta `rendition.path` AEM para que pueda guardarse de nuevo en la pantalla como una representación de recursos.

Este código emplea el [API de Jimp](https://github.com/oliver-moran/jimp#jimp) para realizar estas transformaciones de imagen.

Los trabajadores de asset compute deben finalizar su trabajo sincrónicamente y el `rendition.path` debe estar completamente escrito en antes de la `renditionCallback` completa. Esto requiere que las llamadas a funciones asincrónicas se realicen de forma sincrónica mediante `await` operador. Si no está familiarizado con las funciones asincrónicas de JavaScript y con cómo hacer que se ejecuten de forma sincrónica, familiarícese con [Operador de espera de JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

El trabajador terminado `index.js` debería tener un aspecto similar al siguiente:

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

Ahora que el código de trabajador está completo y se registró y configuró anteriormente en [manifest.yml](./manifest.md), se puede ejecutar utilizando la herramienta de desarrollo de Assets computes local para ver los resultados.

1. Desde la raíz del proyecto de Asset compute
1. Ejecutar `aio app run`
1. Espere a que la herramienta de desarrollo de Assets computes se abra en una nueva ventana
1. En el __Seleccionar un archivo...__ , seleccione una imagen de muestra para procesar
   + Seleccione un archivo de imagen de muestra para utilizarlo como binario del recurso de origen
   + Si todavía no existe, pulse el botón __(+)__ a la izquierda y cargue un [imagen de muestra](../assets/samples/sample-file.jpg) y actualice la ventana del explorador de Herramientas de desarrollo
1. Actualizar `"name": "rendition.png"` como este trabajador para generar un PNG transparente.
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

1. Tocar __Ejecutar__ y espere a que se genere la representación
1. El __Representaciones__ previsualiza la representación generada. Pulse la vista previa de la representación para descargar la representación completa

   ![Representación PNG predeterminada](./assets/worker/default-rendition.png)

### Ejecutar el trabajador con parámetros

Los parámetros, pasados a través de configuraciones de Perfil de procesamiento, se pueden simular en las herramientas de desarrollo de Assets computes proporcionándolos como pares clave/valor en el parámetro de representación JSON.

>[!WARNING]
>
>AEM Durante el desarrollo local, los valores se pueden pasar utilizando varios tipos de datos, cuando se pasan desde perfiles de procesamiento de Cloud Service como cadenas, por lo que asegúrese de que se analizan los tipos de datos correctos si es necesario.
> Por ejemplo, el de Jimp `crop(width, height)` requiere que sus parámetros sean `int`Es... If `parseInt(rendition.instructions.size)` no se analiza en un int y, a continuación, se llama a `jimp.crop(SIZE, SIZE)` falla porque los parámetros son de tipo &quot;cadena&quot; incompatible.

Nuestro código acepta parámetros para:

+ `size` define el tamaño de la representación (altura y anchura como números enteros)
+ `contrast` define el ajuste de contraste, debe estar entre -1 y 1, como flotante
+ `brightness`  define el ajuste brillante, debe estar entre -1 y 1, como flotante

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

1. Tocar __Ejecutar__ de nuevo
1. Pulse la vista previa de la representación para descargar y revisar la representación generada. Observe sus dimensiones y cómo se han cambiado el contraste y el brillo en comparación con la representación predeterminada.

   ![Representación PNG con parámetros](./assets/worker/parameterized-rendition.png)

1. Cargar otras imágenes en __Archivo de origen__ y pruebe a ejecutar el trabajador con parámetros diferentes.

## Worker index.js en Github

La final `index.js` está disponible en Github en:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Solución de problemas

+ [Representación parcialmente dibujada/dañada](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
