---
title: AEM Configuración de las herramientas de Dispatcher para el desarrollo as a Cloud Service de
description: AEM Las herramientas de Dispatcher de SDK facilitan el desarrollo local de proyectos de Adobe Experience Manager AEM () al facilitar la instalación, la ejecución y la resolución de problemas de Dispatcher localmente.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: 9073c1d41c67ec654b232aea9177878f11793d07
workflow-type: tm+mt
source-wordcount: '1621'
ht-degree: 9%

---

# Configurar las herramientas locales de Dispatcher {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Herramientas locales de Dispatcher"
>abstract="Dispatcher es una parte integral de la arquitectura de Experience Manager general y debe formar parte de la configuración de desarrollo local. El SDK de AEM as a Cloud Service incluye la versión de herramientas de Dispatcher recomendada, que facilita la configuración, validación y simulación de Dispatcher de manera local."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=es" text="Dispatcher en la nube"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html" text="Descargar el SDK de AEM as a Cloud Service"

Dispatcher de Adobe Experience Manager AEM () es un módulo de servidor web HTTP Apache que proporciona una capa de seguridad y rendimiento entre el nivel de CDN y AEM Publish. Dispatcher es una parte integral de la arquitectura de Experience Manager general y debe formar parte de la configuración de desarrollo local.

El SDK de AEM as a Cloud Service incluye la versión de herramientas de Dispatcher recomendada, que facilita la configuración, validación y simulación de Dispatcher de manera local. Las herramientas de Dispatcher constan de:

+ un conjunto de línea de base de archivos de configuración del servidor web HTTP Apache y Dispatcher, ubicado en `.../dispatcher-sdk-x.x.x/src`
+ una herramienta CLI de validación de configuración, ubicada en `.../dispatcher-sdk-x.x.x/bin/validate`
+ una herramienta CLI de generación de configuración, ubicada en `.../dispatcher-sdk-x.x.x/bin/validator`
+ una herramienta CLI de implementación de configuración, que se encuentra en `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ archivos de configuración inmutables que sobrescriben la herramienta CLI, ubicada en `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ una imagen Docker que ejecuta el servidor web HTTP Apache con el módulo Dispatcher

Tenga en cuenta que `~` se utiliza como abreviatura del Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`.

>[!NOTE]
>
> Los vídeos de esta página se han grabado en macOS. Los usuarios de Windows pueden seguir, pero utilizar los comandos equivalentes de Windows de las herramientas de Dispatcher que se proporcionan con cada vídeo.

## Requisitos previos

1. Los usuarios de Windows deben utilizar Windows 10 Professional (o una versión compatible con Docker)
1. Instalar [Jar de inicio rápido de publicación de Experience Manager](./aem-runtime.md) en la máquina de desarrollo local.

+ Si lo desea, instale la última versión [AEM sitio web de referencia de](https://github.com/adobe/aem-guides-wknd/releases) en el servicio local AEM Publish. Este sitio web se utiliza en este tutorial para visualizar una instancia de Dispatcher en funcionamiento.

1. Instale e inicie la última versión de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) en la máquina de desarrollo local.

## AEM Descargar las herramientas de Dispatcher (como parte del SDK de la)

AEM El SDK as a Cloud Service AEM, o SDK de, contiene las herramientas de Dispatcher utilizadas para ejecutar el servidor web HTTP de Apache con el módulo de Dispatcher localmente para desarrollo y el Jar de inicio rápido compatible.

AEM Si el SDK as a Cloud Service de la ya se ha descargado en [AEM configuración del tiempo de ejecución de la local](./aem-runtime.md), no es necesario volver a descargarla.

1. Iniciar sesión en [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) con su Adobe ID
   + Su organización de Adobe __debe__ AEM se debe aprovisionar para que los as a Cloud Service AEM descarguen el SDK as a Cloud Service de la
1. Haga clic en la última __AEM SDK de__ fila de resultados para descargar

## AEM Extraiga las herramientas de Dispatcher del zip del SDK de la

>[!TIP]
>
> Los usuarios de Windows no pueden tener espacios ni caracteres especiales en la ruta de acceso a la carpeta que contiene las herramientas de Dispatcher locales. Si existen espacios en la ruta, la variable `docker_run.cmd` falla.

AEM La versión de las herramientas de Dispatcher es diferente a la del SDK de la. AEM AEM Asegúrese de que la versión de las herramientas de Dispatcher se proporciona a través de la versión del SDK de la que coincida con la versión as a Cloud Service.

1. Descomprima el archivo descargado `aem-sdk-xxx.zip` archivo
1. Desempaquetar las herramientas de Dispatcher en `~/aem-sdk/dispatcher`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

Descomprimir `aem-sdk-dispatcher-tools-x.x.x-windows.zip` en `C:\Users\<My User>\aem-sdk\dispatcher` (creando las carpetas que faltan según sea necesario).

>[!TAB Linux]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Todos los comandos que se emiten a continuación suponen que el directorio de trabajo actual contiene el contenido de herramientas de Dispatcher en expansión.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*Este vídeo utiliza macOS con fines ilustrativos. Los comandos equivalentes de Windows/Linux se pueden utilizar para lograr resultados similares.*

## Comprender los archivos de configuración de Dispatcher

>[!TIP]
> Proyectos del Experience Manager creados a partir de [AEM Arquetipo del proyecto Maven](https://github.com/adobe/aem-project-archetype) están rellenados previamente en este conjunto de archivos de configuración de Dispatcher, por lo que no es necesario copiar desde la carpeta src de herramientas de Dispatcher.

Las herramientas de Dispatcher proporcionan un conjunto de archivos de configuración de Apache HTTP Web Server y Dispatcher que definen el comportamiento de todos los entornos, incluido el desarrollo local.

Estos archivos están pensados para copiarse en un proyecto Maven de Experience Manager a `dispatcher/src` , si aún no existen en el proyecto de Experience Manager Maven.

Una descripción completa de los archivos de configuración está disponible en las herramientas de Dispatcher desempaquetadas como `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validar configuraciones

