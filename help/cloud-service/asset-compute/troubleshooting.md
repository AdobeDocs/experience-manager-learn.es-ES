---
title: Resolución de problemas de la extensibilidad del Asset compute para AEM Assets
description: A continuación se muestra un índice de problemas y errores comunes, junto con las resoluciones, que podrían producirse al desarrollar e implementar Assets computes personalizados para AEM Assets.
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---

# Resolución de problemas de extensibilidad del Asset compute

A continuación se muestra un índice de problemas y errores comunes, junto con las resoluciones, que podrían producirse al desarrollar e implementar Assets computes personalizados para AEM Assets.

## Desarrollar{#develop}

### La representación se devuelve parcialmente dibujada/dañada{#rendition-returned-partially-drawn-or-corrupt}

+ __Error__: La representación se procesa incompletamente (cuando una imagen está dañada) o no se puede abrir.

   ![La representación se devuelve parcialmente dibujada](./assets/troubleshooting/develop__await.png)

+ __Causa__: La  `renditionCallback` función del trabajador se cierra antes de que la representación se pueda escribir completamente en  `rendition.path`.
+ __Resolución__: Revise el código de trabajo personalizado y asegúrese de que todas las llamadas asincrónicas se realizan sincrónicamente mediante  `await`.

## Herramienta de desarrollo{#development-tool}

### Falta el archivo Console.json en el proyecto de Asset compute{#missing-console-json}

+ __Error:__ Error: Faltan archivos requeridos en la validación (.../node_module/@adobe/asset-compute-client/lib/integrationConfiguration.:XX:jsYY) en async setupAssetCompute (.../node_module/@adobe/asset-compute-devtool/src/assetComputeDevTool.:XX:jsYY)
+ __Causa:__ falta el  `console.json` archivo en la raíz del proyecto de Asset compute
+ __Resolución:__ Descargar un nuevo  `console.json` formulario del proyecto de Adobe I/O
   1. En console.adobe.io, abra el proyecto de Adobe I/O que el proyecto de Asset compute está configurado para usar
   1. Toque el botón __Download__ en la parte superior derecha
   1. Guarde el archivo descargado en la raíz del proyecto de Asset compute con el nombre de archivo `console.json`

### Sangría YAML incorrecta en manifest.yml{#incorrect-yaml-indentation}

+ __Error:__ YAMLException: sangría incorrecta de una entrada de asignación en la línea X, columna Y: (a través de standard out from  `aio app run` command)
+ __Causa:__ Los archivos de Yaml distinguen entre espacios en blanco, es probable que la sangría sea incorrecta.
+ __Solución:__ revise su  `manifest.yml` y asegúrese de que toda la sangría sea correcta.

### memorySize limit está configurado en demasiado bajo{#memorysize-limit-is-set-too-low}

+ __Error:__  OpenWhiskError del servidor de desarrollo local: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true HTTP 400 devuelto (solicitud incorrecta) —> &quot;El contenido de la solicitud tenía un formato incorrecto:requisito fallido: memoria 64 MB por debajo del umbral permitido de 134217728 B&quot;
+ __Causa:__ se  `memorySize` estableció un  `manifest.yml` límite para el trabajador en por debajo del umbral mínimo permitido, tal como indica el mensaje de error en bytes.
+ __Solución:__  revise los  `memorySize` límites en  `manifest.yml` y asegúrese de que son todos grandes con respecto al umbral mínimo permitido.

### La herramienta de desarrollo no se puede iniciar debido a la falta de private.key{#missing-private-key}

+ __Error:__ Error del servidor de desarrollo local: Faltan archivos necesarios en validatePrivateKeyFile.... (a través del comando estándar out de `aio app run`)
+ __Causa:__ el  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor del  `.env` archivo no apunta a  `private.key` o el usuario actual no  `private.key` puede leerlo.
+ __Solución:__ revise el  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor del  `.env` archivo y asegúrese de que contiene la ruta completa y absoluta a  `private.key` la del sistema de archivos.

### Lista desplegable de archivos de origen incorrecta{#source-files-dropdown-incorrect}

