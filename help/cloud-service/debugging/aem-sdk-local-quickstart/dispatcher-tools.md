---
title: Depuración de herramientas de Dispatcher
description: Las herramientas de Dispatcher proporcionan un entorno de servidor web Apache que se puede utilizar para simular el Dispatcher del servicio AEM Publish de AEM as a Cloud Services de forma local. La depuración de los registros y el contenido de la caché de las herramientas de Dispatcher puede ser vital para garantizar que la aplicación de AEM integral y las configuraciones de caché y seguridad de soporte sean correctas.
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: Desarrollo
role: Desarrollador
level: Principiante, intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 1%

---


# Depuración de herramientas de Dispatcher

Las herramientas de Dispatcher proporcionan un entorno de servidor web Apache que se puede utilizar para simular el Dispatcher del servicio AEM Publish de AEM as a Cloud Services de forma local.
La depuración de los registros y el contenido de la caché de las herramientas de Dispatcher puede ser vital para garantizar que la aplicación de AEM integral y las configuraciones de caché y seguridad de soporte sean correctas.

>[!NOTE]
>
>Dado que las herramientas de Dispatcher se basan en contenedores, cada vez que se reinicia, se destruyen los registros y el contenido de la caché anteriores.

## Registros de herramientas de Dispatcher

Los registros de herramientas de Dispatcher están disponibles a través del comando `stdout` o `bin/docker_run`, o con más detalle, disponibles en el contenedor de Docker en `/etc/https/logs`.

Consulte [Registros de Dispatcher](./logs.md#dispatcher-logs) para obtener instrucciones sobre cómo acceder directamente a los registros del contenedor de Docker de las herramientas de Dispatcher.

## Caché de herramientas de Dispatcher

### Acceso a los registros en el contenedor Docker

La caché de Dispatcher puede acceder directamente al contenedor Docker en ` /mnt/var/www/html`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### Copia de los registros de Docker al sistema de archivos local

Los registros de Dispatcher se pueden copiar desde el contenedor Docker en `/mnt/var/www/html` al sistema de archivos local para su inspección con sus herramientas favoritas. Tenga en cuenta que se trata de una copia puntual y no proporciona actualizaciones en tiempo real de la caché.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

