---
title: Depurar un trabajador de Asset compute
description: Los trabajadores de asset compute se pueden depurar de varias maneras, desde simples sentencias de registro de depuración, pasando por el código VS adjunto como depurador remoto, hasta la extracción de registros para activaciones en Adobe I/O Runtime iniciadas desde AEM como Cloud Service.
feature: Microservicios de asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integraciones, desarrollo
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '623'
ht-degree: 0%

---


# Depurar un trabajador de Asset compute

Los trabajadores de asset compute se pueden depurar de varias maneras, desde simples sentencias de registro de depuración, pasando por el código VS adjunto como depurador remoto, hasta la extracción de registros para activaciones en Adobe I/O Runtime iniciadas desde AEM como Cloud Service.

## Registro

La forma más básica de depuración de los Assets computes de trabajo utiliza las instrucciones tradicionales `console.log(..)` en el código de trabajo. El objeto JavaScript `console` es un objeto global implícito, por lo que no es necesario importarlo ni requerirlo, ya que siempre está presente en todos los contextos.

Estas instrucciones de registro están disponibles para su revisión de forma diferente en función de cómo se ejecuta el trabajador de Asset compute:

+ Desde `aio app run`, los registros se imprimen en el modo estándar y los [Registros de activación de la ](../develop/development-tool.md) herramienta de desarrollo
   ![la aplicación aio ejecuta console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Desde `aio app test`, los registros se imprimen en `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Con `wskdebug`, las instrucciones de registro se imprimen en la consola de depuración de código de VS (Ver > Consola de depuración), estándar
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Con `aio app logs`, las instrucciones de registro se imprimen en la salida del registro de activación

## Depuración remota mediante el depurador adjunto

>[!WARNING]
>
>Usar Microsoft Visual Studio Code 1.48.0 o bueno para compatibilidad con wskdebug

El módulo npm [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) admite la adjuntación de un depurador a los trabajadores de Asset compute, incluida la capacidad de establecer puntos de interrupción en el código VS y pasar por el código.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Pulsación para depurar un Asset compute de trabajo mediante wskdebug (sin audio)_

1. Asegúrese de que los módulos npm [wskdebug](../set-up/development-environment.md#wskdebug) y [ngrok](../set-up/development-environment.md#ngork) estén instalados
1. Asegúrese de que [Docker Desktop y las imágenes de Docker](../set-up/development-environment.md#docker) compatibles estén instaladas y en ejecución
1. Cierre todas las instancias activas en ejecución de la herramienta de desarrollo.
1. Implemente el código más reciente utilizando `aio app deploy` y registre el nombre de la acción implementada (nombre entre `[...]`). Se utilizará para actualizar el `launch.json` en el paso 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```

1. Inicie una nueva instancia de la herramienta de desarrollo de Asset compute mediante el comando `npx adobe-asset-compute devtool`
1. En Código VS, pulse el icono de depuración en el panel de navegación izquierdo
   + Si se le solicita, pulse __crear un archivo launch.json > Node.js__ para crear un nuevo archivo `launch.json`.
   + De lo contrario, pulse el icono __Engranaje__ a la derecha del menú desplegable __Iniciar Programa__ para abrir el `launch.json` existente en el editor.
1. Agregue la siguiente configuración de objeto JSON a la matriz `configurations`:

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. Seleccione el nuevo __wskdebug__ en la lista desplegable
1. Pulse el botón verde __Run__ a la izquierda del menú desplegable __wskdebug__
1. Abra `/actions/worker/index.js` y pulse a la izquierda de los números de línea para añadir puntos de interrupción 1. Acceda a la ventana del explorador web de la herramienta de desarrollo de Asset compute que se abre en el paso 6
1. Pulse el botón __Ejecutar__ para ejecutar el trabajador
1. Vuelva al código VS, vaya a `/actions/worker/index.js` y siga los pasos del código
1. Para salir de la herramienta de desarrollo depurable, pulse `Ctrl-C` en el terminal que ejecuta el comando `npx adobe-asset-compute devtool` en el paso 6

## Acceso a los registros desde Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service aprovecha los trabajadores de Asset compute a través de ](../deploy/processing-profiles.md) perfiles de procesamiento al invocarlos directamente en Adobe I/O Runtime. Dado que estas invocaciones no implican desarrollo local, sus ejecuciones no se pueden depurar con herramientas locales como Herramienta de desarrollo de Asset compute o wskdebug. En su lugar, la CLI de Adobe I/O se puede utilizar para recuperar registros del trabajador ejecutado en un espacio de trabajo en particular en Adobe I/O Runtime.

1. Asegúrese de que las [variables de entorno específicas del espacio de trabajo](../deploy/runtime.md) se establecen mediante `AIO_runtime_namespace` y `AIO_runtime_auth`, según el espacio de trabajo que requiera depuración.
1. Desde la línea de comandos, ejecute `aio app logs`
   + Si el espacio de trabajo está experimentando un tráfico intenso, expanda el número de registros de activación mediante el indicador `--limit` :
      `$ aio app logs --limit=25`
1. Los registros de activaciones más recientes (hasta el `--limit`) proporcionados se devolverán como resultado del comando para su revisión.

   ![registros de aplicaciones de aio](./assets/debug/aio-app-logs.png)

## Solución de problemas

+ [Debugger no se adjunta](../troubleshooting.md#debugger-does-not-attach)
+ [Puntos de interrupción que no se pausan](../troubleshooting.md#breakpoints-no-pausing)
+ [El depurador de código VS no está adjunto](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Depurador de código VS adjunto después de iniciar la ejecución del trabajo](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Se agota el tiempo de espera del trabajo durante la depuración](../troubleshooting.md#worker-times-out-while-debugging)
+ [No se puede finalizar el proceso de depuración](../troubleshooting.md#cannot-terminate-debugger-process)