Opcionalmente, las configuraciones del servidor web de Dispatcher y Apache (a través de ) `httpd -t`) se puede validar utilizando el `validate` script (no debe confundirse con el script `validator` ejecutable). El `validate` proporciona una forma cómoda de ejecutar el [tres fases](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) de la `validator`.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Ejecutar Dispatcher localmente

AEM Dispatcher se ejecuta localmente mediante Docker en el `src` Archivos de configuración de Dispatcher y del servidor web Apache.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux]

```shell
$ ./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!ENDTABS]

El `<aem-publish-host>` se puede establecer en `host.docker.internal`, un nombre DNS especial que Docker proporciona en el contenedor que se resuelve en la IP del equipo host. Si la variable `host.docker.internal` no resuelve, consulte la [solución de problemas](#troubleshooting-host-docker-internal) más abajo.

Por ejemplo, para iniciar el contenedor de Docker de Dispatcher utilizando los archivos de configuración predeterminados proporcionados por las herramientas de Dispatcher:

Inicie el contenedor de Docker de Dispatcher y proporcione la ruta a la carpeta src de configuración de Dispatcher:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ ./bin/docker_run.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

AEM El servicio de publicación del SDK as a Cloud Service, que se ejecuta localmente en el puerto 4503, está disponible a través de Dispatcher en `http://localhost:8080`.

Para ejecutar las herramientas de Dispatcher en la configuración de Dispatcher de un proyecto de Experience Manager, elija la del proyecto `dispatcher/src` carpeta.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Registros de herramientas de Dispatcher

Los registros de Dispatcher son útiles durante el desarrollo local para comprender si las solicitudes HTTP están bloqueadas y por qué. El nivel de registro se puede establecer prefiriendo la ejecución de `docker_run` con parámetros de entorno.

Los registros de las herramientas de Dispatcher se emiten a la salida estándar cuando `docker_run` se ejecuta.

Los parámetros útiles para depurar Dispatcher incluyen:

