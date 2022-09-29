---
title: Depurar un trabajador de Asset compute
description: Los trabajadores de asset compute se pueden depurar de varias maneras, desde simples sentencias de registro de depuración, pasando por el código VS adjunto como depurador remoto, hasta la extracción de registros para activaciones en Adobe I/O Runtime iniciadas desde AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# Depurar un trabajador de Asset compute

Los trabajadores de asset compute se pueden depurar de varias maneras, desde simples sentencias de registro de depuración, pasando por el código VS adjunto como depurador remoto, hasta la extracción de registros para activaciones en Adobe I/O Runtime iniciadas desde AEM as a Cloud Service.

## Registro

La forma más básica de depuración de los trabajadores de Asset compute es la tradicional `console.log(..)` en el código de trabajo. La variable `console` El objeto JavaScript es un objeto global implícito, por lo que no es necesario importarlo ni requerirlo, ya que siempre está presente en todos los contextos.

Estas instrucciones de registro están disponibles para su revisión de forma diferente en función de cómo se ejecuta el trabajador de Asset compute:

+ De `aio app run`, los registros se imprimen en el estándar out y la variable [Herramienta de desarrollo](../develop/development-tool.md) Registros de activación
   ![la aplicación aio ejecuta console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ De `aio app test`, registros imprimir en `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Uso `wskdebug`, registra las instrucciones impresas en la consola de depuración de código VS (Ver > Consola de depuración), salida estándar
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Uso `aio app logs`, las instrucciones de registro se imprimen en la salida del registro de activación

## Depuración remota mediante el depurador adjunto

>[!WARNING]
>
>Usar Microsoft Visual Studio Code 1.48.0 o bueno para compatibilidad con wskdebug

La variable [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) El módulo npm es compatible con la asociación de un depurador a los trabajadores de Asset compute, incluida la capacidad de establecer puntos de interrupción en el código VS y pasar por el código.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Pulsación para depurar un Asset compute de trabajo mediante wskdebug (sin audio)_

1. Asegúrese [wskdebug](../set-up/development-environment.md#wskdebug) y [ngrok](../set-up/development-environment.md#ngork) los módulos npm están instalados
1. Asegúrese [Docker Desktop y las imágenes de Docker compatibles](../set-up/development-environment.md#docker) están instalados y en ejecución
1. Cierre todas las instancias activas en ejecución de la herramienta de desarrollo.
1. Implemente el código más reciente utilizando `aio app deploy`  y registre el nombre de la acción implementada (nombre entre la variable `[...]`). Se usa para actualizar el `launch.json` en el paso 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Inicio de una nueva instancia de la herramienta de desarrollo de Asset compute mediante el comando `npx adobe-asset-compute devtool`
1. En Código VS, pulse el icono Depurar en el panel de navegación izquierdo
   + Si se le solicita, pulse __crear un archivo launch.json > Node.js__ para crear un `launch.json` archivo.
   + Si no, toque el __Engranaje__ a la derecha del __Programa de Launch__ para abrir el `launch.json` en el editor.
1. Agregue la siguiente configuración de objeto JSON a la `configurations` matriz:

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
1. Toque el verde __Ejecutar__ a la izquierda de __wskdebug__ lista desplegable
1. Apertura `/actions/worker/index.js` y pulse a la izquierda de los números de línea para añadir puntos de interrupción 1. Acceda a la ventana del explorador web de la herramienta de desarrollo de Asset compute que se abre en el paso 6
1. Toque . __Ejecutar__ para ejecutar el trabajador
1. Vuelva al código VS, para `/actions/worker/index.js` y paso a paso por el código
1. Para salir de la herramienta de desarrollo depurable, pulse `Ctrl-C` en el terminal que se ejecuta `npx adobe-asset-compute devtool` en el paso 6

## Acceso a los registros desde Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service aprovecha los trabajadores de Asset compute a través de Perfiles de procesamiento](../deploy/processing-profiles.md) invocarlos directamente en Adobe I/O Runtime. Dado que estas invocaciones no implican desarrollo local, sus ejecuciones no se pueden depurar con herramientas locales como Herramienta de desarrollo de Asset compute o wskdebug. En su lugar, la CLI de Adobe I/O se puede utilizar para recuperar registros del trabajador ejecutado en un espacio de trabajo en particular en Adobe I/O Runtime.

1. Asegúrese de que la variable [variables de entorno específicas del espacio de trabajo](../deploy/runtime.md) se establecen mediante `AIO_runtime_namespace` y `AIO_runtime_auth`, según el espacio de trabajo que requiera depuración.
1. Desde la línea de comandos, ejecute `aio app logs`
   + Si el espacio de trabajo está experimentando un tráfico intenso, expanda el número de registros de activación a través de la variable `--limit` indicador:
      `$ aio app logs --limit=25`
1. El más reciente (hasta el `--limit`) activations logs se devuelven como resultado del comando para su revisión.

   ![registros de aplicaciones de aio](./assets/debug/aio-app-logs.png)

## Solución de problemas

+ [Debugger no se adjunta](../troubleshooting.md#debugger-does-not-attach)
+ [Puntos de interrupción que no se pausan](../troubleshooting.md#breakpoints-no-pausing)
+ [El depurador de código VS no está adjunto](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Depurador de código VS adjunto después de iniciar la ejecución del trabajo](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Se agota el tiempo de espera del trabajo durante la depuración](../troubleshooting.md#worker-times-out-while-debugging)
+ [No se puede finalizar el proceso de depuración](../troubleshooting.md#cannot-terminate-debugger-process)
