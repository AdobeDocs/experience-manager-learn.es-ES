---
title: Depuración AEM SDK mediante registros
description: Los registros actúan como primera línea para depurar aplicaciones AEM, pero dependen del registro adecuado en la aplicación AEM implementada.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 2%

---


# Depuración AEM SDK mediante registros

Al acceder a los registros del SDK de AEM, tanto el SDK de AEM local de inicio rápido de Jar como las herramientas de Dispatcher&#39; pueden proporcionar perspectivas clave para depurar aplicaciones AEM.

## Registros de AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Los registros actúan como primera línea para depurar aplicaciones AEM, pero dependen del registro adecuado en la aplicación AEM implementada. Adobe recomienda mantener el desarrollo local y la AEM como configuraciones de registro de Cloud Service Dev de la manera más similar posible, ya que normaliza la visibilidad del registro en el inicio rápido local del SDK de AEM y la AEM como entornos de desarrolladores de Cloud Service, lo que reduce la ampliación y la reimplementación de la configuración.

El [AEM Arquetipo](https://github.com/adobe/aem-project-archetype) de proyecto configura el registro en el nivel DEBUG de los paquetes Java de la aplicación de AEM para el desarrollo local mediante la configuración OSGi de Sling Logger que se encuentra en

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que inicia sesión en el `error.log`.

Si el registro predeterminado no es suficiente para el desarrollo local, el registro ad hoc se puede configurar mediante la consola web de soporte de registro local de inicio rápido del SDK de AEM, en ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)); sin embargo, no se recomiendan los cambios ad hoc en Git a menos que se necesiten estas mismas configuraciones de registro en AEM como entornos de desarrollo Cloud Service. Tenga en cuenta que los cambios realizados a través de la consola de asistencia de registro se mantienen directamente en el repositorio local de inicio rápido del SDK de AEM.

Las sentencias de registro Java pueden ser vistas en el `error.log` archivo:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

A menudo resulta útil &quot;recortar&quot; el `error.log` que transmite su salida a la terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows requiere aplicaciones [de cola de](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) terceros o el uso del comando [Get-Content de](https://stackoverflow.com/a/46444596/133936)Powershell.

## Registros de Dispatcher

Los registros de Dispatcher se envían a stdout cuando `bin/docker_run` se invocan, pero los registros pueden acceder directamente con ellos en el contenedor de Docker.

### Acceso a los registros en el contenedor del Docker

Los registros de Dispatcher pueden acceder directamente en el contenedor de Docker en `/etc/httpd/logs`.

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

### Copia de los registros de Docker al sistema de archivos local

Los registros de Dispatcher se pueden copiar desde el contenedor de Docker en `/etc/httpd/logs` el sistema de archivos local para su inspección mediante la herramienta de análisis de registro favorita. Tenga en cuenta que se trata de una copia puntual y no proporciona actualizaciones en tiempo real de los registros.

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

