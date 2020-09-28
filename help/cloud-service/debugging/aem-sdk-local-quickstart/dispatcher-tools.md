---
title: Herramientas de Distribuidor de Depuración
description: Dispatcher Tools proporciona un entorno de servidor web Apache que se puede utilizar para simular AEM como Dispatcher del servicio AEM Publish de Cloud Services localmente. La depuración de los registros y el contenido de la caché de Dispatcher Tools puede ser vital para garantizar que la aplicación de AEM end-to-end y las configuraciones de seguridad y caché admitidas sean correctas.
feature: dispatcher, aem-sdk
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# Herramientas de Distribuidor de Depuración

Dispatcher Tools proporciona un entorno de servidor web Apache que se puede utilizar para simular AEM como Dispatcher del servicio AEM Publish de Cloud Services localmente.
La depuración de los registros y el contenido de la caché de Dispatcher Tools puede ser vital para garantizar que la aplicación de AEM end-to-end y las configuraciones de seguridad y caché admitidas sean correctas.

>[!NOTE]
>
>Dado que Dispatcher Tools se basa en contenedores, cada vez que se reinicia, se destruyen los registros anteriores y el contenido de la caché.

## Registros de herramientas de despachador

Los registros de herramientas de despachante están disponibles mediante el comando `stdout` o el `bin/docker_run` , o con más detalle, disponibles en el contenedor de acoplamiento en `/etc/https/logs`.

Consulte [Registros](./logs.md#dispatcher-logs) de Dispatcher para obtener instrucciones sobre cómo acceder directamente a los registros del contenedor del Docker de Dispatcher Tools.

## Caché de herramientas de despachante

### Acceso a los registros en el contenedor del Docker

La caché de Dispatcher puede acceder directamente en el contenedor de Docker en ` /mnt/var/www/html`.

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

Los registros de Dispatcher se pueden copiar desde el contenedor de Docker en `/mnt/var/www/html` el sistema de archivos local para inspeccionar con sus herramientas favoritas. Tenga en cuenta que se trata de una copia puntual y no proporciona actualizaciones en tiempo real en la caché.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

