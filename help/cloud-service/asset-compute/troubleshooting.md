---
title: Solucionar problemas de extensibilidad de Assets computes para AEM Assets
description: A continuación se muestra un índice de problemas y errores comunes, junto con las soluciones que podrían encontrarse al desarrollar e implementar Assets computes personalizadas para AEM Assets.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 0%

---

# Solucionar problemas de extensibilidad de Asset compute

A continuación se muestra un índice de problemas y errores comunes, junto con las soluciones que podrían encontrarse al desarrollar e implementar Assets computes personalizadas para AEM Assets.

## Desarrollar{#develop}

### La representación se devuelve parcialmente dibujada/dañada{#rendition-returned-partially-drawn-or-corrupt}

+ __Error__: la representación no se procesa por completo (cuando hay una imagen) o está dañada y no se puede abrir.

  ![La representación se ha devuelto parcialmente dibujada](./assets/troubleshooting/develop__await.png)

+ __Causa__: la función `renditionCallback` del trabajador se está cerrando antes de que la representación se pueda escribir completamente en `rendition.path`.
+ __Resolución__: revise el código de trabajo personalizado y asegúrese de que todas las llamadas asincrónicas se realizan sincrónicas mediante `await`.

## Herramienta de desarrollo{#development-tool}

### Falta el archivo Console.json en el proyecto de Asset compute{#missing-console-json}

+ __Error:__ Error: faltan los archivos necesarios en la validación (`.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY`) en setupAssetCompute asincrónico (`.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY`)
+ __Causa:__ Falta el archivo `console.json` en la raíz del proyecto de Asset compute
+ __Resolución:__ Descargue un nuevo(a) `console.json` de su proyecto de Adobe I/O
   1. En console.adobe.io, abra el proyecto de Adobe I/O que el proyecto de Asset compute está configurado para utilizar
   1. Pulsa el botón __Descargar__ en la parte superior derecha
   1. Guarde el archivo descargado en la raíz del proyecto de Asset compute con el nombre de archivo `console.json`

### Sangría YAML incorrecta en manifest.yml{#incorrect-yaml-indentation}

+ __Error:__ YAMLException: sangría incorrecta de una entrada de asignación en la línea X, columna Y:(a través de la salida estándar del comando `aio app run`)
+ __Causa:__ los archivos Yaml distinguen entre espacios en blanco, por lo que es probable que la sangría sea incorrecta.
+ __Resolución:__ Revise su `manifest.yml` y asegúrese de que toda la sangría sea correcta.

### El límite memorySize se ha establecido en un valor demasiado bajo{#memorysize-limit-is-set-too-low}

+ __Error:__ OpenWhiskError del servidor de desarrollo local: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Devolvió HTTP 400 (Solicitud incorrecta) —> &quot;El contenido de la solicitud tenía un formato incorrecto: error de requisito: la memoria está 64 MB por debajo del umbral permitido de 134217728 B&quot;
+ __Causa:__ Se estableció un límite de `memorySize` para el trabajador en `manifest.yml` por debajo del umbral mínimo permitido indicado en el mensaje de error en bytes.
+ __Resolución:__ Revise los límites de `memorySize` en `manifest.yml` y asegúrese de que todos superen el umbral mínimo permitido.

### No se puede iniciar la herramienta de desarrollo porque falta private.key{#missing-private-key}

+ __Error:__ Error del servidor de desarrollo local: faltan los archivos necesarios en validatePrivateKeyFile.... (mediante salida estándar desde el comando `aio app run`)
+ __Causa:__ El valor `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` del archivo `.env`, no apunta a `private.key` o `private.key` no es legible para el usuario actual.
+ __Resolución:__ Revise el valor `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` del archivo `.env` y asegúrese de que contiene la ruta de acceso completa y absoluta a `private.key` en su sistema de archivos.

### Menú desplegable de archivos Source incorrecto{#source-files-dropdown-incorrect}

Es posible que la herramienta de desarrollo de assets computes introduzca un estado en el que extrae datos antiguos y que sea más visible en la lista desplegable __archivo Source__ que muestra elementos incorrectos.

