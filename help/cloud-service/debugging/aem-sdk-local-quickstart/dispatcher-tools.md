---
title: Depuración de herramientas de Dispatcher
description: Las herramientas de Dispatcher proporcionan un entorno de servidor web Apache que se puede utilizar para simular AEM como Dispatcher del servicio de publicación de AEM de Cloud Services localmente. La depuración de los registros y el contenido de la caché de las herramientas de Dispatcher puede ser vital para garantizar que la aplicación de AEM end-to-end y las configuraciones de caché y seguridad de soporte sean correctas.
feature: Dispatcher
kt: 5918
topic: Development
role: Developer
level: Beginner, Intermediate
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---


# Depuración de herramientas de Dispatcher

Las herramientas de Dispatcher proporcionan un entorno de servidor web Apache que se puede utilizar para simular AEM como Dispatcher del servicio de publicación de AEM de Cloud Services localmente.

La depuración de los registros y el contenido de la caché de las herramientas de Dispatcher puede ser vital para garantizar que la aplicación de AEM end-to-end y las configuraciones de caché y seguridad de soporte sean correctas.

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
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