La herramienta de desarrollo de assets computes puede introducir un estado en el que extrae datos antiguos y es más visible en el menú desplegable __Source file__ que muestra elementos incorrectos.

+ __Error:__ la lista desplegable Archivo de origen muestra los elementos incorrectos.
+ __Causa:__ el estado del explorador almacenado en caché antiguo hace que la variable
+ __Solución:__ En el explorador, borre completamente el &quot;estado de la aplicación&quot; de la pestaña del explorador, la caché del explorador, el almacenamiento local y el trabajador del servicio.

### Falta el parámetro de consulta devToolToken o no es válido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Error: notificación__ &quot;no autorizada&quot; en la herramienta de desarrollo de Asset compute
+ __Causa:__ `devToolToken`  falta o no es válido
+ __Solución:__ cierre la ventana del explorador de la herramienta de desarrollo de Asset compute, finalice los procesos de la herramienta de desarrollo que se ejecuten iniciados mediante el  `aio app run` comando y reinicie la herramienta de desarrollo (con  `aio app run`).

### No se pueden quitar los archivos de origen{#unable-to-remove-source-files}

+ __Error:__ no hay forma de eliminar los archivos de origen añadidos de la interfaz de usuario de las herramientas de desarrollo
+ __Causa:__ esta funcionalidad no se ha implementado
+ __Solución:__ inicie sesión en el proveedor de almacenamiento en la nube con las credenciales definidas en  `.env`. Busque el contenedor utilizado por las herramientas de desarrollo (también especificadas en `.env`), navegue hasta la carpeta __source__ y elimine las imágenes de origen. Es posible que tenga que realizar los pasos descritos en la lista desplegable [Source files grepldown incorrecto](#source-files-dropdown-incorrect) si los archivos de origen eliminados siguen mostrándose en la lista desplegable, ya que se pueden almacenar en caché localmente en el &quot;estado de aplicación&quot; de las herramientas de desarrollo.

   ![Almacenamiento de blob de Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Probar{#test}

### No se genera ninguna representación durante la ejecución de la prueba{#test-no-rendition-generated}

+ __Error:__ error: No se generó ninguna representación.
+ __Causa:__ el trabajador no pudo generar una representación debido a un error inesperado, como un error de sintaxis de JavaScript.
+ __Solución:__ revise la ejecución de la prueba  `test.log` en  `/build/test-results/test-worker/test.log`. Busque la sección en este archivo correspondiente al caso de prueba fallido y revise si hay errores.

   ![Solución de problemas: no se genera ninguna representación](./assets/troubleshooting/test__no-rendition-generated.png)

### La prueba genera una representación incorrecta, lo que provoca que la prueba falle{#tests-generates-incorrect-rendition}

+ __Error:__ error: La representación &quot;rendition.xxx&quot; no se ajusta a lo esperado.
+ __Causa:__ el trabajador genera una representación que no es la misma que la  `rendition.<extension>` proporcionada en el caso de prueba.
   + Si el archivo `rendition.<extension>` esperado no se crea de la misma manera que la representación generada localmente en el caso de prueba, la prueba puede fallar ya que puede haber alguna diferencia en los bits. Por ejemplo, si el programa de trabajo del Asset compute cambia el contraste mediante API y el resultado esperado se crea ajustando el contraste en Adobe Photoshop CC, los archivos pueden aparecer del mismo modo, pero las variaciones menores en los bits pueden ser diferentes.
+ __Solución:__ revise la salida de representación de la prueba navegando hasta  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` y compárela con el archivo de representación esperado en el caso de prueba. Para crear un recurso esperado exacto, haga lo siguiente:
   + Utilice la herramienta de desarrollo para generar una representación, validar que sea correcta y utilizarla como el archivo de representación esperado
   + O bien, valide el archivo generado por la prueba en `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, valide que es correcto y utilícelo como el archivo de representación esperado

## Depurar

### Debugger no se adjunta{#debugger-does-not-attach}

+ __Error__: Error al procesar el inicio: Error: No se pudo conectar para depurar el destino en...
+ __Causa__: Docker Desktop no se está ejecutando en el sistema local. Para verificarlo, revise la consola de depuración de código de VS (Ver > Consola de depuración), confirmando este error.
+ __Resolución__: Inicie  [Docker Desktop y confirme que están instaladas](./set-up/development-environment.md#docker) las imágenes de Docker necesarias.

### Puntos de interrupción que no se pausan{#breakpoints-no-pausing}

+ __Error__: Al ejecutar el programa de trabajo de Asset compute desde la herramienta de desarrollo depurable, el código VS no se pausa en los puntos de interrupción.

#### El depurador de código VS no está adjunto{#vs-code-debugger-not-attached}

+ __Causa:__ el depurador de código VS se detuvo/desconectó.
+ __Solución:__ reinicie el depurador de código VS y verifique que se adjunte mirando la consola de salida de depuración de código VS (Ver > Consola de depuración)

#### Depurador de código VS adjunto después de iniciar la ejecución del trabajo{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ el depurador de código VS no se adjuntó antes de pulsar la  ____ Herramienta de desarrollo de ejecución.
+ __Solución:__ asegúrese de que el depurador se ha adjuntado revisando la consola de depuración de VS Code (Ver > Consola de depuración) y, a continuación, vuelva a ejecutar el programa de trabajo de Asset compute desde la herramienta de desarrollo.

### Se agota el tiempo de espera del trabajo durante la depuración{#worker-times-out-while-debugging}

+ __Error__: La consola de depuración informa de que la acción agotará el tiempo de espera en -XXX milisegundos&quot; o de que la vista previa de la  [herramienta de desarrollo de Asset compute ](./develop/development-tool.md) gira indefinidamente o
+ __Causa__: Se ha superado el tiempo de espera de trabajador definido en  [manifest.](./develop/manifest.md) ymlis durante la depuración.
+ __Resolución__: Aumente temporalmente el tiempo de espera del trabajador en  [manifest.](./develop/manifest.md) ymlor acelere las actividades de depuración.

### No se puede finalizar el proceso de depuración{#cannot-terminate-debugger-process}

+ __Error__:  `Ctrl-C` en la línea de comandos no finaliza el proceso de depuración (`npx adobe-asset-compute devtool`).
+ __Causa__: Un error en  `@adobe/aio-cli-plugin-asset-compute` 1.3.x  `Ctrl-C` no se reconoce como un comando de terminación.
+ __Resolución__: Actualización  `@adobe/aio-cli-plugin-asset-compute` a la versión 1.4.1+

   ```
   $ aio update
   ```

   ![Solución de problemas: actualización de aio](./assets/troubleshooting/debug__terminate.png)

## Implementar{#deploy}

### Falta una representación personalizada en el recurso en AEM{#custom-rendition-missing-from-asset}

+ __Error:__ los recursos nuevos y reprocesados se procesan correctamente, pero no tienen la representación personalizada

#### Perfil de procesamiento no aplicado a la carpeta antecesora

+ __Causa:__ el recurso no existe en una carpeta con el perfil de procesamiento que utiliza el trabajador personalizado
+ __Resolución:__ Aplicar el perfil de procesamiento a una carpeta antecesora del recurso

#### Perfil de procesamiento reemplazado por perfil de procesamiento inferior

+ __Causa:__ el recurso existe debajo de una carpeta con el perfil de procesamiento de trabajador personalizado aplicado, pero se ha aplicado un perfil de procesamiento diferente que no utiliza el trabajador del cliente entre esa carpeta y el recurso.
+ __Solución:__ combine o reconcilie los dos perfiles de procesamiento y elimine el perfil de procesamiento intermedio

### El procesamiento de recursos falla en AEM{#asset-processing-fails}

+ __Error:__ se muestra el distintivo Error en el procesamiento de recursos en el recurso
+ __Causa:__ error en la ejecución del trabajador personalizado
+ __Solución:__ siga las instrucciones sobre la  [depuración de la ](./test-debug/debug.md#aio-app-logs) activación de Adobe I/O Runtime mediante  `aio app logs`.