+ __Error:__ La lista desplegable del archivo Source muestra elementos incorrectos.
+ __Causa:__ El estado de explorador en caché obsoleto causa el error
+ __Resolución:__ En su explorador borre completamente el &quot;estado de aplicación&quot; de la ficha del explorador, la caché del explorador, el almacenamiento local y el trabajador de servicio.

### Parámetro de consulta devToolToken faltante o no válido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Error:__ notificación &quot;no autorizada&quot; en la herramienta de desarrollo de Assets computes
+ __Falta la causa:__ `devToolToken` o no es válida
+ __Resolución:__ Cierre la ventana del explorador de la Herramienta de desarrollo de Assets computes, termine cualquier proceso de la Herramienta de desarrollo que se esté ejecutando iniciado mediante el comando `aio app run` y vuelva a iniciar la Herramienta de desarrollo (con `aio app run`).

### No se pueden eliminar los archivos de origen{#unable-to-remove-source-files}

+ __Error:__ No hay forma de quitar los archivos de origen agregados de la interfaz de usuario de las herramientas de desarrollo
+ __Causa:__ Esta funcionalidad no se ha implementado
+ __Resolución:__ Inicie sesión en su proveedor de almacenamiento en la nube con las credenciales definidas en `.env`. Busque el contenedor utilizado por las herramientas de desarrollo (también especificado en `.env`), navegue hasta la carpeta __source__ y elimine las imágenes de origen. Es posible que tenga que realizar los pasos descritos en [Lista desplegable de archivos Source incorrecta](#source-files-dropdown-incorrect) si los archivos de origen eliminados siguen mostrándose en la lista desplegable, ya que pueden almacenarse en la caché local en el &quot;estado de aplicación&quot; de las herramientas de desarrollo.

  ![Almacenamiento de blob de Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Prueba{#test}

### No se ha generado ninguna representación durante la ejecución de la prueba{#test-no-rendition-generated}

+ __Error:__ Error: No se generó ninguna representación.
+ __Causa:__ El trabajador no pudo generar una representación debido a un error inesperado, como un error de sintaxis de JavaScript.
+ __Resolución:__ Revise `test.log` de la ejecución de la prueba a las `/build/test-results/test-worker/test.log`. Busque la sección de este archivo correspondiente al caso de prueba que falla y revise los errores.

  ![Solución de problemas - No se generó ninguna representación](./assets/troubleshooting/test__no-rendition-generated.png)

### La prueba genera una representación incorrecta que provoca que la prueba falle{#tests-generates-incorrect-rendition}

+ __Error:__ Error: La representación &#39;rendition.xxx&#39; no es la esperada.
+ __Causa:__ El trabajador generó una representación que no era la misma que la `rendition.<extension>` proporcionada en el caso de prueba.
   + Si el archivo `rendition.<extension>` esperado no se crea exactamente de la misma manera que la representación generada localmente en el caso de prueba, es posible que la prueba falle, ya que puede haber alguna diferencia en los bits. Por ejemplo, si el trabajador de Asset compute cambia el contraste mediante API y el resultado esperado se crea ajustando el contraste en Adobe Photoshop CC, los archivos pueden aparecer igual, pero las variaciones menores en los bits pueden ser diferentes.
+ __Resolución:__ revise el resultado de la representación de la prueba navegando hasta `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` y compárelo con el archivo de representación esperado en el caso de prueba. Para crear un recurso esperado exacto, haga lo siguiente:
   + Utilice la herramienta de desarrollo para generar una representación, valide que es correcta y utilícela como el archivo de representación esperado
   + O bien, valide el archivo generado por la prueba en `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, valide que es correcto y utilícelo como el archivo de representación esperado

## Depurar

### El depurador no se asocia{#debugger-does-not-attach}

+ __Error__: Error al procesar el inicio: Error: No se pudo conectar con el destino de depuración en...
+ __Causa__: Docker Desktop no se está ejecutando en el sistema local. Compruebe esto revisando la consola de depuración de código de VS (Ver > Consola de depuración), confirmando que se ha informado de este error.
+ __Resolución__: Inicie [Docker Desktop y confirme que se han instalado las imágenes de Docker necesarias](./set-up/development-environment.md#docker).

### Los puntos de interrupción no se pausan{#breakpoints-no-pausing}

+ __Error__: Al ejecutar el trabajo de Asset compute desde la herramienta de desarrollo de depuración, el código VS no se pausa en los puntos de interrupción.

#### El depurador de código VS no está adjunto{#vs-code-debugger-not-attached}

+ __Causa:__ Se detuvo o desconectó el depurador de código de VS.
+ __Resolución:__ Reinicie el depurador de código de VS y compruebe que se adjunta observando la consola de salida de depuración de código de VS (Ver > Consola de depuración)

#### Se adjuntó el depurador de código VS después de iniciar la ejecución del trabajador{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ El depurador de código VS no se adjuntó antes de pulsar __Ejecutar__ en la herramienta de desarrollo.
+ __Resolución:__ Asegúrese de que el depurador se ha adjuntado revisando la consola de depuración de VS Code (Ver > Consola de depuración) y, a continuación, vuelva a ejecutar el trabajo de Asset compute desde la herramienta de desarrollo.

### Se agota el tiempo de espera del trabajador durante depuración{#worker-times-out-while-debugging}

+ __Error__: la consola de depuración informa de que la acción agotará el tiempo de espera en -XXX milisegundos&quot; o que la vista previa de la representación de [Herramienta de desarrollo de Assets computes](./develop/development-tool.md) gira indefinidamente o
+ __Causa__: el tiempo de espera del trabajador definido en el [manifest.yml](./develop/manifest.md) se excede durante la depuración.
+ __Resolución__: aumente temporalmente el tiempo de espera del trabajador en [manifest.yml](./develop/manifest.md) o acelere las actividades de depuración.

### No se puede finalizar el proceso del depurador{#cannot-terminate-debugger-process}

+ __Error__: `Ctrl-C` en la línea de comandos no finaliza el proceso del depurador (`npx adobe-asset-compute devtool`).
+ __Causa__: Un error en `@adobe/aio-cli-plugin-asset-compute` 1.3.x hace que `Ctrl-C` no se reconozca como un comando de terminación.
+ __Resolución__: actualizar `@adobe/aio-cli-plugin-asset-compute` a la versión 1.4.1 o posterior

  ```
  $ aio update
  ```

  ![Solución de problemas - actualización de aio](./assets/troubleshooting/debug__terminate.png)

## Implementación de{#deploy}

### AEM Falta la representación personalizada en el recurso en el recurso en el que se ha realizado el{#custom-rendition-missing-from-asset}

+ __Error:__ Los recursos nuevos y reprocesados se procesaron correctamente, pero faltan en la representación personalizada

#### Perfil de procesamiento no aplicado a la carpeta antecesora

+ __Causa:__ El recurso no existe en una carpeta con el perfil de procesamiento que usa el trabajador personalizado
+ __Resolución:__ Aplicar el perfil de procesamiento a una carpeta antecesora del recurso

#### Perfil de procesamiento reemplazado por un perfil de procesamiento inferior

+ __Causa:__ El recurso existe debajo de una carpeta con el perfil de procesamiento de trabajador personalizado aplicado, pero se ha aplicado un perfil de procesamiento diferente que no utiliza el trabajador del cliente entre esa carpeta y el recurso.
+ __Resolución:__ Combine o reconcilie los dos perfiles de procesamiento y quite el perfil de procesamiento intermedio

### AEM El procesamiento de recursos falla en la{#asset-processing-fails}

+ __Error:__ Error al procesar el recurso mostrado en el recurso
+ __Causa:__ Se produjo un error en la ejecución del trabajador personalizado
+ __Resolución:__ Siga las instrucciones de [depuración de activaciones de Adobe I/O Runtime](./test-debug/debug.md#aio-app-logs) mediante `aio app logs`.
