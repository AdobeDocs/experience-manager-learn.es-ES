---
title: Prueba de un trabajador de Asset compute
description: El proyecto de Asset compute define un patrón para crear y ejecutar fácilmente pruebas de los trabajadores de Asset compute.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 205
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Prueba de un trabajador de Asset compute

El proyecto de Asset compute define un patrón para crear y ejecutar fácilmente [pruebas de los trabajadores de la Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## Anatomía de una prueba de trabajo

Las pruebas de los trabajadores de asset compute se dividen en grupos de pruebas y, dentro de cada grupo de pruebas, en uno o más casos de prueba que afirman una condición para probar.

La estructura de las pruebas en un proyecto de Asset compute es la siguiente:

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

Cada conversión de prueba puede tener los siguientes archivos:

+ `file.<extension>`
   + Archivo de origen para probar (la extensión puede ser cualquier cosa excepto `.link`)
   + Requerido
+ `rendition.<extension>`
   + Representación esperada
   + Necesario, excepto para la prueba de errores
+ `params.json`
   + Las instrucciones JSON de una sola representación
   + Opcional
+ `validate`
   + Secuencia de comandos que obtiene rutas de acceso de archivos de representación esperadas y reales como argumentos y que debe devolver el código de salida 0 si el resultado es correcto, o un código de salida distinto de cero si la validación o comparación fallan.
   + Opcional, toma el valor predeterminado de `diff` mando
   + Use un script de shell que ajuste un comando de ejecución de docker para usar diferentes herramientas de validación
+ `mock-<host-name>.json`
   + Respuestas HTTP con formato JSON para [burla de las llamadas de servicio externas](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Opcional, sólo se utiliza si el código de trabajo realiza sus propias solicitudes HTTP

## Escritura de un caso de prueba

Este caso de prueba confirma la entrada parametrizada (`params.json`) para el archivo de entrada (`file.jpg`) genera la representación PNG esperada (`rendition.png`).

1. En primer lugar, elimine el `simple-worker` caso de pruebas en `/test/asset-compute/simple-worker` como no es válido, como el trabajador ya no copia el origen en la representación.
1. Cree una nueva carpeta de caso de prueba en `/test/asset-compute/worker/success-parameterized` para probar una ejecución correcta del trabajador que genera una representación PNG.
1. En el `success-parameterized` carpeta, añadir la prueba [archivo de entrada](./assets/test/success-parameterized/file.jpg) para este caso de prueba y asígnele un nombre `file.jpg`.
1. En el `success-parameterized` carpeta, agregue un nuevo archivo llamado `params.json` que define los parámetros de entrada del trabajador:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Son las mismas claves o valores pasados a la variable [Definición del perfil de Asset compute de la herramienta de desarrollo](../develop/development-tool.md), menos el `worker` clave.

1. Añada el esperado [archivo de representación](./assets/test/success-parameterized/rendition.png) a este caso de prueba y asígnele un nombre `rendition.png`. Este archivo representa la salida esperada del trabajador para la entrada dada `file.jpg`.
1. Desde la línea de comandos, ejecute las pruebas en la raíz del proyecto ejecutando `aio app test`
   + Asegurar [Docker Desktop](../set-up/development-environment.md#docker) y compatibles: las imágenes de Docker se instalan y se inician
   + Finalice cualquier instancia de la herramienta de desarrollo en ejecución

![Prueba - Correcta ](./assets/test/success-parameterized/result.png)

## Escribir un caso de prueba de comprobación de errores

Este caso de prueba prueba prueba para garantizar que el trabajador emita el error correspondiente cuando la variable `contrast` El parámetro se ha establecido en un valor no válido.

1. Cree una nueva carpeta de caso de prueba en `/test/asset-compute/worker/error-contrast` para probar una ejecución errónea del trabajador debido a un `contrast` valor del parámetro.
1. En el `error-contrast` carpeta, añadir la prueba [archivo de entrada](./assets/test/error-contrast/file.jpg) para este caso de prueba y asígnele un nombre `file.jpg`. El contenido de este archivo no es material para esta prueba, solo necesita existir para pasar la comprobación de &quot;Fuente dañada&quot;, para llegar a la `rendition.instructions` comprobaciones de validez, que este caso de prueba valida.
1. En el `error-contrast` carpeta, agregue un nuevo archivo llamado `params.json` que define los parámetros de entrada del trabajador con el contenido:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Establecer `contrast` parámetros para `10`, un valor no válido, ya que el contraste debe estar entre -1 y 1, para producir un `RenditionInstructionsError`.
   + Afirmar que el error apropiado se produce en las pruebas estableciendo el `errorReason` clave para el &quot;motivo&quot; asociado con el error esperado. Este parámetro de contraste no válido genera el [error personalizado](../develop/worker.md#errors), `RenditionInstructionsError`, establezca la variable `errorReason` por el motivo de este error, o`rendition_instructions_error` para afirmar que se ha lanzado.

1. Dado que no se debe generar ninguna representación durante una ejecución errónea, no es necesario `rendition.<extension>` es necesario el archivo.
1. Ejecute el grupo de pruebas desde la raíz del proyecto ejecutando el comando `aio app test`
   + Asegurar [Docker Desktop](../set-up/development-environment.md#docker) y compatibles: las imágenes de Docker se instalan y se inician
   + Finalice cualquier instancia de la herramienta de desarrollo en ejecución

![Prueba: contraste del error](./assets/test/error-contrast/result.png)

## Casos de prueba en Github

Los casos de prueba finales están disponibles en Github en:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Solución de problemas

+ [No se ha generado ninguna representación durante la ejecución de la prueba](../troubleshooting.md#test-no-rendition-generated)
+ [La prueba genera una representación incorrecta](../troubleshooting.md#tests-generates-incorrect-rendition)
