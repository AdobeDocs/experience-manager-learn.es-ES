---
title: Implementación de trabajadores de Asset Compute en Adobe I/O Runtime para su uso con AEM as a Cloud Service
description: 'Los proyectos de Asset Compute y los trabajadores que contienen deben implementarse en Adobe I/O Runtime para que los use AEM as a Cloud Service. '
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---


# Implementar en Adobe I/O Runtime

Los proyectos de Asset Compute y los trabajadores que contienen deben implementarse en Adobe I/O Runtime mediante la CLI de Adobe I/O para que la utilice AEM as a Cloud Service.

Al implementar en Adobe I/O Runtime para uso de AEM as a Cloud Service Author Services, solo se requieren dos variables de entorno:

+ `AIO_runtime_namespace` señala el espacio de trabajo de Adobe Project Firefly que se va a implementar en
+ `AIO_runtime_auth` son las credenciales de autenticación del espacio de trabajo de Adobe Project Firefly

Las otras variables estándar definidas en el archivo `.env` son proporcionadas implícitamente por AEM as a Cloud Service cuando invoca el trabajador de Asset Compute.

## Espacio de trabajo de desarrollo

Dado que este proyecto se generó utilizando `aio app init` utilizando el espacio de trabajo `Development` , `AIO_runtime_namespace` se configura automáticamente como `81368-wkndaemassetcompute-development` con el `AIO_runtime_auth` coincidente en nuestro archivo `.env` local.  Si existe un archivo `.env` en el directorio utilizado para emitir el comando deploy, se utilizan sus valores, a menos que se sustituyan mediante una exportación de variables de nivel del sistema operativo, que es el modo en que se dirigen los espacios de trabajo [stage y production](#stage-and-production).

![implementación de aplicaciones de aio mediante variables .env](./assets/runtime/development__aio.png)

Para implementar en el espacio de trabajo, defina en el archivo `.env` de proyectos:

1. Abra la línea de comandos en la raíz del proyecto Asset Compute.
1. Ejecute el comando `aio app deploy`
1. Ejecute el comando `aio app get-url` para obtener la URL de trabajo para utilizarla en el perfil de procesamiento de AEM as a Cloud Service para hacer referencia a este trabajador personalizado de Asset Compute. Si el proyecto contiene varios trabajadores, se muestran las direcciones URL discretas para cada trabajador.

Si los entornos de desarrollo de AEM as a Cloud Service utilizan implementaciones separadas de Asset Compute, las implementaciones en AEM as a Cloud Service Dev se pueden administrar del mismo modo que las implementaciones de [fase y producción](#stage-and-production).

## Espacios de trabajo de fase y producción{#stage-and-production}

La implementación en los espacios de trabajo de fase y producción se realiza normalmente mediante el sistema de CI/CD que elija. El proyecto Asset Compute debe implementarse en cada espacio de trabajo (fase y producción) de forma discreta.

La configuración de variables de entorno verdaderas anula los valores de las variables con el mismo nombre en `.env`.

![implementación de aplicaciones de aio mediante variables de exportación](./assets/runtime/stage__export-and-aio.png)

El enfoque general, normalmente automatizado por un sistema CI/CD, para la implementación en entornos de fase y producción es:

1. Asegúrese de que el módulo [Adobe I/O CLI npm y el complemento Asset Compute](../set-up/development-environment.md#aio) estén instalados
1. Consulte el proyecto de Asset Compute para implementar desde Git
1. Establezca las variables de entorno con los valores que corresponden al espacio de trabajo de destino (fase o producción)
   + Las dos variables requeridas son `AIO_runtime_namespace` y `AIO_runtime_auth` y se obtienen por espacio de trabajo en Adobe I/O Developer Console mediante la función __Descargar todo__ de Workspace.

![Adobe Developer Console: Espacio de nombres y autenticación en tiempo de ejecución de AIO](./assets/runtime/stage-auth-namespace.png)

Los valores de estas claves se pueden configurar emitiendo comandos de exportación desde la línea de comandos:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si los trabajadores de Asset Compute requieren otras variables, como el almacenamiento en la nube, estas también deben exportarse como variables de entorno.

1. Una vez que todas las variables de entorno estén configuradas para que el espacio de trabajo de destino implemente en, ejecute el comando deploy:
   + `aio app deploy`
1. Las URL de trabajo a las que hace referencia el perfil de procesamiento de AEM as a Cloud Service también están disponibles mediante:
   + `aio app get-url`.

Si la versión del proyecto de Asset Compute cambia, las direcciones URL de trabajo también cambian para reflejar la nueva versión y la URL deberá actualizarse en Perfiles de procesamiento.

## Aprovisionamiento de API de Workspace{#workspace-api-provisioning}

Cuando [configuró el proyecto de Adobe Project Firefly en Adobe I/O](../set-up/firefly.md) para admitir el desarrollo local, se creó un nuevo espacio de trabajo de desarrollo y se agregaron __Asset Compute, I/O Events__ y __API de administración de eventos de E/S__.

Las API __Asset Compute, I/O Events__ y __I/O Events Management API__ solo se agregan explícitamente a los espacios de trabajo utilizados para el desarrollo local. Los espacios de trabajo que se integran (exclusivamente) con entornos de AEM as a Cloud Service __no__ necesitan estas API explícitamente añadidas, ya que las API están disponibles de forma natural para AEM as a Cloud Service.
