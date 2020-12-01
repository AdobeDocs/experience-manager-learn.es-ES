---
title: Implementar trabajadores de Asset compute en Adobe I/O Runtime para usarlos con AEM como Cloud Service
description: 'Los proyectos de asset compute, y los trabajadores que contienen, deben enviarse a Adobe I/O Runtime para que los AEM los utilicen como Cloud Service. '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---


# Implementar en Adobe I/O Runtime

Los proyectos de asset compute, y los trabajadores que contienen, deben implementarse en Adobe I/O Runtime a través de Adobe I/O CLI para que los AEM los usen como Cloud Service.

Al implementar en Adobe I/O Runtime para su uso por AEM como servicios de Cloud Service Author, solo se requieren dos variables de entorno:

+ `AIO_runtime_namespace` señala el área de trabajo de búsqueda del proyecto de Adobe en la que implementar
+ `AIO_runtime_auth` son las credenciales de autenticación del espacio de trabajo de búsqueda del proyecto de Adobe

Las demás variables estándar definidas en el archivo `.env` son proporcionadas implícitamente por AEM como Cloud Service cuando invoca al trabajador de Asset compute.

## Espacio de trabajo de desarrollo

Debido a que este proyecto se generó usando `aio app init` el espacio de trabajo `Development`, `AIO_runtime_namespace` se configura automáticamente en `81368-wkndaemassetcompute-development` con la coincidencia `AIO_runtime_auth` en nuestro archivo `.env` local.  Si existe un archivo `.env` en el directorio utilizado para ejecutar el comando de implementación, se utilizan sus valores, a menos que se sustituyan por una exportación de variable de nivel de SO, que es la manera en que se dirigen los espacios de trabajo [stage y production](#stage-and-production).

![implementación de la aplicación de AIO mediante variables .env](./assets/runtime/development__aio.png)

Para implementar en el área de trabajo, defina en el archivo `.env` de proyectos:

1. Abrir la línea de comandos en la raíz del proyecto de Asset compute
1. Ejecutar el comando `aio app deploy`
1. Ejecute el comando `aio app get-url` para obtener la dirección URL del programa de trabajo para utilizarla en el AEM como Perfil de procesamiento de Cloud Service para hacer referencia a este programa de trabajo de Assets computes personalizado. Si el proyecto contiene varios trabajadores, se muestran las direcciones URL discretas de cada trabajador.

Si el desarrollo local y la AEM como entornos de desarrollo Cloud Service utilizan implementaciones de Asset compute independientes, las implementaciones en AEM como Cloud Service Dev se pueden administrar de la misma manera que las [implementaciones de etapa y producción](#stage-and-production).

## Espacios de trabajo de etapa y producción{#stage-and-production}

La implementación en los espacios de trabajo de fase y producción suele realizarse mediante el sistema de CI/CD que elija. El proyecto de Asset compute debe implementarse en cada espacio de trabajo (fase y producción) de forma discreta.

Al establecer las variables de entorno verdaderas se anulan los valores de las variables con el mismo nombre en `.env`.

![implementación de la aplicación de aio mediante variables de exportación](./assets/runtime/stage__export-and-aio.png)

El enfoque general, normalmente automatizado por un sistema CI/CD, para la implementación en entornos de fase y producción es:

1. Asegúrese de que el [módulo npm de Adobe I/O CLI y el complemento de Asset compute](../set-up/development-environment.md#aio) están instalados
1. Consulte el proyecto de Asset compute para implementar desde Git
1. Configure las variables de entorno con los valores que corresponden al espacio de trabajo de destinatario (fase o producción)
   + Las dos variables requeridas son `AIO_runtime_namespace` y `AIO_runtime_auth` y se obtienen por espacio de trabajo en Adobe I/O Developer Console mediante la función __Descargar todo__ del espacio de trabajo.

![Adobe Developer Console: Área de nombres y autenticación de AIO Runtime](./assets/runtime/stage-auth-namespace.png)

Los valores de estas claves se pueden configurar mediante la ejecución de comandos de exportación desde la línea de comandos:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si los trabajadores de la Asset compute requieren otras variables, como por ejemplo en el almacenamiento de nube, éstas también deben exportarse como variables de entorno.

1. Una vez definidas todas las variables de entorno para el espacio de trabajo de destinatario en el que se va a implementar, ejecute el comando de implementación:
   + `aio app deploy`
1. Las direcciones URL de trabajo a las que hace referencia el AEM como Perfil de procesamiento de Cloud Service también están disponibles mediante:
   + `aio app get-url`.

Si la versión del proyecto de Asset compute cambia, las direcciones URL del programa de trabajo también cambian para reflejar la nueva versión y la dirección URL deberá actualizarse en los Perfiles de procesamiento.

## Aprovisionamiento de API de espacio de trabajo{#workspace-api-provisioning}

Cuando [configuró el proyecto de Adobe Project Firefly en Adobe I/O](../set-up/firefly.md) para admitir el desarrollo local, se creó un nuevo espacio de trabajo de desarrollo y se agregaron __Asset compute, Eventos de E/S__ y __API de administración de Eventos de E/S__.

Las API de __Asset compute, Eventos de E/S__ y __Administración de Eventos de E/S__ sólo se agregan explícitamente a los espacios de trabajo utilizados para el desarrollo local. Los espacios de trabajo que se integran (exclusivamente) con AEM como entornos Cloud Service __no__ necesitan estas API agregadas explícitamente ya que las API se ponen naturalmente a disposición de AEM como Cloud Service.
