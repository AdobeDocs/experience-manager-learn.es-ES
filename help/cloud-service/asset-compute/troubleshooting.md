---
title: Resolución de problemas de extensibilidad de Asset compute para AEM Assets
description: A continuación se muestra un índice de problemas y errores comunes, junto con las resoluciones, que podrían encontrarse al desarrollar e implementar los trabajadores de Asset compute personalizados para AEM Assets.
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 649d971ecaa67c0d1dd2636f3c212bfee3d13561
workflow-type: tm+mt
source-wordcount: '1241'
ht-degree: 0%

---


# Resolución de problemas de extensibilidad de Assets computes

A continuación se muestra un índice de problemas y errores comunes, junto con las resoluciones, que podrían encontrarse al desarrollar e implementar los trabajadores de Asset compute personalizados para AEM Assets.

## Desarrollar{#develop}

### La representación se devuelve parcialmente dibujada/dañada{#rendition-returned-partially-drawn-or-corrupt}

+ __Error__: La representación se procesa de forma incompleta (cuando una imagen está dañada) o no se puede abrir.

   ![La representación se devuelve parcialmente dibujada](./assets/troubleshooting/develop__await.png)

+ __Causa__: La  `renditionCallback` función del trabajador se está cerrando antes de que se pueda escribir completamente en la representación  `rendition.path`.
+ __Resolución__: Revise el código de trabajo personalizado y asegúrese de que todas las llamadas asincrónicas se hacen sincrónicas mediante  `await`.

## Herramienta de desarrollo{#development-tool}

### Falta el archivo Console.json en el proyecto de Asset compute{#missing-console-json}

