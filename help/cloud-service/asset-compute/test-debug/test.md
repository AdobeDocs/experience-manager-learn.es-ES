---
title: Probar un trabajador de cálculo de recursos
description: El proyecto de cómputo de recursos define un patrón para crear y ejecutar fácilmente pruebas de los trabajadores de cómputo de recursos.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---


# Probar un trabajador de cálculo de recursos

El proyecto de cómputo de recursos define un patrón para crear y ejecutar fácilmente [pruebas de trabajadores](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)de computación de recursos.

## Anatomía de una prueba de trabajo

Las pruebas de los trabajadores de Asset Compute se desglosan en grupos de pruebas y, dentro de cada grupo de pruebas, uno o más casos de prueba que afirman una condición para probar.

La estructura de las pruebas en un proyecto de Asset Compute es la siguiente:

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

Cada serie de pruebas puede tener los siguientes archivos:

+ `file.<extension>`
   + Archivo de origen que probar (la extensión puede ser cualquier cosa excepto `.link`)
   + Requerido
+ `rendition.<extension>`
   + Representación esperada
   + Obligatorio, excepto para la prueba de errores
+ `params.json`
   + Instrucciones JSON de representación única
   + Opcional
+ `validate`
   + Una secuencia de comandos que obtiene rutas de archivo de representación esperadas y reales como argumentos y debe devolver el código de salida 0 si el resultado es correcto, o un código de salida distinto de cero si falla la validación o comparación.
   + Opcional, el valor predeterminado es el `diff` comando
   + Utilice una secuencia de comandos de shell que ajuste un comando de ejecución de docker para utilizar distintas herramientas de validación
+ `mock-<host-name>.json`
   + Respuestas HTTP con formato JSON para [burlarse de las llamadas](https://www.mock-server.com/mock_server/creating_expectations.html)de servicio externas.
   + Opcional, solo se utiliza si el código de trabajo realiza solicitudes HTTP propias

## Escritura de un caso de prueba

Este caso de prueba confirma la entrada parametrizada (`params.json`) para el archivo de entrada (`file.jpg`) genera la representación PNG esperada (`rendition.png`).

1. Primero elimine el caso de `simple-worker` pruebas generadas automáticamente `/test/asset-compute/simple-worker` porque no es válido, ya que nuestro trabajador ya no copia simplemente el origen en la representación.
1. Cree una nueva carpeta de casos de prueba en `/test/asset-compute/worker/success-parameterized` para probar la ejecución correcta del programa de trabajo que genera una representación PNG.
1. En la `success-parameterized` carpeta, agregue el archivo [de](./assets/test/success-parameterized/file.jpg) entrada de prueba para este caso de prueba y asígnele un nombre `file.jpg`.
1. En la `success-parameterized` carpeta, agregue un nuevo archivo denominado `params.json` que defina los parámetros de entrada del programa de trabajo:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   Son las mismas claves o valores pasados a la definición [de perfil de cálculo de recursos de la herramienta](../develop/development-tool.md)de desarrollo, menos la `worker` clave.
1. Añada el archivo [de](./assets/test/success-parameterized/rendition.png) representación esperado a este caso de prueba y asígnele un nombre `rendition.png`. Este archivo representa la salida esperada del programa de trabajo para la entrada determinada `file.jpg`.
1. Desde la línea de comandos, ejecute las pruebas en la raíz del proyecto mediante la ejecución `aio app test`
   + Asegúrese de que [Docker Desktop](../set-up/development-environment.md#docker) y las imágenes de acoplamiento compatibles están instaladas e iniciadas
   + Finalización de las instancias de la herramienta de desarrollo en ejecución

![Prueba - Éxito ](./assets/test/success-parameterized/result.png)

## Escritura de un caso de prueba de comprobación de errores

Este caso de prueba se prueba para garantizar que el programa de trabajo arroja el error adecuado cuando el `contrast` parámetro se establece en un valor no válido.

1. Cree una nueva carpeta de casos de prueba en `/test/asset-compute/worker/error-contrast` para probar una ejecución de error del programa de trabajo debido a un valor de `contrast` parámetro no válido.
1. En la `error-contrast` carpeta, agregue el archivo [de](./assets/test/error-contrast/file.jpg) entrada de prueba para este caso de prueba y asígnele un nombre `file.jpg`. El contenido de este archivo no es importante para esta prueba, sólo necesita existir para superar la comprobación de &quot;Origen dañado&quot;, para alcanzar las comprobaciones de validez, que este caso de prueba valida. `rendition.instructions`
1. En la `error-contrast` carpeta, agregue un nuevo archivo denominado `params.json` que defina los parámetros de entrada del trabajo con el contenido:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Establezca `contrast` parámetros en `10`, un valor no válido, ya que el contraste debe estar entre -1 y 1, para generar un `RenditionInstructionsError`.
   + Afirmemos que se produce el error adecuado en las pruebas configurando la `errorReason` clave en el &quot;motivo&quot; asociado al error esperado. Este parámetro de contraste no válido emite el error [](../develop/worker.md#errors)personalizado, `RenditionInstructionsError`por lo tanto establezca el `errorReason` en el motivo del error o`rendition_instructions_error` para afirmar que se ha generado.

1. Dado que no debe generarse ninguna representación durante la ejecución de un error, no es necesario ningún `rendition.<extension>` archivo.
1. Ejecute el grupo de pruebas desde la raíz del proyecto ejecutando el comando `aio app test`
   + Asegúrese de que [Docker Desktop](../set-up/development-environment.md#docker) y las imágenes de acoplamiento compatibles están instaladas e iniciadas
   + Finalización de las instancias de la herramienta de desarrollo en ejecución

![Prueba - Contraste de errores](./assets/test/error-contrast/result.png)

## Casos de prueba en Github

Los casos de prueba finales están disponibles en Github en:

+ [aem-guide-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Solución de problemas

### No se ha generado ninguna representación

El caso de prueba falla sin generar una representación.

+ __Error:__ Error: No se generó ninguna representación.
+ __Causa:__ El programa de trabajo no pudo generar una representación debido a un error inesperado, como un error de sintaxis de JavaScript.
+ __Resolución:__ Revise la ejecución de la prueba `test.log` en `/build/test-results/test-worker/test.log`. Busque la sección de este archivo correspondiente al caso de prueba fallido y revise los errores.

   ![Resolución de problemas: no se genera ninguna representación](./assets/test/troubleshooting__no-rendition-generated.png)

### La prueba genera una representación incorrecta

El caso de prueba no puede generar una representación incorrecta.

+ __Error:__ Error: La representación &#39;rendition.xxx&#39; no es la esperada.
+ __Causa:__ El programa de trabajo genera una representación que no es la misma que la `rendition.<extension>` proporcionada en el caso de prueba.
   + Si el `rendition.<extension>` archivo esperado no se crea de la misma manera que la representación generada localmente en el caso de prueba, la prueba puede fallar, ya que puede haber alguna diferencia en los bits. Si la representación esperada en el caso de prueba se guarda desde la herramienta de desarrollo, es decir, se genera en Adobe I/O Runtime, los bits técnicamente pueden ser diferentes, lo que provoca que la prueba falle, incluso si, desde una perspectiva humana, los archivos de representación esperados y reales son idénticos.
+ __Resolución:__ Revise el resultado de la representación de la prueba navegando hasta `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`y compárelo con el archivo de representación esperado en el caso de prueba.
