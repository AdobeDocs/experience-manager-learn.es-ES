---
title: Depuración de AEM SDK mediante registros
description: Los registros actúan como primera línea para depurar aplicaciones de AEM, pero dependen del registro adecuado en la aplicación de AEM implementada.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 411
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Depuración de AEM SDK mediante registros

Al acceder a los registros de AEM SDK, tanto el Jar de inicio rápido local de AEM SDK como las herramientas de Dispatcher pueden proporcionar perspectivas clave en la depuración de aplicaciones de AEM.

## Registros de AEM

>[!VIDEO](https://video.tv.adobe.com/v/38118?quality=12&learn=on&captions=spa)

Los registros actúan como primera línea para depurar aplicaciones de AEM, pero dependen del registro adecuado en la aplicación de AEM implementada. Adobe recomienda mantener las configuraciones de desarrollo local y registro de desarrollo de AEM as a Cloud Service lo más similares posible, ya que normaliza la visibilidad del registro en los entornos de inicio rápido local y desarrollo de AEM as a Cloud Service de AEM SDK, lo que reduce la alternancia y la reimplementación de la configuración.

El [tipo de archivo del proyecto de AEM](https://github.com/adobe/aem-project-archetype) configura el registro en el nivel DEBUG para los paquetes Java de la aplicación AEM para el desarrollo local mediante la configuración OSGi del registrador de Sling que se encuentra en

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que registra a `error.log`.

Si el registro predeterminado no es suficiente para el desarrollo local, se puede configurar el registro ad hoc a través de la consola web de soporte de registros de Quickstart local de AEM SDK, en ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)); sin embargo, no se recomienda que se mantengan los cambios ad hoc en Git a menos que también se necesiten estas mismas configuraciones de registro en los entornos de desarrollo de AEM as a Cloud Service. Tenga en cuenta que los cambios a través de la consola de Log Support se mantienen directamente en el repositorio de inicio rápido local de AEM SDK.

Las instrucciones de registro de Java se pueden ver en el archivo `error.log`:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

A menudo resulta útil &quot;rastrear&quot; al `error.log` que transmite su salida al terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows requiere [aplicaciones de cola de terceros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) o el uso del [comando Get-Content de Powershell](https://stackoverflow.com/a/46444596/133936).

## Registros de Dispatcher

Los registros de Dispatcher se muestran en stdout cuando se invoca `bin/docker_run`, pero se puede acceder directamente a los registros con en el contenedor Docker.

### Acceder a los registros del contenedor Docker{#dispatcher-tools-access-logs}

Se puede acceder directamente a los registros de Dispatcher en el contenedor Docker en `/etc/httpd/logs`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_El `<CONTAINER ID>` de `docker exec -it <CONTAINER ID> /bin/sh` debe reemplazarse con el ID. de contenedor de Docker de destino indicado en el comando `docker ps`._


### Copiar los registros de Docker al sistema de archivos local{#dispatcher-tools-copy-logs}

Los registros de Dispatcher se pueden copiar del contenedor Docker en `/etc/httpd/logs` al sistema de archivos local para inspeccionarlos con su herramienta de análisis de registros favorita. Tenga en cuenta que se trata de una copia puntual y no proporciona actualizaciones en tiempo real de los registros.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_El `<CONTAINER_ID>` de `docker cp <CONTAINER_ID>:/var/log/apache2 ./` debe reemplazarse con el ID. de contenedor de Docker de destino indicado en el comando `docker ps`._
