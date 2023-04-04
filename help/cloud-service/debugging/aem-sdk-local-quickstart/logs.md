---
title: Depuración AEM SDK mediante registros
description: Los registros actúan como primera línea para depurar aplicaciones AEM, pero dependen del registro adecuado en la aplicación de AEM implementada.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# Depuración AEM SDK mediante registros

Al acceder a los registros del SDK de AEM, tanto el SDK local de AEM de inicio rápido Jar como las herramientas de Dispatcher pueden proporcionar perspectivas clave sobre la depuración de las aplicaciones de AEM.

## Registros de AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

Los registros actúan como primera línea para depurar aplicaciones AEM, pero dependen del registro adecuado en la aplicación de AEM implementada. Adobe recomienda mantener el desarrollo local y AEM configuraciones de registro de desarrollo as a Cloud Service como sea posible, ya que normaliza la visibilidad del registro en los entornos de inicio rápido locales del SDK de AEM y en los entornos de desarrollo de AEM as a Cloud Service, lo que reduce la manipulación y la reimplementación de la configuración.

La variable [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) configura el registro en el nivel de depuración para los paquetes Java de su aplicación AEM para el desarrollo local a través de la configuración OSGi de Sling Logger que se encuentra en

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que inicia sesión en el `error.log`.

Si el registro predeterminado no es suficiente para el desarrollo local, el registro ad hoc se puede configurar mediante la consola web local de inicio rápido del SDK, en ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), sin embargo, no se recomienda que los cambios ad hoc se mantengan en Git a menos que estas mismas configuraciones de registro se necesiten también en AEM entornos de desarrollo as a Cloud Service. Tenga en cuenta que los cambios realizados a través de la consola de soporte de registro se mantienen directamente en el repositorio local de inicio rápido del SDK AEM.

Las instrucciones de registro de Java se pueden ver en la sección `error.log` archivo:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

A menudo es útil &quot;seguir&quot; el `error.log` que transmite su salida al terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows requiere [Aplicaciones de cola de terceros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) o el uso de [Comando Get-Content de Powershell](https://stackoverflow.com/a/46444596/133936).

## Registros de Dispatcher

Los registros de Dispatcher se emiten para stdout cuando `bin/docker_run` se invoca, sin embargo los registros pueden tener acceso directo con en el contenedor Docker.

### Acceso a los registros en el contenedor Docker{#dispatcher-tools-access-logs}

Los registros de Dispatcher pueden acceder directamente al contenedor Docker en `/etc/httpd/logs`.

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

_La variable `<CONTAINER ID>` en `docker exec -it <CONTAINER ID> /bin/sh` debe reemplazarse por el ID del CONTENEDOR del Docker de destino enumerado en el `docker ps` comando._


### Copia de los registros de Docker al sistema de archivos local{#dispatcher-tools-copy-logs}

Los registros de Dispatcher se pueden copiar fuera del contenedor de Docker en `/etc/httpd/logs` al sistema de archivos local para su inspección con su herramienta favorita de análisis de registros. Tenga en cuenta que se trata de una copia puntual y no proporciona actualizaciones en tiempo real de los registros.

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

_La variable `<CONTAINER_ID>` en `docker cp <CONTAINER_ID>:/var/log/apache2 ./` debe reemplazarse por el ID del CONTENEDOR del Docker de destino enumerado en el `docker ps` comando._