+ `DISP_LOG_LEVEL=Debug` establece el registro del módulo de Dispatcher en el nivel de depuración
   + El valor predeterminado es: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` establece el registro del módulo de reescritura del servidor web HTTP Apache en el nivel de depuración
   + El valor predeterminado es: `Warn`
+ `DISP_RUN_MODE` establece el &quot;modo de ejecución&quot; del entorno de Dispatcher, cargando los modos de ejecución correspondientes y los archivos de configuración de Dispatcher.
   + El valor predeterminado es `dev`
+ Valores válidos: `dev`, `stage`, o `prod`

Se pueden pasar uno o varios parámetros a `docker_run`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### Acceso a archivo de registro

AEM Se puede acceder directamente al servidor web Apache y a los registros de Dispatcher en el contenedor de Docker:

+ [Acceder a los registros del contenedor Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copiar los registros de Docker al sistema de archivos local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Cuándo actualizar las herramientas de Dispatcher{#dispatcher-tools-version}

Las versiones de las herramientas de Dispatcher aumentan con menos frecuencia que el Experience Manager y, por lo tanto, las herramientas de Dispatcher requieren menos actualizaciones en el entorno de desarrollo local.

AEM La versión recomendada de las herramientas de Dispatcher es la que está empaquetada con el SDK as a Cloud Service de que coincide con la versión as a Cloud Service de Experience Manager. AEM La versión de as a Cloud Service se encuentra en [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Entornos__, por entorno especificado por el __AEM Versión de__ etiqueta

![Versión del Experience Manager](./assets/dispatcher-tools/aem-version.png)

*Tenga en cuenta que la versión de las herramientas de Dispatcher no coincide con la versión del Experience Manager.*

## Cómo actualizar el conjunto de líneas de base de las configuraciones de Apache y Dispatcher

AEM El conjunto de línea de base de la configuración de Apache y Dispatcher se mejora regularmente y se lanza con la versión del SDK as a Cloud Service de la. AEM Se recomienda incorporar las mejoras de configuración de línea de base en el proyecto de y evitar [validación local](#validate-configurations) Errores en la canalización de y Cloud Manager. Actualícelos con el `update_maven.sh` desde el `.../dispatcher-sdk-x.x.x/bin` carpeta.

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*Este vídeo utiliza macOS con fines ilustrativos. Los comandos equivalentes de Windows/Linux se pueden utilizar para lograr resultados similares.*


AEM Supongamos que ha creado un proyecto en el pasado con el que se ha creado un proyecto de. [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype), las configuraciones de línea base de Apache y Dispatcher estaban actualizadas. Con estas configuraciones de línea de base, las configuraciones específicas del proyecto se crearon reutilizando y copiando los archivos como `*.vhost`, `*.conf`, `*.farm` y `*.any` desde el `dispatcher/src/conf.d` y `dispatcher/src/conf.dispatcher.d` carpetas. La validación local de Dispatcher y las canalizaciones de Cloud Manager funcionaban correctamente.

Mientras tanto, las configuraciones de línea base de Apache y Dispatcher se mejoraron por varios motivos, como nuevas funciones, correcciones de seguridad y optimización. AEM Se lanzan a través de una versión más reciente de las herramientas de Dispatcher como parte de la versión as a Cloud Service de la.

Ahora, al validar las configuraciones de Dispatcher específicas del proyecto con la última versión de las herramientas de Dispatcher, comienzan a fallar. Para resolver esto, es necesario actualizar las configuraciones de línea de base mediante los siguientes pasos:

+ Compruebe que la validación falle en la última versión de las herramientas de Dispatcher

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Actualizar los archivos inmutables mediante la variable `update_maven.sh` script

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

+ Verifique los archivos inmutables actualizados como `dispatcher_vhost.conf`, `default.vhost`, y `default.farm` y, si es necesario, realice los cambios relevantes en los archivos personalizados que se derivan de estos archivos.

+ Vuelva a validar las configuraciones y debería aprobarse

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

El `host.docker.internal` es un nombre de host proporcionado al contenedor Docker que se resuelve en el host. Por docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> A partir de Docker 18.03, la recomendación es conectarse al nombre DNS especial host.docker.internal, que se resuelve en la dirección IP interna utilizada por el host

Cuándo `bin/docker_run src host.docker.internal:4503 8080` resultados en el mensaje __Esperando hasta que host.docker.internal esté disponible__ y luego:

1. Asegúrese de que la versión instalada de Docker sea 18.03 o buena
1. Es posible que tenga configurado un equipo local que impida el registro/resolución del `host.docker.internal` nombre. En su lugar, utilice su IP local.

>[!BEGINTABS]

>[!TAB macOS]

+ Desde el terminal, ejecute `ifconfig` y registrar el host __inet__ Dirección IP, normalmente el __en0__ dispositivo.
+ A continuación, ejecutar `docker_run` uso de la dirección IP del host: `$ bin/docker_run.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ Desde el Símbolo del sistema, ejecute `ipconfig`y registre el del host. __Dirección IPv4__ del equipo host.
+ A continuación, ejecute `docker_run` con esta dirección IP: `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux]

+ Desde el terminal, ejecute `ifconfig` y registrar el host __inet__ Dirección IP, normalmente el __en0__ dispositivo.
+ A continuación, ejecutar `docker_run` uso de la dirección IP del host: `$ bin/docker_run.sh src <HOST IP>:4503 8080`

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

+ [AEM Descarga de SDK de](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [AEM Descargar el sitio web de referencia de la (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentación de Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=es)
