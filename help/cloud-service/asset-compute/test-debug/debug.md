---
title: Depuración de un trabajador de Asset compute
description: Los trabajadores de asset compute se pueden depurar de varias formas, desde simples sentencias de registro de depuración, pasando por el código VS adjunto como depurador remoto, hasta la extracción de registros para activaciones en Adobe I/O Runtime AEM iniciadas desde el as a Cloud Service de la.
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# Depuración de un trabajador de Asset compute

Los trabajadores de asset compute se pueden depurar de varias formas, desde simples sentencias de registro de depuración, pasando por el código VS adjunto como depurador remoto, hasta la extracción de registros para activaciones en Adobe I/O Runtime AEM iniciadas desde el as a Cloud Service de la.

## Registro

La forma más básica de depurar los Assets computes de trabajo es la tradicional `console.log(..)` en el código de trabajador. El `console` El objeto JavaScript es un objeto global implícito, por lo que no es necesario importarlo ni requerirlo, ya que siempre está presente en todos los contextos.

Estas instrucciones de registro están disponibles para revisarlas de forma diferente en función de cómo se ejecute el trabajador de Asset compute:

+ Desde `aio app run`, los registros se imprimen en la salida estándar y la [Herramienta de desarrollo](../develop/development-tool.md) Registros de activación
   ![aplicación aio ejecutar console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Desde `aio app test`, los registros se imprimen en `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Uso de `wskdebug`, las instrucciones de registros se imprimen en la consola de depuración de código de VS (Ver > Consola de depuración), salida estándar
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Uso de `aio app logs`, las instrucciones de registro se imprimen en la salida del registro de activación

## Depuración remota mediante el depurador adjunto

>[!WARNING]
>
>Use Microsoft Visual Studio Code 1.48.0 o bueno para comprobar la compatibilidad con wskdebug

El [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm module, admite la asociación de un depurador a los Assets computes de trabajo, incluida la capacidad de establecer puntos de interrupción en el código VS y recorrer paso a paso el código.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Pulsación para depurar un trabajador de Asset compute mediante wskdebug (sin audio)_

1. Asegurar [wskdebug](../set-up/development-environment.md#wskdebug) y [ngrok](../set-up/development-environment.md#ngork) Los módulos npm están instalados
1. Asegurar [Docker Desktop y las imágenes compatibles de Docker](../set-up/development-environment.md#docker) están instalados y en ejecución
1. Cierre todas las instancias activas en ejecución de la herramienta de desarrollo.
1. Implemente el código más reciente utilizando `aio app deploy`  y registrar el nombre de la acción implementada (nombre entre las variables `[...]`). Se utiliza para actualizar el `launch.json` en el paso 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Inicie una nueva instancia de la herramienta de desarrollo de Asset compute con el comando `npx adobe-asset-compute devtool`
1. En Código VS, pulse el icono Depurar en el panel de navegación izquierdo
   + Si se le solicita, pulse __cree un archivo launch.json > Node.js__ para crear una nueva `launch.json` archivo.
   + De lo contrario, pulse el botón __Engranaje__ a la derecha del icono __Iniciar programa__ desplegable para abrir el existente `launch.json` en el editor.
1. Agregue la siguiente configuración de objeto JSON a `configurations` matriz:

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
1. Toca el verde __Ejecutar__ a la izquierda de __wskdebug__ desplegable
1. Abrir `/actions/worker/index.js` y pulse a la izquierda de los números de línea para agregar los puntos de interrupción 1. Vaya a la ventana del explorador web de la Herramienta de desarrollo de Assets computes que se abrió en el paso 6
1. Pulse el botón __Ejecutar__ para ejecutar el trabajador
1. Vaya de nuevo a Código VS, a `/actions/worker/index.js` y revise paso a paso el código
1. Para salir de la herramienta de desarrollo depurable, pulse `Ctrl-C` en el terminal que se ejecutó `npx adobe-asset-compute devtool` comando en el paso 6

## Acceder a registros desde Adobe I/O Runtime{#aio-app-logs}

[AEM El as a Cloud Service aprovecha los Assets computes de trabajo mediante Perfiles de procesamiento](../deploy/processing-profiles.md) al invocarlos directamente en Adobe I/O Runtime. Debido a que estas invocaciones no implican desarrollo local, sus ejecuciones no se pueden depurar con herramientas locales como Herramienta de desarrollo de Asset compute o wskdebug. En su lugar, la CLI de Adobe I/O se puede utilizar para recuperar registros del trabajador ejecutado en un espacio de trabajo determinado en Adobe I/O Runtime.

1. Asegúrese de que [variables de entorno específicas de workspace](../deploy/runtime.md) se configuran mediante `AIO_runtime_namespace` y `AIO_runtime_auth`, basado en el espacio de trabajo que requiere depuración.
1. Desde la línea de comandos, ejecute `aio app logs`
   + Si el espacio de trabajo está sufriendo mucho tráfico, amplíe el número de registros de activación mediante el `--limit` indicador:
      `$ aio app logs --limit=25`
1. El más reciente (hasta el `--limit`) los registros de activaciones se devuelven como salida del comando para su revisión.

   ![registros de aplicaciones aio](./assets/debug/aio-app-logs.png)

## Solución de problemas

+ [El depurador no se asocia](../troubleshooting.md#debugger-does-not-attach)
+ [Los puntos de interrupción no se pausan](../troubleshooting.md#breakpoints-no-pausing)
+ [El depurador de código VS no está adjunto](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Se adjuntó el depurador de código VS después de iniciar la ejecución del trabajador](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Se agota el tiempo de espera del trabajador durante depuración](../troubleshooting.md#worker-times-out-while-debugging)
+ [No se puede finalizar el proceso del depurador](../troubleshooting.md#cannot-terminate-debugger-process)
