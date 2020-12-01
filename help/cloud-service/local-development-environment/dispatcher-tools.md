---
title: Configurar las herramientas de Dispatcher para AEM como desarrollo de Cloud Service
description: AEM SDK's Dispatcher Tools facilita el desarrollo local de proyectos de Adobe Experience Manager (AEM) al facilitar la instalación, ejecución y resolución de problemas de Dispatcher localmente.
sub-product: foundation
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: 1b4a927a68d24eeb08d0ee244e85519323482910
workflow-type: tm+mt
source-wordcount: '1534'
ht-degree: 2%

---


# Configuración de las herramientas de Dispatcher locales

Dispatcher de Adobe Experience Manager (AEM) es un módulo de servidor web Apache HTTP que proporciona una capa de seguridad y rendimiento entre el nivel de CDN y AEM Publish. Dispatcher es parte integrante de la estructura general de Experience Manager y debe formar parte del desarrollo local.

El SDK de AEM como Cloud Service incluye la versión recomendada de Dispatcher Tools, que facilita la configuración, validación y simulación de Dispatcher localmente. Dispatcher Tools está compuesto por:

+ un conjunto básico de archivos de configuración de Dispatcher y del servidor Web HTTP Apache, ubicados en `.../dispatcher-sdk-x.x.x/src`
+ una herramienta CLI del validador de configuración, ubicada en `.../dispatcher-sdk-x.x.x/bin/validate` (Dispatcher SDK 2.0.29+)
+ una herramienta CLI de generación de configuración ubicada en `.../dispatcher-sdk-x.x.x/bin/validator`
+ una herramienta CLI de implementación de configuración ubicada en `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ una imagen de Docker que ejecuta el servidor web Apache HTTP con el módulo Dispatcher

Tenga en cuenta que `~` se utiliza como método abreviado para el Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`.

>[!NOTE]
>
> Los vídeos de esta página se grabaron en macOS. Los usuarios de Windows pueden seguir el ejemplo, pero utilizan los comandos equivalentes de Windows Dispatcher Tools, que se proporcionan con cada vídeo.

## Requisitos previos

1. Los usuarios de Windows deben utilizar Windows 10 Professional
1. Instale [Experience Manager Publish Quickstart Jar](./aem-runtime.md) en el equipo de desarrollo local.
   + Si lo desea, instale el [sitio Web de referencia más reciente](https://github.com/adobe/aem-guides-wknd/releases) en el servicio AEM Publish local. Este sitio web se utiliza en este tutorial para visualizar un despachante en funcionamiento.
1. Instale y inicio la versión más reciente de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) en el equipo de desarrollo local.

## Descargar las herramientas de Dispatcher (como parte del SDK de AEM)

El AEM como SDK de Cloud Service o SDK de AEM contiene las herramientas de Dispatcher utilizadas para ejecutar el servidor web Apache HTTP con el módulo Dispatcher localmente para el desarrollo, así como la barra de inicio rápido compatible.

Si el AEM como Cloud Service SDK ya se ha descargado en [configurar el AEM de tiempo de ejecución local](./aem-runtime.md), no es necesario volver a descargarlo.

1. Inicie sesión en [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout lista&amp;p.offset=0&amp;p.limit=1) con su Adobe ID
   + La organización de Adobe __debe__ aprovisionarse para AEM como Cloud Service para descargar la AEM como un SDK de Cloud Service
1. Haga clic en la última fila de resultados __AEM SDK__ para descargar
   + Asegúrese de que las herramientas de despachante del SDK de AEM v2.0.29+ aparecen en la descripción de la descarga

## Extraer las herramientas de Dispatcher del zip del SDK de AEM

>[!TIP]
>
> Los usuarios de Windows no pueden tener espacios ni caracteres especiales en la ruta de la carpeta que contiene las Herramientas de despachante locales. Si existen espacios en la ruta, `docker_run.cmd` fallará.

La versión de Dispatcher Tools es diferente de la del SDK de AEM. Asegúrese de que la versión de Dispatcher Tools se proporciona mediante la versión AEM SDK que coincide con la AEM como versión de Cloud Service.

