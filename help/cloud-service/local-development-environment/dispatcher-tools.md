---
title: Configuración de las herramientas de Dispatcher para AEM como desarrollo de Cloud Service
description: Las herramientas de Dispatcher del SDK de AEM facilitan el desarrollo local de los proyectos de Adobe Experience Manager (AEM), ya que facilitan la instalación, ejecución y resolución de problemas de Dispatcher localmente.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 2%

---


# Configuración de las herramientas locales de Dispatcher

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Herramientas locales de Dispatcher"
>abstract="Dispatcher es una parte integral de la arquitectura general del Experience Manager y debe formar parte de la configuración de desarrollo local. El SDK de AEM as a Cloud Service incluye la versión de herramientas de Dispatcher recomendada, que facilita la configuración, validación y simulación de Dispatcher localmente."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="Dispatcher en la nube"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/es-ES/aemcloud.html" text="Descargar AEM como SDK de Cloud Service"

Dispatcher de Adobe Experience Manager (AEM) es un módulo de servidor web HTTP Apache que proporciona una capa de seguridad y rendimiento entre el nivel de CDN y AEM Publish. Dispatcher es una parte integral de la arquitectura general del Experience Manager y debe formar parte de la configuración de desarrollo local.

El SDK de AEM as a Cloud Service incluye la versión de herramientas de Dispatcher recomendada, que facilita la configuración, validación y simulación de Dispatcher localmente. Las herramientas de Dispatcher constan de:

+ un conjunto de línea de base de los archivos de configuración del servidor web HTTP Apache y Dispatcher, ubicados en `.../dispatcher-sdk-x.x.x/src`
+ una herramienta CLI de validación de configuración ubicada en `.../dispatcher-sdk-x.x.x/bin/validate`
+ una herramienta CLI de generación de configuración ubicada en `.../dispatcher-sdk-x.x.x/bin/validator`
+ una herramienta CLI de implementación de configuración ubicada en `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ una imagen Docker que ejecuta el servidor web HTTP Apache con el módulo Dispatcher

Tenga en cuenta que `~` se utiliza como método abreviado para el Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`.

>[!NOTE]
>
> Los vídeos de esta página se grabaron en macOS. Los usuarios de Windows pueden seguir el ejemplo, pero utilizan los comandos equivalentes de Windows de Dispatcher Tools que se proporcionan con cada vídeo.

## Requisitos previos

1. Los usuarios de Windows deben utilizar Windows 10 Professional (o una versión compatible con Docker)
1. Instale [Experience Manager Publish Quickstart Jar](./aem-runtime.md) en el equipo de desarrollo local.
   + Opcionalmente, instale el [AEM sitio web de referencia](https://github.com/adobe/aem-guides-wknd/releases) más reciente en el servicio AEM Publish local. Este sitio web se utiliza en este tutorial para visualizar un Dispatcher en funcionamiento.
1. Instale e inicie la versión más reciente de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) en el equipo de desarrollo local.

## Descargar las herramientas de Dispatcher (como parte del SDK de AEM)

El SDK de AEM as a Cloud Service, o SDK de AEM, contiene las herramientas de Dispatcher utilizadas para ejecutar el servidor web HTTP Apache con el módulo de Dispatcher localmente para el desarrollo, así como el Jar de inicio rápido compatible.

Si el SDK de AEM as a Cloud Service ya se ha descargado para [configurar el tiempo de ejecución de AEM local](./aem-runtime.md), no es necesario volver a descargarlo.

1. Inicie sesión en [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=1) con su Adobe ID
   + La organización de Adobe __debe__ aprovisionarse para AEM como Cloud Service para descargar el SDK de AEM as a Cloud Service
1. Haga clic en la última fila de resultados de __AEM SDK__ para descargar

## Extraer las herramientas de Dispatcher del zip del SDK de AEM

