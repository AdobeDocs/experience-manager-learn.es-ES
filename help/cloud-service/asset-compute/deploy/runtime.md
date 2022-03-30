---
title: Implementar Assets computes en Adobe I/O Runtime para su uso con AEM as a Cloud Service
description: Los proyectos de asset compute, y los trabajadores que contienen, deben implementarse en Adobe I/O Runtime para que los utilice AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Implementar en Adobe I/O Runtime

Los proyectos de asset compute, y los trabajadores que contienen, deben implementarse en Adobe I/O Runtime a través de la CLI de Adobe I/O para que los utilice AEM as a Cloud Service.

Al implementar en Adobe I/O Runtime para AEM uso de los servicios de Autor as a Cloud Service, solo se requieren dos variables de entorno:

+ `AIO_runtime_namespace` señala el espacio de trabajo de App Builder que se va a implementar en
+ `AIO_runtime_auth` son las credenciales de autenticación del espacio de trabajo de App Builder

Las demás variables estándar definidas en la variable `.env` el archivo lo proporciona implícitamente AEM as a Cloud Service cuando invoca al trabajador de Asset compute.

## Espacio de trabajo de desarrollo

Porque este proyecto se generó usando `aio app init` usando la variable `Development` espacio de trabajo, `AIO_runtime_namespace` se configura automáticamente como `81368-wkndaemassetcompute-development` con la coincidencia `AIO_runtime_auth` en nuestra `.env` archivo.  Si una `.env` existe en el directorio utilizado para emitir el comando deploy, se utilizan sus valores, a menos que se sustituyan por una exportación de variable a nivel del sistema operativo, que es así [fase y producción](#stage-and-production) los espacios de trabajo están dirigidos.

![implementación de aplicaciones de aio mediante variables .env](./assets/runtime/development__aio.png)

Para implementar en el espacio de trabajo, defina en los proyectos `.env` archivo:

1. Abra la línea de comandos en la raíz del proyecto de Asset compute.
1. Ejecutar el comando `aio app deploy`
1. Ejecutar el comando `aio app get-url` para obtener la URL de trabajo para usar en el perfil de procesamiento as a Cloud Service de AEM para hacer referencia a este trabajador de Asset compute personalizado. Si el proyecto contiene varios trabajadores, se muestran las direcciones URL discretas para cada trabajador.

Si los entornos de desarrollo local y AEM desarrollo as a Cloud Service utilizan implementaciones de Asset compute independientes, las implementaciones en AEM desarrollo as a Cloud Service se pueden administrar del mismo modo que [Implementaciones de fase y producción](#stage-and-production).

## Espacios de trabajo de fase y producción{#stage-and-production}

La implementación en los espacios de trabajo de fase y producción se realiza normalmente mediante el sistema de CI/CD que elija. El proyecto de Asset compute debe implementarse en cada espacio de trabajo (fase y producción) de forma discreta.

Si se establecen variables de entorno verdaderas, se sobrescriben los valores de las variables con el mismo nombre en `.env`.

![implementación de aplicaciones de aio mediante variables de exportación](./assets/runtime/stage__export-and-aio.png)

El enfoque general, normalmente automatizado por un sistema CI/CD, para la implementación en entornos de fase y producción es:

1. Asegúrese de que la variable [Módulo npm de CLI de Adobe I/O y complemento de Asset compute](../set-up/development-environment.md#aio) están instalados
1. Consulte el proyecto de Asset compute para implementar desde Git
1. Establezca las variables de entorno con los valores que corresponden al espacio de trabajo de destino (fase o producción)
   + Las dos variables requeridas son `AIO_runtime_namespace` y `AIO_runtime_auth` y se obtienen por espacio de trabajo en Adobe I/O Developer Console mediante el __Descargar todo__ función.

![Adobe Developer Console: espacio de nombres y autenticación en tiempo de ejecución de AIO](./assets/runtime/stage-auth-namespace.png)

Los valores de estas claves se pueden configurar emitiendo comandos de exportación desde la línea de comandos:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si los Assets computes requieren otras variables, como el almacenamiento en la nube, estas también deben exportarse como variables de entorno.

1. Una vez que todas las variables de entorno estén configuradas para que el espacio de trabajo de destino implemente en, ejecute el comando deploy:
   + `aio app deploy`
1. Las URL de trabajo a las que hace referencia el perfil de procesamiento as a Cloud Service de AEM también están disponibles mediante:
   + `aio app get-url`.

Si la versión del proyecto de Asset compute cambia, las direcciones URL de trabajo también cambian para reflejar la nueva versión y la URL deberá actualizarse en los perfiles de procesamiento.

## Aprovisionamiento de API de Workspace{#workspace-api-provisioning}

When [configuración del proyecto de App Builder en Adobe I/O](../set-up/app-builder.md) para apoyar el desarrollo local, se creó un nuevo espacio de trabajo de desarrollo y __asset compute, eventos de E/S__ y __API de administración de eventos de E/S__ se agregaron a él.

La variable __asset compute, eventos de E/S__ y __API de administración de eventos de E/S__ Las API solo se añaden explícitamente a los espacios de trabajo utilizados para el desarrollo local. Los espacios de trabajo que se integran (exclusivamente) con AEM entornos as a Cloud Service sí __not__ necesitan estas API añadidas explícitamente, ya que las API están disponibles de forma natural para AEM as a Cloud Service.