+ __Error:__ Error: Faltan los archivos necesarios en la validación (.../node_module/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) en async setupAssetCompute (.../node_module/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __Causa:__ Falta el  `console.json` archivo en la raíz del proyecto de Asset compute
+ __Resolución:__ Descargar un nuevo  `console.json` formulario del proyecto de Adobe I/O
   1. En console.adobe.io, abra el proyecto de Adobe I/O que el proyecto de Asset compute está configurado para usar
   1. Toque el botón __Descargar__ en la parte superior derecha
   1. Guarde el archivo descargado en la raíz del proyecto de Asset compute utilizando el nombre de archivo `console.json`

### Sangría YAML incorrecta en manifest.yml{#incorrect-yaml-indentation}

+ __Error:__ YAMLException: sangría incorrecta de una entrada de asignación en la línea X, columna Y: (mediante salida estándar desde el  `aio app run` comando)
+ __Causa:Los archivos__ Yaml distinguen entre espacios en blanco, es probable que la sangría sea incorrecta.
+ __Resolución:__ revise la sangría  `manifest.yml` y asegúrese de que la sangría es correcta.

### el límite de memorySize está establecido en demasiado bajo{#memorysize-limit-is-set-too-low}

+ __Error:__  Local Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true HTTP 400 devuelto (solicitud incorrecta) —> &quot;El contenido de la solicitud tenía un formato incorrecto:error de requisito: memoria 64 MB por debajo del umbral permitido de 134217728 B&quot;
+ __Causa:__ Se  `memorySize` estableció un  `manifest.yml` límite para el trabajador por debajo del umbral mínimo permitido tal como se indica en el mensaje de error en bytes.
+ __Resolución:__  Revise los  `memorySize` límites en la  `manifest.yml` y asegúrese de que son todos grandes por encima del umbral mínimo permitido.

### La herramienta de desarrollo no puede inicio debido a la falta de private.key{#missing-private-key}

+ __Error:__ Local Dev ServerError: Faltan los archivos necesarios en validatePrivateKeyFile.... (mediante el comando estándar out de `aio app run`)
+ __Causa:__ El  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor del  `.env` archivo no señala  `private.key` o no  `private.key` es legible por el usuario actual.
+ __Resolución:__ Revise el  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor del  `.env` archivo y asegúrese de que contiene la ruta completa y absoluta al  `private.key` archivo del sistema de archivos.

### Lista desplegable de archivos de origen incorrecta{#source-files-dropdown-incorrect}

La herramienta de desarrollo de assets computes puede ingresar un estado en el que extrae datos antiguos y se nota más en la lista desplegable __Archivo de origen__ que muestra elementos incorrectos.

+ __Error:el menú desplegable__ del archivo de origen muestra elementos incorrectos.
+ __Causa:el estado__ del explorador en caché antiguo provoca la aparición de
+ __Resolución:__ En el navegador, borre completamente el &quot;estado de la aplicación&quot; de la ficha del navegador, la caché del navegador, el almacenamiento local y el trabajador del servicio.

### Falta el parámetro de consulta devToolToken o no es válido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Error:__  notificación &quot;no autorizada&quot; en la herramienta de desarrollo de Assets computes
+ __Causa:__ `devToolToken` falta o no es válida
+ __Resolución:__ cierre la ventana del navegador de la herramienta de desarrollo de Assets computes, finalice los procesos de la herramienta de desarrollo que se ejecuten iniciados mediante el  `aio app run` comando y vuelva a utilizar la herramienta de desarrollo de inicios (mediante  `aio app run`).

### No se pueden quitar los archivos de origen{#unable-to-remove-source-files}

+ __Error:no__ hay forma de eliminar los archivos de origen agregados de la interfaz de usuario de las herramientas de desarrollo
+ __Causa:__ Esta funcionalidad no se ha implementado
+ __Resolución:__ inicie sesión en el proveedor de almacenamiento de nube con las credenciales definidas en  `.env`. Busque el contenedor utilizado por las Herramientas de desarrollo (también especificado en `.env`), navegue hasta la carpeta __source__ y elimine las imágenes de origen. Es posible que tenga que realizar los pasos descritos en la lista desplegable [Archivos de origen incorrectos](#source-files-dropdown-incorrect) si los archivos de origen eliminados siguen mostrándose en la lista desplegable, ya que se pueden almacenar en caché localmente en el &quot;estado de la aplicación&quot; de las Herramientas de desarrollo.

   ![Almacenamiento de blob de Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Probar{#test}

### No se generó ninguna representación durante la ejecución de la prueba{#test-no-rendition-generated}

+ __Error:__ Error: No se generó ninguna representación.
+ __Causa:__ El programa de trabajo no pudo generar una representación debido a un error inesperado, como un error de sintaxis de JavaScript.
+ __Resolución:__ Revise la ejecución de la prueba  `test.log` en  `/build/test-results/test-worker/test.log`. Busque la sección de este archivo correspondiente al caso de prueba fallido y revise los errores.

   ![Resolución de problemas: no se genera ninguna representación](./assets/troubleshooting/test__no-rendition-generated.png)

### La prueba genera una representación incorrecta, lo que provoca que la prueba falle{#tests-generates-incorrect-rendition}

+ __Error:__ Error: La representación &#39;rendition.xxx&#39; no es la esperada.
+ __Causa:__ El programa de trabajo genera una representación que no es la misma que la  `rendition.<extension>` proporcionada en el caso de prueba.
   + Si el archivo `rendition.<extension>` esperado no se crea de la misma manera que la representación generada localmente en el caso de prueba, la prueba puede fallar, ya que puede haber alguna diferencia en los bits. Por ejemplo, si el programa de trabajo de Asset compute cambia el contraste mediante API y el resultado esperado se crea ajustando el contraste en Adobe Photoshop CC, los archivos pueden aparecer del mismo modo, pero las variaciones menores en los bits pueden ser diferentes.
+ __Resolución:__ Revise la salida de representación de la prueba navegando hasta  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`y compárela con el archivo de representación esperado en el caso de prueba. Para crear un recurso esperado exacto, haga lo siguiente:
   + Utilice la herramienta de desarrollo para generar una representación, validar que sea correcta y utilizarla como archivo de representación esperado
   + O bien, valide el archivo generado por la prueba en `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, valide que es correcto y utilícelo como archivo de representación esperado

## Depurar

### El depurador no se adjunta{#debugger-does-not-attach}

+ __Error__: Error al procesar el inicio: Error: No se pudo conectar al destinatario de depuración en...
+ __Causa__: Docker Desktop no se está ejecutando en el sistema local. Para verificar esto, revise la consola de depuración de código VS (Vista > Consola de depuración) y confirme que se ha producido este error.
+ __Resolución__: Inicio  [Docker Desktop y confirme que las imágenes de acoplamiento necesarias están instaladas](./set-up/development-environment.md#docker).

### Los puntos de interrupción no se pausan{#breakpoints-no-pausing}

+ __Error__: Al ejecutar el programa de trabajo de Asset compute desde la herramienta de desarrollo depurable, VS Code no se pausa en los puntos de interrupción.

#### El depurador de código VS no está adjunto{#vs-code-debugger-not-attached}

+ __Causa:__ El depurador de código VS se detuvo o desconectó.
+ __Resolución:__ Reinicie el depurador de código VS y verifique que se adjunta, mirando la consola de salida de depuración de código VS (Vista > Consola de depuración)

#### El depurador de código VS se adjunta tras iniciar la ejecución del trabajo{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ El depurador de código VS no se adjuntó antes de tocar  ____ Ejecutar herramienta de desarrollo.
+ __Resolución:__ asegúrese de que el depurador se ha conectado mediante la revisión de la consola de depuración de VS Code (Vista > Consola de depuración) y, a continuación, vuelva a ejecutar el programa de trabajo de Asset compute desde la herramienta de desarrollo.

### Se agotó el tiempo de espera del trabajo durante la depuración{#worker-times-out-while-debugging}

+ __Error__: La consola de depuración informa de que la acción se agotará en -XXX milisegundos&quot; o de que la previsualización de la representación de la herramienta de desarrollo de  [ ](./develop/development-tool.md) Assets computes gira indefinidamente o
+ __Causa__: Se superó el tiempo de espera de trabajo definido en  [manifest.](./develop/manifest.md) ymlis durante la depuración.
+ __Resolución__: Aumente temporalmente el tiempo de espera del programa de trabajo en  [manifest.](./develop/manifest.md) ymlor y acelere las actividades de depuración.

### No se puede terminar el proceso de depuración{#cannot-terminate-debugger-process}

+ __Error__:  `Ctrl-C` en la línea de comandos no termina el proceso de depuración (`npx adobe-asset-compute devtool`).
+ __Causa__: Un error en  `@adobe/aio-cli-plugin-asset-compute` 1.3.x  `Ctrl-C` no se reconoce como un comando de finalización.
+ __Resolución__: Actualización  `@adobe/aio-cli-plugin-asset-compute` a la versión 1.4.1 o posterior

   ```
   $ aio update
   ```

   ![Resolución de problemas: actualización de aio](./assets/troubleshooting/debug__terminate.png)

## Implementar{#deploy}

### Falta la representación personalizada del recurso en AEM{#custom-rendition-missing-from-asset}

+ __Error:los recursos__ nuevos y reprocesados se procesan correctamente, pero no se encuentran en la representación personalizada

#### Perfil de procesamiento no aplicado a la carpeta antecesora

+ __Causa:__ el recurso no existe en una carpeta con el Perfil de procesamiento que utiliza el programa de trabajo personalizado
+ __Resolución:__ Aplicación del Perfil de procesamiento a una carpeta antecesora del recurso

#### Perfil de procesamiento sustituido por un Perfil de procesamiento inferior

+ __Causa:__ El recurso existe debajo de una carpeta con el Perfil de procesamiento de trabajador personalizado aplicado, pero se ha aplicado un Perfil de procesamiento diferente que no utiliza el programa de trabajo de cliente entre esa carpeta y el recurso.
+ __Resolución:__ Combinar o reconciliar de otro modo los dos Perfiles de procesamiento y quitar el Perfil de procesamiento intermedio

### Error en el procesamiento de recursos en AEM{#asset-processing-fails}

+ __Error:distintivo de error de procesamiento de__ recursos mostrado en el recurso
+ __Causa:__ error al ejecutar el programa de trabajo personalizado
+ __Resolución:__ siga las instrucciones para  [depurar la ](./test-debug/debug.md#aio-app-logs) activación de Adobe I/O Runtime mediante  `aio app logs`.