>[!TIP]
>
> Los usuarios de Windows no pueden tener espacios ni caracteres especiales en la ruta de acceso a la carpeta que contiene las herramientas locales de Dispatcher. Si existen espacios en la ruta, el `docker_run.cmd` fallará.

La versión de las herramientas de Dispatcher es diferente de la del SDK de AEM. Asegúrese de que la versión de las herramientas de Dispatcher se proporcione a través de la versión de AEM SDK que coincida con la AEM como versión de Cloud Service.

1. Descomprima el archivo `aem-sdk-xxx.zip` descargado
1. Desempaquete las herramientas de Dispatcher en `~/aem-sdk/dispatcher`
   + Windows: Descomprimir `aem-sdk-dispatcher-tools-x.x.x-windows.zip` en `C:\Users\<My User>\aem-sdk\dispatcher` (creando carpetas que faltan según sea necesario)
   + macOS / Linux: Ejecute el script shell `aem-sdk-dispatcher-tools-x.x.x-unix.sh` que lo acompaña para desempaquetar las herramientas de Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Tenga en cuenta que todos los comandos que se emiten a continuación suponen que el directorio de trabajo actual contiene el contenido expandido de las herramientas de Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Este vídeo utiliza macOS con fines ilustrativos. Los comandos equivalentes de Windows/Linux se pueden usar para obtener resultados similares*

## Explicación de los archivos de configuración de Dispatcher

