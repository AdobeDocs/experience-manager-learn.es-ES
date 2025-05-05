---
title: Configurar las herramientas de Dispatcher para el desarrollo de AEM as a Cloud Service
description: Las herramientas de Dispatcher de AEM SDK facilitan el desarrollo local de proyectos de Adobe Experience Manager (AEM) al facilitar la instalación, la ejecución y la resolución de problemas de Dispatcher localmente.
version: Experience Manager as a Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 5%

---

# Configurar las herramientas locales de Dispatcher {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Herramientas locales de Dispatcher"
>abstract="Dispatcher es una parte integral de la arquitectura de Experience Manager general y debe formar parte de la configuración de desarrollo local. El SDK de AEM as a Cloud Service incluye la versión de herramientas de Dispatcher recomendada que facilita la configuración, validación y simulación de Dispatcher localmente."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=es" text="Dispatcher en la nube"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html?lang=es" text="Descargar el SDK de AEM as a Cloud Service"

Dispatcher de Adobe Experience Manager (AEM) es un módulo de servidor web HTTP Apache que proporciona una capa de seguridad y rendimiento entre el nivel de CDN y AEM Publish. Dispatcher es una parte integral de la arquitectura de Experience Manager general y debe formar parte de la configuración de desarrollo local.

AEM as a Cloud Service SDK incluye la versión de Dispatcher Tools recomendada que facilita la configuración, validación y simulación de Dispatcher localmente. Las herramientas de Dispatcher están compuestas por:

+ un conjunto de línea de base de archivos de configuración de Dispatcher y servidor web HTTP Apache, ubicado en `.../dispatcher-sdk-x.x.x/src`
+ una herramienta CLI de validación de configuración, ubicada en `.../dispatcher-sdk-x.x.x/bin/validate`
+ una herramienta CLI de generación de configuración, ubicada en `.../dispatcher-sdk-x.x.x/bin/validator`
+ una herramienta CLI de implementación de configuración, ubicada en `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ un archivo de configuración inmutable que sobrescribe la herramienta CLI, ubicada en `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ una imagen Docker que ejecuta el servidor web HTTP Apache con el módulo Dispatcher

Tenga en cuenta que `~` se usa como abreviatura del directorio del usuario. En Windows, es el equivalente de `%HOMEPATH%`.

>[!NOTE]
>
> Los vídeos de esta página se han grabado en macOS. Los usuarios de Windows pueden seguirlo, pero utilizan los comandos equivalentes de Windows de las Herramientas de Dispatcher que se proporcionan con cada vídeo.

## Requisitos previos

1. Los usuarios de Windows deben utilizar Windows 10 Professional (o una versión compatible con Docker)
1. Instale [Experience Manager Publish Quickstart Jar](./aem-runtime.md) en el equipo de desarrollo local.

+ Opcionalmente, instale el [sitio web de referencia de AEM](https://github.com/adobe/aem-guides-wknd/releases) más reciente en el servicio de publicación de AEM local. Este sitio web se utiliza en este tutorial para visualizar una Dispatcher en funcionamiento.

1. Instale e inicie la última versión de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) en el equipo de desarrollo local.

## Descargar las herramientas de Dispatcher (como parte de AEM SDK)

AEM as a Cloud Service SDK, o AEM SDK, contiene las herramientas de Dispatcher utilizadas para ejecutar el servidor web HTTP Apache con el módulo de Dispatcher localmente para desarrollo y el Jar de inicio rápido compatible.

Si AEM as a Cloud Service SDK ya se ha descargado para [configurar el tiempo de ejecución local de AEM](./aem-runtime.md), no es necesario volver a descargarlo.

1. Inicie sesión en [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) con su Adobe ID
   + Su organización de Adobe __debe__ estar aprovisionada para que AEM as a Cloud Service descargue AEM as a Cloud Service SDK
1. Haga clic en la última fila de resultados de __AEM SDK__ que quiera descargar

## Extraiga las herramientas de Dispatcher del zip de SDK de AEM

>[!TIP]
>
> Los usuarios de Windows no pueden tener espacios ni caracteres especiales en la ruta de acceso a la carpeta que contiene las herramientas locales de Dispatcher. Si existen espacios en la ruta de acceso, `docker_run.cmd` produce un error.

La versión de las herramientas de Dispatcher es diferente de la de AEM SDK. Asegúrese de que la versión de las herramientas de Dispatcher se proporciona a través de la versión de AEM SDK que coincide con la versión de AEM as a Cloud Service.

1. Descomprima el archivo `aem-sdk-xxx.zip` descargado
1. Desempaquetar las herramientas de Dispatcher en `~/aem-sdk/dispatcher`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

Descomprima `aem-sdk-dispatcher-tools-x.x.x-windows.zip` en `C:\Users\<My User>\aem-sdk\dispatcher` (creando las carpetas que faltan según sea necesario).

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Todos los comandos que se emiten a continuación suponen que el directorio de trabajo actual contiene el contenido de herramientas de Dispatcher en expansión.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*Este vídeo utiliza macOS con fines ilustrativos. Los comandos equivalentes de Windows/Linux se pueden usar para obtener resultados similares.*

