---
title: Depuración del SDK de AEM mediante registros
description: Los registros actúan como primera línea para depurar aplicaciones AEM, pero dependen del registro adecuado en la aplicación AEM implementada.
feature: Herramientas para desarrolladores
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Desarrollo
role: Desarrollador
level: Principiante, intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---


# Depuración del SDK de AEM mediante registros

Al acceder a los registros del SDK de AEM, el Jar local de inicio rápido del SDK de AEM o las herramientas de Dispatcher&#39; pueden proporcionar perspectivas clave para depurar aplicaciones de AEM.

## Registros de AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Los registros actúan como primera línea para depurar aplicaciones AEM, pero dependen del registro adecuado en la aplicación AEM implementada. Adobe recomienda mantener las configuraciones de registro de desarrollo local y AEM as a Cloud Service Dev de la forma más similar posible, ya que normaliza la visibilidad del registro en los entornos de inicio rápido locales del SDK de AEM y de desarrollo de AEM as a Cloud Service, lo que reduce la vinculación y la reimplementación de la configuración.

El [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) configura el registro en el nivel de depuración para los paquetes Java de la aplicación AEM para el desarrollo local a través de la configuración OSGi de Sling Logger que se encuentra en

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que inicia sesión en `error.log`.

Si el registro predeterminado no es suficiente para el desarrollo local, el registro ad hoc se puede configurar a través de la consola web local de soporte de registro del SDK de AEM, en ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), pero no se recomiendan los cambios ad hoc en Git a menos que también se necesiten estas mismas configuraciones de registro en AEM como entornos de desarrollo de Cloud Service. Tenga en cuenta que los cambios realizados a través de la consola de soporte de registro se mantienen directamente en el repositorio local de inicio rápido del SDK de AEM.

Las instrucciones de registro de Java se pueden ver en el archivo `error.log`:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

A menudo es útil &quot;seguir&quot; el `error.log` que transmite su salida al terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows requiere [aplicaciones de cola de terceros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) o el uso del comando Get-Content de [Powershell](https://stackoverflow.com/a/46444596/133936).

## Registros de Dispatcher

Los registros de Dispatcher se emiten para stdout cuando se invoca `bin/docker_run`, pero se puede acceder directamente a los registros en el contenedor de Docker.

### Acceso a los registros en el contenedor de Docker{#dispatcher-tools-access-logs}

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

_El  `<CONTAINER ID>` en  `docker exec -it <CONTAINER ID> /bin/sh` debe reemplazarse por el ID del CONTENEDOR del Docker de destino que aparece en el  `docker ps` comando._


### Copia de los registros de Docker al sistema de archivos local{#dispatcher-tools-copy-logs}

Los registros de Dispatcher se pueden copiar desde el contenedor Docker en `/etc/httpd/logs` al sistema de archivos local para su inspección usando su herramienta favorita de análisis de registros. Tenga en cuenta que se trata de una copia puntual y no proporciona actualizaciones en tiempo real de los registros.

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

_El  `<CONTAINER_ID>` en  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` debe reemplazarse por el ID del CONTENEDOR del Docker de destino que aparece en el  `docker ps` comando._
