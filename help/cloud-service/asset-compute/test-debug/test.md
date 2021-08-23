---
title: Probar un Asset compute de trabajo
description: El proyecto Asset compute define un patrón para crear y ejecutar fácilmente pruebas de los trabajadores del Asset compute.
feature: Microservicios de asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: Integraciones, desarrollo
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 0%

---


# Probar un Asset compute de trabajo

El proyecto de Asset compute define un patrón para crear y ejecutar fácilmente [pruebas de Asset compute Workers](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## Anatomía de una prueba de trabajador

Las pruebas de los trabajadores de asset compute se desglosan en grupos de pruebas y, en cada grupo de pruebas, uno o más casos de prueba que afirman una condición para probar.

La estructura de las pruebas de un proyecto de Asset compute es la siguiente:

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
   + Archivo de origen a probar (la extensión puede ser cualquier cosa excepto `.link`)
   + Requerido
+ `rendition.<extension>`
   + Representación esperada
   + Requerido, excepto para la prueba de errores
+ `params.json`
   + Instrucciones JSON de representación única
   + Opcional
+ `validate`
   + Una secuencia de comandos que obtiene rutas de archivo de representación esperadas y reales como argumentos y debe devolver el código de salida 0 si el resultado es correcto, o un código de salida distinto de cero si la validación o la comparación fallan.
   + Opcional, de forma predeterminada se usa el comando `diff`
   + Utilice un script shell que ajuste un comando docker run para utilizar distintas herramientas de validación
+ `mock-<host-name>.json`
   + Respuestas HTTP con formato JSON para [burlas a las llamadas de servicio externas](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Opcional, solo se utiliza si el código de trabajo realiza solicitudes HTTP propias

## Escritura de un caso de prueba

Este caso de prueba afirma la entrada parametrizada (`params.json`) para el archivo de entrada (`file.jpg`) genera la representación PNG esperada (`rendition.png`).

1. Primero elimine el caso de pruebas `simple-worker` generadas automáticamente en `/test/asset-compute/simple-worker` ya que no es válido, ya que nuestro trabajador ya no copia el origen simplemente en la representación.
1. Cree una nueva carpeta de casos de prueba en `/test/asset-compute/worker/success-parameterized` para probar una ejecución correcta del trabajador que genera una representación PNG.
1. En la carpeta `success-parameterized`, añada el archivo de entrada [prueba](./assets/test/success-parameterized/file.jpg) para este caso de prueba y asígnele el nombre `file.jpg`.
1. En la carpeta `success-parameterized`, añada un nuevo archivo llamado `params.json` que defina los parámetros de entrada del trabajador:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Son las mismas claves o valores pasados a la definición de perfil de Asset compute](../develop/development-tool.md) de la `worker` herramienta de desarrollo, menos a la clave [.

1. Agregue el archivo de representación [esperado](./assets/test/success-parameterized/rendition.png) a este caso de prueba y asígnele el nombre `rendition.png`. Este archivo representa la salida esperada del trabajador para la entrada determinada `file.jpg`.
1. Desde la línea de comandos, ejecute las pruebas en la raíz del proyecto ejecutando `aio app test`
   + Asegúrese de que [Docker Desktop](../set-up/development-environment.md#docker) y las imágenes de Docker compatibles estén instaladas e iniciadas
   + Finalización de cualquier instancia de herramienta de desarrollo en ejecución

![Prueba: Correcto  ](./assets/test/success-parameterized/result.png)

## Escritura de un caso de prueba de comprobación de errores

Este caso de prueba comprueba que el trabajador genera el error apropiado cuando el parámetro `contrast` está establecido en un valor no válido.

1. Cree una nueva carpeta de casos de prueba en `/test/asset-compute/worker/error-contrast` para probar una ejecución errónea del trabajador debido a un valor de parámetro `contrast` no válido.
1. En la carpeta `error-contrast`, añada el archivo de entrada [prueba](./assets/test/error-contrast/file.jpg) para este caso de prueba y asígnele el nombre `file.jpg`. El contenido de este archivo no es relevante para esta prueba, solo necesita existir para superar la comprobación &quot;Origen dañado&quot;, para alcanzar las `rendition.instructions` comprobaciones de validez que este caso de prueba valida.
1. En la carpeta `error-contrast`, añada un nuevo archivo llamado `params.json` que defina los parámetros de entrada del trabajo con el contenido:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Defina los parámetros `contrast` en `10`, un valor no válido, ya que el contraste debe estar entre -1 y 1, para generar un `RenditionInstructionsError`.
   + Asume que se produce el error apropiado en las pruebas configurando la clave `errorReason` en el &quot;motivo&quot; asociado con el error esperado. Este parámetro de contraste no válido lanza el [error personalizado](../develop/worker.md#errors), `RenditionInstructionsError`, por lo tanto establece el `errorReason` en el motivo del error, o`rendition_instructions_error` para afirmar que se ha generado.

1. Dado que no se debe generar ninguna representación durante una ejecución errónea, no es necesario ningún archivo `rendition.<extension>`.
1. Ejecute el grupo de pruebas desde la raíz del proyecto ejecutando el comando `aio app test`
   + Asegúrese de que [Docker Desktop](../set-up/development-environment.md#docker) y las imágenes de Docker compatibles estén instaladas e iniciadas
   + Finalización de cualquier instancia de herramienta de desarrollo en ejecución

![Prueba: contraste de error](./assets/test/error-contrast/result.png)

## Casos de prueba en Github

Los casos de prueba finales están disponibles en Github en:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Solución de problemas

+ [No se genera ninguna representación durante la ejecución de la prueba](../troubleshooting.md#test-no-rendition-generated)
+ [La prueba genera una representación incorrecta](../troubleshooting.md#tests-generates-incorrect-rendition)