1. Descomprima el archivo `aem-sdk-xxx.zip` descargado
1. Desempaquetar las herramientas del despachante en `~/aem-sdk/dispatcher`
   + Windows: Descomprima `aem-sdk-dispatcher-tools-x.x.x-windows.zip` en `C:\Users\<My User>\aem-sdk\dispatcher` (creando carpetas que faltan según sea necesario)
   + macOS / Linux: Ejecute el script de shell correspondiente `aem-sdk-dispatcher-tools-x.x.x-unix.sh` para desempaquetar las herramientas de Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Tenga en cuenta que todos los comandos que se emiten a continuación suponen que el directorio de trabajo actual contiene el contenido de las herramientas de Dispatcher en expansión.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Este vídeo utiliza macOS para fines ilustrativos. Los comandos equivalentes de Windows/Linux pueden utilizarse para obtener resultados similares*

## Comprender los archivos de configuración de Dispatcher

>[!TIP]
> Los proyectos de Experience Manager creados a partir del [AEM arquetipo Maven Project](https://github.com/adobe/aem-project-archetype) se rellenan previamente con este conjunto de archivos de configuración de Dispatcher, por lo que no es necesario copiar desde la carpeta src de Dispatcher Tools.

Las Herramientas de Dispatcher proporcionan un conjunto de archivos de configuración de Dispatcher y de servidor web HTTP Apache que definen el comportamiento de todos los entornos, incluido el desarrollo local.

Estos archivos están destinados a copiarse en un proyecto Maven Experience Manager en la carpeta `dispatcher/src` si no existen en el proyecto Maven Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*Este vídeo utiliza macOS para fines ilustrativos. Los comandos equivalentes de Windows/Linux pueden utilizarse para obtener resultados similares*

Una descripción completa de los archivos de configuración está disponible en las herramientas de Dispatcher desempaquetadas como `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validar configuraciones

Opcionalmente, las configuraciones de los servidores Web Dispatcher y Apache (mediante `httpd -t`) se pueden validar usando la secuencia de comandos `validate` (no confundir con el archivo ejecutable `validator`).

+ Uso:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate ./src`

## Ejecutar Dispatcher localmente

Para ejecutar Dispatcher localmente, los archivos de configuración de Dispatcher deben generarse con la herramienta CLI `validator` de Dispatcher Tools.

+ Uso:
   + Windows: `bin\validator full -d out src`
   + macOS / Linux: `./bin/validator full -d ./out ./src`

Este comando transpila las configuraciones en un conjunto de archivos compatible con el servidor web Apache HTTP del contenedor del Docker.

Una vez generadas, las configuraciones transpiladas se utilizan ejecutar Dispatcher localmente en el contenedor del Docker. Es importante asegurarse de que las configuraciones más recientes se han validado mediante `validate` __y__ resultados mediante la opción `-d` del validador.

+ Uso:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

El `aem-publish-host` se puede establecer en `host.docker.internal`, un Docker de nombre DNS especial proporciona en el contenedor que responde a la IP del equipo host. Si `host.docker.internal` no se resuelve, consulte la sección [solución de problemas](#troubleshooting-host-docker-internal) a continuación.

Por ejemplo, para el inicio del contenedor Dispatcher Docker mediante los archivos de configuración predeterminados proporcionados por las Herramientas de Dispatcher:

1. Genere el `deployment-folder`, denominado `out` por convención, desde cero cada vez que cambie una configuración:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS / Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Re)inicio Dispatcher Docker contenedor que proporciona la ruta a la carpeta de implementación:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

El AEM como servicio de publicación del SDK para Cloud Service, que se ejecuta localmente en el puerto 4503, estará disponible a través de Dispatcher en `http://localhost:8080`.

Para ejecutar Dispatcher Tools en la configuración de Dispatcher de un proyecto Experience Manager, simplemente genere `deployment-folder` utilizando la carpeta `dispatcher/src` del proyecto.

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*Este vídeo utiliza macOS para fines ilustrativos. Los comandos equivalentes de Windows/Linux pueden utilizarse para obtener resultados similares*

## Registros de herramientas de despachador

Los registros de Dispatcher son útiles durante el desarrollo local para saber si las solicitudes HTTP están bloqueadas y por qué. El nivel de registro se puede configurar añadiendo un prefijo a la ejecución de `docker_run` con parámetros de entorno.

Los registros de las herramientas del despachante se emiten hasta el final estándar cuando se ejecuta `docker_run`.

Los parámetros útiles para la depuración de Dispatcher incluyen:

+ `DISP_LOG_LEVEL=Debug` establece el registro del módulo Dispatcher en el nivel de depuración
   + El valor predeterminado es: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` establece el registro del módulo de reescritura del servidor web Apache HTTP en el nivel de depuración
   + El valor predeterminado es: `Warn`
+ `DISP_RUN_MODE` establece el &quot;modo de ejecución&quot; del entorno Dispatcher, cargando los correspondientes archivos de configuración Dispatcher de modo de ejecución.
   + El valor predeterminado es `dev`
+ Valores válidos: `dev`, `stage` o `prod`

Uno o varios parámetros se pueden pasar a `docker_run`

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*Este vídeo utiliza macOS para fines ilustrativos. Los comandos equivalentes de Windows/Linux pueden utilizarse para obtener resultados similares*

## Cuándo actualizar las herramientas de Dispatcher{#dispatcher-tools-version}

Las versiones de Dispatcher Tools aumentan con menos frecuencia que el Experience Manager, por lo que Dispatcher Tools requiere menos actualizaciones en el entorno de desarrollo local.

La versión recomendada de Dispatcher Tools es la que se incluye con el AEM como un SDK de Cloud Service que coincide con el Experience Manager como versión de Cloud Service. La versión de AEM como Cloud Service se puede encontrar a través de [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Entornos__, por entorno especificado por la  __AEM__ recargable

![Versión de Experience Manager](./assets/dispatcher-tools/aem-version.png)

_Tenga en cuenta que la versión de las herramientas de Dispatcher no coincidirá con la versión del Experience Manager._

## Solución de problemas

### docker_run da como resultado el mensaje &#39;Esperando hasta que host.docker.internal esté disponible&#39;{#troubleshooting-host-docker-internal}

`host.docker.internal` es un nombre de host proporcionado al contenedor de Docker que responde al host. Por docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> A partir de Docker 18.03, nuestra recomendación es conectar con el nombre DNS especial host.docker.internal, que responde a la dirección IP interna utilizada por el host

Si, cuando `bin/docker_run out host.docker.internal:4503 8080` resulta en el mensaje __Esperando hasta que host.docker.internal esté disponible__, entonces:

1. Asegúrese de que la versión instalada de Docker es 18.03 o buena
2. Es posible que tenga una máquina local configurada que impida el registro/resolución del nombre `host.docker.internal`. En su lugar, utilice su IP local.
   + Windows:
      + En el símbolo del sistema, ejecute `ipconfig` y registre la __dirección IPv4__ del host.
      + A continuación, ejecute `docker_run` con esta dirección IP:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + Desde Terminal, ejecute `ifconfig` y registre la dirección IP del host __inet__, generalmente el dispositivo __en0__.
      + A continuación, ejecute `docker_run` con la dirección IP del host:
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### Error de ejemplo

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run da como resultado el error &#39;**: No se encontró la carpeta de implementación&#39;

Al ejecutar `docker_run.cmd`, aparece un error que indica __** error: No se encontró la carpeta de implementación:__. Esto suele ocurrir porque hay espacios en la ruta. Si es posible, elimine los espacios de la carpeta o mueva la carpeta `aem-sdk` a una ruta que no contenga espacios.

Por ejemplo, las carpetas de usuario de Windows suelen ser `<First name> <Last name>`, con un espacio entre. En el ejemplo siguiente, la carpeta `...\My User\...` contiene un espacio que rompe la ejecución `docker_run` de las Herramientas de Dispatcher locales. Si los espacios están en una carpeta de usuario de Windows, no intente cambiar el nombre de esta carpeta, ya que se romperá Windows, en lugar de mover la carpeta `aem-sdk` a una nueva ubicación en la que el usuario tenga permisos para modificarla por completo. Tenga en cuenta que las instrucciones que suponen que la carpeta `aem-sdk` está en el directorio principal del usuario deberán ajustarse a la nueva ubicación.

#### Error de ejemplo

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run falla en el inicio en Windows{#troubleshooting-windows-compatible}

La ejecución de `docker_run` en Windows puede provocar el siguiente error, impidiendo que Dispatcher se inicie. Se trata de un problema notificado con Dispatcher en Windows y se solucionará en una versión futura.

#### Error de ejemplo

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## Recursos adicionales

+ [Descargar SDK AEM](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [Descargar el sitio web de referencia de AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentación del despachante de Experience Manager](https://docs.adobe.com/content/help/es-ES/experience-manager-dispatcher/using/dispatcher.html)
