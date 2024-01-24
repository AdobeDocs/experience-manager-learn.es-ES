---
title: AEM Depuración del SDK de mediante registros
description: AEM AEM Los registros actúan como primera línea para la depuración de aplicaciones de, pero dependen del inicio de sesión adecuado en la aplicación de la aplicación de la aplicación de la aplicación implementada.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 430
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# AEM Depuración del SDK de mediante registros

AEM AEM AEM Al acceder a los registros del SDK de la, ya sea el Jar de inicio rápido local del SDK de la aplicación de la aplicación de la aplicación de la aplicación de la o las herramientas de Dispatcher, puede proporcionar información clave sobre la depuración de las aplicaciones de la aplicación.

## AEM Registros de

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

AEM AEM Los registros actúan como primera línea para la depuración de aplicaciones de, pero dependen del inicio de sesión adecuado en la aplicación de la aplicación de la aplicación de la aplicación implementada. Adobe AEM recomienda mantener las configuraciones de desarrollo local y registro de desarrollo as a Cloud Service AEM AEM as a Cloud Service de la manera más similar posible, ya que normaliza la visibilidad del registro en los entornos de desarrollo de inicio rápido local y de desarrollo de la aplicación del SDK de la, lo que reduce la alternancia y la reimplementación de la configuración.

El [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype) AEM configura el registro en el nivel de DEPURACIÓN para los paquetes Java de la aplicación de la aplicación para el desarrollo local mediante la configuración OSGi del registrador de Sling que se encuentra en

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que inicia sesión en `error.log`.

AEM Si el registro predeterminado no es suficiente para el desarrollo local, se puede configurar el registro ad hoc a través de la consola web de soporte de registros local de inicio rápido del SDK, en ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)AEM ), sin embargo, no se recomienda que se mantengan los cambios ad hoc en Git a menos que estas mismas configuraciones de registro también sean necesarias en entornos de desarrollo as a Cloud Service de la. AEM Tenga en cuenta que los cambios realizados a través de la consola de compatibilidad de registros se conservan directamente en el repositorio de inicio rápido local del SDK de la.

Las sentencias de registro de Java se pueden ver en `error.log` archivo:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

A menudo, es útil &quot;seguir&quot; el `error.log` que transmite su salida al terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows requiere [aplicaciones de cola de terceros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) o el uso de [Comando Get-Content de Powershell](https://stackoverflow.com/a/46444596/133936).

## Registros de Dispatcher

Los registros de Dispatcher se envían a stdout cuando `bin/docker_run` se invoca, pero se puede acceder directamente a los registros con en el contenedor Docker.

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

_El `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` debe reemplazarse por el ID del CONTENEDOR de Docker de destino indicado en la `docker ps` comando._


### Copiar los registros de Docker al sistema de archivos local{#dispatcher-tools-copy-logs}

Los registros de Dispatcher se pueden copiar desde el contenedor de Docker en `/etc/httpd/logs` Vaya al sistema de archivos local para inspeccionarlo con su herramienta de análisis de registros favorita. Tenga en cuenta que se trata de una copia puntual y no proporciona actualizaciones en tiempo real de los registros.

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

_El `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` debe reemplazarse por el ID del CONTENEDOR de Docker de destino indicado en la `docker ps` comando._