## Comprender los archivos de configuración de Dispatcher

>[!TIP]
> Los proyectos de Experience Manager creados a partir del [tipo de archivo Maven del proyecto de AEM](https://github.com/adobe/aem-project-archetype) se han rellenado previamente en este conjunto de archivos de configuración de Dispatcher, por lo que no es necesario realizar ninguna copia desde la carpeta src de las herramientas de Dispatcher.

Las herramientas de Dispatcher proporcionan un conjunto de archivos de configuración de Dispatcher y del servidor web HTTP Apache que definen el comportamiento de todos los entornos, incluido el desarrollo local.

Estos archivos están pensados para copiarse en un proyecto Maven de Experience Manager en la carpeta `dispatcher/src`, si no existen en el proyecto Maven de Experience Manager.

Hay disponible una descripción completa de los archivos de configuración en las herramientas de Dispatcher desempaquetadas como `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validar configuraciones

Opcionalmente, las configuraciones del servidor web Dispatcher y Apache (a través de `httpd -t`) se pueden validar utilizando el script `validate` (no confundirlo con el ejecutable `validator`). El script `validate` proporciona una forma cómoda de ejecutar las [tres fases](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=es) de `validator`.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux®]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Ejecutar Dispatcher localmente

AEM Dispatcher se ejecuta localmente mediante Docker en los archivos de configuración de `src` servidor web Dispatcher y Apache.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

El ejecutable `docker_run_hot_reload` es preferible sobre `docker_run`, ya que vuelve a cargar los archivos de configuración a medida que se modifican, sin tener que terminar manualmente ni reiniciar `docker_run`. Como alternativa, `docker_run` se puede usar, pero requiere finalizar y reiniciar manualmente `docker_run` cuando se cambien los archivos de configuración.

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

El ejecutable `docker_run_hot_reload` es preferible sobre `docker_run`, ya que vuelve a cargar los archivos de configuración a medida que se modifican, sin tener que terminar manualmente ni reiniciar `docker_run`. Como alternativa, `docker_run` se puede usar, pero requiere finalizar y reiniciar manualmente `docker_run` cuando se cambien los archivos de configuración.

>[!ENDTABS]

El `<aem-publish-host>` se puede establecer en `host.docker.internal`, un nombre DNS especial que Docker proporciona en el contenedor que se resuelve en la dirección IP del equipo host. Si `host.docker.internal` no se resuelve, consulte la sección [solución de problemas](#troubleshooting-host-docker-internal) a continuación.

Por ejemplo, para iniciar el contenedor de Dispatcher Docker utilizando los archivos de configuración predeterminados proporcionados por las herramientas de Dispatcher:

Inicie el contenedor de Dispatcher Docker que proporciona la ruta a la carpeta src de configuración de Dispatcher:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

El servicio de publicación de AEM as a Cloud Service SDK que se ejecuta localmente en el puerto 4503 está disponible a través de Dispatcher en `http://localhost:8080`.

Para ejecutar las herramientas de Dispatcher en la configuración de Dispatcher de un proyecto de Experience Manager, elija la carpeta `dispatcher/src` del proyecto.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Registros de herramientas de Dispatcher

Los registros de Dispatcher son útiles durante el desarrollo local para comprender si las solicitudes HTTP están bloqueadas y por qué. El nivel de registro se puede establecer prefiriendo la ejecución de `docker_run` con parámetros de entorno.

Los registros de Dispatcher Tools se emiten al resultado estándar cuando se ejecuta `docker_run`.

Los parámetros útiles para depurar Dispatcher incluyen:

+ `DISP_LOG_LEVEL=Debug` establece el registro del módulo de Dispatcher en el nivel de depuración
   + El valor predeterminado es: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` establece el registro del módulo de reescritura del servidor web HTTP Apache en el nivel de depuración
   + El valor predeterminado es: `Warn`
+ `DISP_RUN_MODE` establece el &quot;modo de ejecución&quot; del entorno de Dispatcher y carga los archivos de configuración de Dispatcher de los modos de ejecución correspondientes.
   + Valores predeterminados de `dev`
+ Valores válidos: `dev`, `stage` o `prod`

Se pueden pasar uno o varios parámetros a `docker_run`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### Acceso a archivo de registro

Se puede acceder directamente al servidor web Apache y a los registros de AEM Dispatcher en el contenedor de Docker:

+ [Acceder a los registros del contenedor Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copiar los registros de Docker al sistema de archivos local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Cuándo actualizar las herramientas de Dispatcher{#dispatcher-tools-version}

Las versiones de Dispatcher Tools aumentan con menos frecuencia que Experience Manager y, por lo tanto, Dispatcher Tools requiere menos actualizaciones en el entorno de desarrollo local.

La versión de Dispatcher Tools recomendada es la que está empaquetada con AEM as a Cloud Service SDK que coincide con la versión de Experience Manager as a Cloud Service. Se puede encontrar la versión de AEM as a Cloud Service a través de [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Entornos__, por entorno especificado por la etiqueta __AEM Release__

![Versión de Experience Manager](./assets/dispatcher-tools/aem-version.png)

*Tenga en cuenta que la versión de Dispatcher Tools no coincide con la versión de Experience Manager.*

## Cómo actualizar el conjunto de líneas de base de las configuraciones de Apache y Dispatcher

El conjunto de línea de base de la configuración de Apache y Dispatcher se mejora regularmente y se lanza con la versión de AEM as a Cloud Service SDK. Se recomienda incorporar las mejoras de configuración de línea de base en el proyecto de AEM y evitar [errores de validación local](#validate-configurations) y de canalización de Cloud Manager. Actualícelos utilizando el script `update_maven.sh` de la carpeta `.../dispatcher-sdk-x.x.x/bin`.

>[!VIDEO](https://video.tv.adobe.com/v/33559?quality=12&learn=on&captions=spa)

*Este vídeo utiliza macOS con fines ilustrativos. Los comandos equivalentes de Windows/Linux se pueden usar para obtener resultados similares.*


Supongamos que ha creado un proyecto de AEM en el pasado utilizando [AEM Project Archetype](https://github.com/adobe/aem-project-archetype), las configuraciones de línea de base de Apache y Dispatcher eran actuales. Con estas configuraciones de línea de base, las configuraciones específicas del proyecto se crearon reutilizando y copiando archivos como `*.vhost`, `*.conf`, `*.farm` y `*.any` de las carpetas `dispatcher/src/conf.d` y `dispatcher/src/conf.dispatcher.d`. Su validación local de Dispatcher y sus canalizaciones de Cloud Manager funcionaban correctamente.

Mientras tanto, las configuraciones de línea base de Apache y Dispatcher se mejoraron por varias razones, como nuevas funciones, correcciones de seguridad y optimización. Se publican mediante una versión más reciente de las herramientas de Dispatcher como parte de la versión de AEM as a Cloud Service.

Ahora, al validar las configuraciones de Dispatcher específicas del proyecto con la última versión de Dispatcher Tools, comienzan a dar errores. Para resolver esto, es necesario actualizar las configuraciones de línea de base mediante los siguientes pasos:

+ Compruebe que la validación falla con la última versión de las herramientas de Dispatcher

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Actualizar los archivos inmutables mediante el script `update_maven.sh`

  ```shell
  $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Updating dispatcher configuration at folder 
  running in 'extract' mode
  running in 'extract' mode
  reading immutable file list from /etc/httpd/immutable.files.txt
  preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
  ...
  immutable files extraction COMPLETE
  fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
  Cloud manager validator 2.0.53
  ```

+ Compruebe los archivos inmutables actualizados como `dispatcher_vhost.conf`, `default.vhost` y `default.farm` y, si es necesario, realice los cambios pertinentes en los archivos personalizados que se derivan de estos archivos.

+ Vuelva a validar la configuración y debería pasar.

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ Después de la verificación local de los cambios, confirme los archivos de configuración actualizados

## Solución de problemas

### docker_run da como resultado el mensaje &quot;Esperando hasta que host.docker.internal esté disponible&quot;{#troubleshooting-host-docker-internal}

`host.docker.internal` es un nombre de host proporcionado al contenedor Docker que se resuelve en el host. Por docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> A partir de Docker 18.03, la recomendación es conectarse al nombre DNS especial host.docker.internal, que se resuelve en la dirección IP interna utilizada por el host

Cuando `bin/docker_run src host.docker.internal:4503 8080` dé como resultado el mensaje __Esperando a que host.docker.internal esté disponible__, entonces:

1. Asegúrese de que la versión instalada de Docker sea 18.03 o superior
1. Es posible que tenga configurado un equipo local que impida el registro o la resolución del nombre de `host.docker.internal`. En su lugar, utilice su IP local.

>[!BEGINTABS]

>[!TAB macOS]

+ Desde el terminal, ejecute `ifconfig` y registre la dirección IP del host __inet__, normalmente el dispositivo __en0__.
+ A continuación, ejecute `docker_run` utilizando la dirección IP del host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ Desde el símbolo del sistema, ejecute `ipconfig` y registre la __dirección IPv4__ del host en el equipo host.
+ A continuación, ejecute `docker_run` con esta dirección IP: `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux®]

+ Desde el terminal, ejecute `ifconfig` y registre la dirección IP del host __inet__, normalmente el dispositivo __en0__.
+ A continuación, ejecute `docker_run` utilizando la dirección IP del host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

#### Error de ejemplo

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## Recursos adicionales

+ [Descargar AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [Descargar el sitio web de referencia de AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentación de Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=es)
