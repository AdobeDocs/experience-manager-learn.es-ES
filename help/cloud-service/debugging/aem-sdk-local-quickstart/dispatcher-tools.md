---
title: Depuración de herramientas de Dispatcher
description: AEM Las herramientas de Dispatcher proporcionan un entorno de servidor web Apache en contenedores que se puede utilizar para simular el uso de los recursos como un Cloud Service AEM de manera local, como Dispatcher de un servicio de publicación de. AEM La depuración de los registros y el contenido de la caché de las herramientas de Dispatcher puede ser vital para garantizar que la aplicación de extremo a extremo y las configuraciones de caché y seguridad de soporte sean correctas.
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Depuración de herramientas de Dispatcher

AEM Las herramientas de Dispatcher proporcionan un entorno de servidor web Apache en contenedores que se puede utilizar para simular el uso de los recursos como un Cloud Service AEM de manera local, como Dispatcher de un servicio de publicación de.

AEM La depuración de los registros y el contenido de la caché de las herramientas de Dispatcher puede ser vital para garantizar que la aplicación de extremo a extremo y las configuraciones de caché y seguridad de soporte sean correctas.

>[!NOTE]
>
>Dado que las herramientas de Dispatcher se basan en contenedores, cada vez que se reinicia, se destruyen los registros anteriores y el contenido de la caché.

## Registros de herramientas de Dispatcher

Los registros de las herramientas de Dispatcher están disponibles a través de `stdout` o el `bin/docker_run` o con más detalles, disponible en el contenedor Docker en `/etc/https/logs`.

Consulte [Registros de Dispatcher](./logs.md#dispatcher-logs) para obtener instrucciones sobre cómo acceder directamente a los registros del contenedor Docker de las herramientas de Dispatcher.

## Caché de herramientas de Dispatcher

### Acceder a los registros del contenedor Docker

Se puede acceder directamente a la caché de Dispatcher en el contenedor Docker en ` /mnt/var/www/html`.

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

### Copiar los registros de Docker al sistema de archivos local

Los registros de Dispatcher se pueden copiar desde el contenedor de Docker en `/mnt/var/www/html` al sistema de archivos local para su inspección con sus herramientas favoritas. Tenga en cuenta que se trata de una copia puntual y no proporciona actualizaciones en tiempo real a la caché.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