>[!TIP]
> Los proyectos de Experience Manager creados a partir del [AEM tipo de archivo Maven del proyecto](https://github.com/adobe/aem-project-archetype) se rellenan previamente con este conjunto de archivos de configuración de Dispatcher, por lo que no es necesario copiar desde la carpeta src de las herramientas de Dispatcher.

Las herramientas de Dispatcher proporcionan un conjunto de archivos de configuración de Dispatcher y del servidor web HTTP Apache que definen el comportamiento de todos los entornos, incluido el desarrollo local.

Estos archivos están pensados para copiarse en un proyecto Maven de Experience Manager a la carpeta `dispatcher/src` si no existen en el proyecto Maven de Experience Manager.

Una descripción completa de los archivos de configuración está disponible en las herramientas de Dispatcher desempaquetadas como `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validar configuraciones

Opcionalmente, las configuraciones del servidor web Dispatcher y Apache (a través de `httpd -t`) se pueden validar usando el script `validate` (no confundir con el ejecutable `validator`). La secuencia de comandos `validate` proporciona una forma conveniente de ejecutar las [3 fases](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/validation-debug.html?lang=en#local-validation-flexible-mode) de la `validator`.

+ Uso:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate.sh ./src`

## Ejecutar Dispatcher localmente

AEM Dispatcher se ejecuta localmente usando Docker con los archivos de configuración del servidor web `src` Dispatcher y Apache.

+ Uso:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

El `<aem-publish-host>` se puede establecer en `host.docker.internal`, proporciona un nombre DNS especial Docker en el contenedor que se resuelve en la IP del equipo host. Si `host.docker.internal` no se resuelve, consulte la sección [solución de problemas](#troubleshooting-host-docker-internal) a continuación.

Por ejemplo, para iniciar el contenedor de Docker de Dispatcher utilizando los archivos de configuración predeterminados proporcionados por las herramientas de Dispatcher:

Inicie el contenedor de Docker de Dispatcher que proporciona la ruta a la carpeta src de configuración de Dispatcher:

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS / Linux: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

El servicio de publicación del SDK de AEM as a Cloud Service, que se ejecuta localmente en el puerto 4503, estará disponible a través de Dispatcher en `http://localhost:8080`.

Para ejecutar las herramientas de Dispatcher con la configuración de Dispatcher de un proyecto de Experience Manager, elija la carpeta `dispatcher/src` del proyecto.

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Registros de herramientas de Dispatcher

Los registros de Dispatcher son útiles durante el desarrollo local para comprender si las solicitudes HTTP están bloqueadas y por qué. El nivel de registro se puede configurar prefiriendo la ejecución de `docker_run` con parámetros de entorno.

Los registros de las herramientas de Dispatcher se emiten al estándar cuando se ejecuta `docker_run`.

Los parámetros útiles para depurar Dispatcher incluyen:

+ `DISP_LOG_LEVEL=Debug` establece el registro del módulo de Dispatcher en el nivel de depuración
   + El valor predeterminado es: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` establece el registro del módulo de reescritura del servidor web HTTP Apache en el nivel de depuración
   + El valor predeterminado es: `Warn`
+ `DISP_RUN_MODE` establece el &quot;modo de ejecución&quot; del entorno de Dispatcher, cargando los modos de ejecución correspondientes archivos de configuración de Dispatcher.
   + El valor predeterminado es `dev`
+ Valores válidos: `dev`, `stage` o `prod`

Se pueden pasar uno o varios parámetros a `docker_run`

+ Windows:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

### Acceso a archivos de registro

Se puede acceder directamente a los registros del servidor web Apache y AEM Dispatcher en el contenedor Docker:

+ [Acceso a los registros en el contenedor Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copia de los registros de Docker al sistema de archivos local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Cuándo actualizar las herramientas de Dispatcher{#dispatcher-tools-version}

Las versiones de las herramientas de Dispatcher aumentan con menor frecuencia que el Experience Manager, por lo que las herramientas de Dispatcher requieren menos actualizaciones en el entorno de desarrollo local.

La versión recomendada de las herramientas de Dispatcher es la que se incluye con el SDK de AEM as a Cloud Service que coincide con el Experience Manager como versión de Cloud Service. La versión de AEM as a Cloud Service se puede encontrar a través de [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Entornos__, por entorno especificado por la etiqueta  __AEM__ Versiones

![Versión del Experience Manager](./assets/dispatcher-tools/aem-version.png)

_Tenga en cuenta que la versión de las herramientas de Dispatcher no coincidirá con la versión del Experience Manager._

## Solución de problemas

### los resultados de docker_run en el mensaje &quot;Esperando hasta que host.docker.internal esté disponible&quot;{#troubleshooting-host-docker-internal}

`host.docker.internal` es un nombre de host proporcionado al Docker contiene que se resuelve en el host. Por docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> A partir de Docker 18.03, nuestra recomendación es conectarse al nombre DNS especial host.docker.internal, que se resuelve a la dirección IP interna utilizada por el host

Si, cuando `bin/docker_run src host.docker.internal:4503 8080` resulta en el mensaje __Esperando hasta que host.docker.internal esté disponible__, entonces:

1. Asegúrese de que la versión instalada de Docker es 18.03 o buena
2. Es posible que tenga una máquina local configurada que impida el registro/resolución del nombre `host.docker.internal`. En su lugar, utilice su IP local.
   + Windows:
      + Desde el símbolo del sistema, ejecute `ipconfig` y registre la __dirección IPv4__ del host.
      + A continuación, ejecute `docker_run` utilizando esta dirección IP:
         `bin\docker_run src <HOST IP>:4503 8080`
   + macOS / Linux:
      + Desde Terminal, ejecute `ifconfig` y registre la dirección IP del host __inet__, normalmente el dispositivo __en0__.
      + A continuación, ejecute `docker_run` utilizando la dirección IP del host:
         `bin/docker_run.sh src <HOST IP>:4503 8080`

#### Error de ejemplo

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run no se inicia en Windows{#troubleshooting-windows-compatible}

La ejecución de `docker_run` en Windows puede dar como resultado el siguiente error, impidiendo que Dispatcher se inicie. Este es un problema del que se informa con Dispatcher en Windows y se solucionará en una versión futura.

#### Error de ejemplo

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run src host.docker.internal:4503 8080

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

+ [Descargar AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [Descargar el sitio web de referencia de AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentación de Dispatcher de Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=es)
