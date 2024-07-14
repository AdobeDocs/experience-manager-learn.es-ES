---
title: Depuración de un trabajador de Asset compute
description: Los trabajadores de assets computes se pueden depurar de varias formas, desde simples sentencias de registro de depuración, pasando por el código VS adjunto como depurador remoto, hasta la extracción de registros para activaciones en Adobe I/O Runtime iniciadas desde AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 229
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Depuración de un trabajador de Asset compute

Los trabajadores de assets computes se pueden depurar de varias formas, desde simples sentencias de registro de depuración, pasando por el código VS adjunto como depurador remoto, hasta la extracción de registros para activaciones en Adobe I/O Runtime iniciadas desde AEM as a Cloud Service.

## Registro

La forma más básica de depurar los trabajadores de Asset compute usa las instrucciones `console.log(..)` tradicionales en el código de trabajo. El objeto JavaScript `console` es un objeto global implícito, por lo que no es necesario importarlo ni requerirlo, ya que siempre está presente en todos los contextos.

Estas instrucciones de registro están disponibles para revisarse de forma diferente en función de cómo se ejecute el trabajador de Asset compute:

+ Desde `aio app run`, los registros se imprimen hasta la salida estándar y los registros de activación de la [herramienta de desarrollo](../develop/development-tool.md)
  ![consola de ejecución de aplicación aio.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Desde `aio app test`, los registros se imprimen a `/build/test-results/test-worker/test.log`
  ![consola de prueba de aplicación aio.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Con `wskdebug`, las instrucciones de registro se imprimen en la consola de depuración de código de VS (Ver > Consola de depuración), salida estándar
  ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Con `aio app logs`, las instrucciones de registro se imprimen en la salida del registro de activación

## Depuración remota mediante el depurador adjunto

>[!WARNING]
>
>Use Microsoft Visual Studio Code 1.48.0 o posterior para comprobar la compatibilidad con wskdebug

El módulo [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm, admite la asociación de un depurador a los trabajadores de Asset compute, incluida la capacidad de establecer puntos de interrupción en código VS y recorrer paso a paso el código.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Clic en la depuración de un trabajador de Asset compute mediante wskdebug (sin audio)_

1. Asegúrese de que los módulos [wskdebug](../set-up/development-environment.md#wskdebug) y [ngrok](../set-up/development-environment.md#ngork) npm estén instalados
1. Asegúrese de que [Docker Desktop y las imágenes de Docker compatibles](../set-up/development-environment.md#docker) estén instalados y en ejecución
1. Cierre todas las instancias activas en ejecución de la herramienta de desarrollo.
1. Implemente el código más reciente utilizando `aio app deploy` y registre el nombre de la acción implementada (nombre entre `[...]`). Se usa para actualizar `launch.json` en el paso 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Iniciar una nueva instancia de la herramienta de desarrollo de Assets computes con el comando `npx adobe-asset-compute devtool`
1. En Código VS, pulse el icono Depurar en el panel de navegación izquierdo
   + Si se le solicita, pulse __crear un archivo launch.json > Node.js__ para crear un nuevo archivo `launch.json`.
   + De lo contrario, pulsa el icono __Engranaje__ a la derecha de la lista desplegable __Programa de lanzamiento__ para abrir el(la) `launch.json` existente(a) en el editor.
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
1. Pulse el botón verde __Ejecutar__ a la izquierda del menú desplegable __wskdebug__
1. Abra `/actions/worker/index.js` y pulse a la izquierda de los números de línea para agregar los puntos de interrupción 1. Vaya a la ventana del explorador web de la herramienta de desarrollo de Assets computes que se abrió en el paso 6
1. Pulse el botón __Ejecutar__ para ejecutar el trabajador
1. Vuelva al código VS, a `/actions/worker/index.js` y desplácese por el código
1. Para salir de la herramienta de desarrollo de depuración, pulse `Ctrl-C` en el terminal que ejecutó el comando `npx adobe-asset-compute devtool` en el paso 6

## Acceder a registros desde Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service aprovecha los Assets computes a través de los perfiles de procesamiento](../deploy/processing-profiles.md) invocándolos directamente en Adobe I/O Runtime. Debido a que estas invocaciones no implican desarrollo local, sus ejecuciones no se pueden depurar con herramientas locales como Herramienta de desarrollo de Assets computes o wskdebug. En su lugar, la CLI de Adobe I/O se puede utilizar para recuperar registros del trabajador ejecutado en un espacio de trabajo determinado en Adobe I/O Runtime.

1. Asegúrese de que las [variables de entorno específicas del espacio de trabajo](../deploy/runtime.md) se establecen a través de `AIO_runtime_namespace` y `AIO_runtime_auth`, en función del espacio de trabajo que requiere depuración.
1. Desde la línea de comandos, ejecute `aio app logs`
   + Si el área de trabajo está sufriendo mucho tráfico, expanda el número de registros de activación a través del indicador `--limit`:
     `$ aio app logs --limit=25`
1. Los registros de activaciones más recientes (hasta el `--limit` proporcionado) se devuelven como salida del comando para su revisión.

   ![registros de aplicaciones aio](./assets/debug/aio-app-logs.png)

## Resolución de problemas

+ [El depurador no se asocia](../troubleshooting.md#debugger-does-not-attach)
+ [Los puntos de interrupción no se pausan](../troubleshooting.md#breakpoints-no-pausing)
+ [El depurador de código VS no está adjunto](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Se adjuntó el depurador de código VS después de iniciar la ejecución del trabajador](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Se agota el tiempo de espera del trabajador durante depuración](../troubleshooting.md#worker-times-out-while-debugging)
+ [No se puede finalizar el proceso del depurador](../troubleshooting.md#cannot-terminate-debugger-process)
