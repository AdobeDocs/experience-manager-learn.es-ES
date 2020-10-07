---
title: Implementar trabajadores de Asset Compute en Adobe I/O Runtime para utilizarlos con AEM como Cloud Service
description: 'Los proyectos de cómputo de recursos y los trabajadores que contienen deben implementarse en Adobe I/O Runtime para que los AEM los utilicen como Cloud Service. '
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

Los proyectos de cómputo de recursos, y los trabajadores que contienen, deben implementarse en Adobe I/O Runtime mediante la CLI de E/S de Adobe para que AEM los use como Cloud Service.

Al implementar en Adobe I/O Runtime para su uso por AEM como servicios de Cloud Service Author, solo se requieren dos variables de entorno:

+ `AIO_runtime_namespace` señala el área de trabajo de búsqueda del proyecto de Adobe en la que implementar
+ `AIO_runtime_auth` son las credenciales de autenticación del espacio de trabajo de búsqueda del proyecto de Adobe

Las demás variables estándar definidas en el `.env` archivo son proporcionadas implícitamente por AEM como Cloud Service cuando invoca al trabajador de cómputo de recursos.

## Espacio de trabajo de desarrollo

Debido a que este proyecto se generó usando `aio app init` el `Development` espacio de trabajo, `AIO_runtime_namespace` se configura automáticamente `81368-wkndaemassetcompute-development` con la coincidencia `AIO_runtime_auth` en nuestro `.env` archivo local.  Si existe un `.env` archivo en el directorio utilizado para ejecutar el comando de implementación, se utilizan sus valores, a menos que se sustituyan por una exportación de variable de nivel de SO, que es la manera en que se dirigen los espacios de trabajo de [etapa y producción](#stage-and-production) .

![implementación de la aplicación de AIO mediante variables .env](./assets/runtime/development__aio.png)

Para implementar en el espacio de trabajo, defina en el archivo de proyectos `.env` :

1. Abra la línea de comandos en la raíz del proyecto de cómputo de recursos
1. Ejecutar el comando `aio app deploy`
1. Ejecute el comando `aio app get-url` para obtener la dirección URL del programa de trabajo para utilizarla en el AEM como Perfil de procesamiento de Cloud Service para hacer referencia a este programa de trabajo de cómputo de recursos personalizado. Si el proyecto contiene varios trabajadores, se muestran las direcciones URL discretas de cada trabajador.

Si el desarrollo local y la AEM como entornos de desarrollo Cloud Service utilizan implementaciones separadas de Asset Compute, las implementaciones en AEM como Cloud Service Dev se pueden administrar de la misma manera que las implementaciones [de](#stage-and-production)Stage y Production.

## Espacios de trabajo de fase y producción{#stage-and-production}

La implementación en los espacios de trabajo de fase y producción suele realizarse mediante el sistema de CI/CD que elija. El proyecto de cómputo de recursos debe implementarse en cada espacio de trabajo (fase y producción) de forma discreta.

La configuración de las variables de entorno reales anula los valores de las variables con el mismo nombre en `.env`.

![implementación de la aplicación de aio mediante variables de exportación](./assets/runtime/stage__export-and-aio.png)

El enfoque general, normalmente automatizado por un sistema CI/CD, para la implementación en entornos de fase y producción es:

1. Asegúrese de que el módulo npm de CLI de E/S de [Adobe y el complemento](../set-up/development-environment.md#aio) Asset Compute estén instalados
1. Consulte el proyecto de cómputo de recursos para implementar desde Git
1. Configure las variables de entorno con los valores que corresponden al espacio de trabajo de destinatario (fase o producción)
   + Las dos variables requeridas son `AIO_runtime_namespace` y `AIO_runtime_auth` se obtienen por espacio de trabajo en la consola del desarrollador de Adobe I/O mediante la función __Descargar todo__ del espacio de trabajo.

![Adobe Developer Console: Área de nombres y autenticación de AIO Runtime](./assets/runtime/stage-auth-namespace.png)

Los valores de estas claves se pueden configurar mediante la ejecución de comandos de exportación desde la línea de comandos:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si los trabajadores de la computación de recursos requieren otras variables, como en el almacenamiento en la nube, éstas también deben exportarse como variables de entorno.

1. Una vez definidas todas las variables de entorno para el espacio de trabajo de destinatario en el que se va a implementar, ejecute el comando de implementación:
   + `aio app deploy`
1. Las direcciones URL de trabajo a las que hace referencia el AEM como Perfil de procesamiento de Cloud Service también están disponibles mediante:
   + `aio app get-url`.

Si la versión del proyecto de cálculo de recursos cambia, las direcciones URL de trabajo también cambian para reflejar la nueva versión y la dirección URL deberá actualizarse en las Perfiles de procesamiento.

## Aprovisionamiento de API de Workspace{#workspace-api-provisioning}

Al [configurar el proyecto Adobe Project Firefly en E/S](../set-up/firefly.md) Adobe para admitir el desarrollo local, se creó un nuevo espacio de trabajo de desarrollo y se agregaron __Asset Compute, Eventos__ de E/S y API __de administración de Eventos de__ E/S.

Las API de __Asset Compute, Eventos__ de E/S y administración de Eventos de __E/S__ solo se agregan explícitamente a los espacios de trabajo utilizados para el desarrollo local. Los espacios de trabajo que se integran (exclusivamente) con AEM como entornos Cloud Service __no necesitan__ que estas API se agreguen explícitamente, ya que las API se ponen naturalmente a disposición de AEM como Cloud Service.
