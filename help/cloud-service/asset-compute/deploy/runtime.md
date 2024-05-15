---
title: Implementación de trabajadores de Asset compute en Adobe I/O Runtime AEM as a Cloud Service para su uso con el
description: Los proyectos de asset compute, y los recursos que contienen, deben implementarse en Adobe I/O Runtime AEM para que los utilicen los as a Cloud Service de la aplicación de la aplicación de la.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 128
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Implementación en Adobe I/O Runtime

Los proyectos de asset compute, y los trabajadores que contienen, deben implementarse en Adobe I/O Runtime a través de la CLI de Adobe I/O AEM para que los utilicen los as a Cloud Service de la.

Al implementar en Adobe I/O Runtime AEM para que lo utilicen los servicios de autor as a Cloud Service de la aplicación, solo se requieren dos variables de entorno:

+ `AIO_runtime_namespace` señala el espacio de trabajo del generador de aplicaciones para implementar en
+ `AIO_runtime_auth` son las credenciales de autenticación del espacio de trabajo del App Builder

Las demás variables estándar definidas en la variable `.env` AEM as a Cloud Service proporciona implícitamente los archivos cuando invoca al trabajador de Asset compute.

## Espacio de trabajo Desarrollo

Dado que este proyecto se generó utilizando `aio app init` uso del `Development` workspace, `AIO_runtime_namespace` se establece automáticamente como `81368-wkndaemassetcompute-development` con el correspondiente `AIO_runtime_auth` en nuestro local `.env` archivo.  Si un `.env` El archivo existe en el directorio utilizado para emitir el comando deploy, y se utilizan sus valores, a menos que se reemplacen mediante una exportación de variables de nivel del sistema operativo, que es cómo [fase y producción](#stage-and-production) los espacios de trabajo son específicos.

![implementación de aplicaciones aio mediante variables .env](./assets/runtime/development__aio.png)

Para implementar en el espacio de trabajo definido en los proyectos `.env` archivo:

1. Abra la línea de comandos en la raíz del proyecto de Asset compute
1. Ejecutar el comando `aio app deploy`
1. Ejecutar el comando `aio app get-url` AEM para obtener la URL del trabajador y usarla en el Perfil de procesamiento as a Cloud Service de la para hacer referencia a este trabajador de Asset compute personalizado. Si el proyecto contiene varios trabajadores, se muestran las direcciones URL discretas de cada trabajador.

AEM Si los entornos de desarrollo local y desarrollo as a Cloud Service de la comunidad utilizan implementaciones de Asset compute AEM independientes, las implementaciones para el desarrollo as a Cloud Service se pueden administrar de la misma manera que [Implementaciones de fase y producción](#stage-and-production).

## Espacios de trabajo de fase y producción{#stage-and-production}

La implementación en los espacios de trabajo de fase y producción se suele realizar mediante el sistema de CI/CD que elija. El proyecto de Asset compute debe implementarse en cada espacio de trabajo (fase y, a continuación, producción) de forma independiente.

La configuración de variables de entorno verdaderas anula los valores de las variables con el mismo nombre en `.env`.

![implementación de aplicaciones aio mediante variables de exportación](./assets/runtime/stage__export-and-aio.png)

El enfoque general, automatizado normalmente por un sistema de CI/CD, para la implementación en entornos de ensayo y producción es el siguiente:

1. Asegúrese de que [Módulo npm de CLI de Adobe I/O y complemento de Asset compute](../set-up/development-environment.md#aio) están instalados
1. Consulte el proyecto de Asset compute para implementar desde Git
1. Configure las variables de entorno con los valores que correspondan al espacio de trabajo de destino (fase o producción).
   + Las dos variables requeridas son `AIO_runtime_namespace` y `AIO_runtime_auth` y se obtienen por espacio de trabajo en Adobe I/O Developer Console mediante el __Descargar todo__ función.

![Consola de Adobe Developer: espacio de nombres y autenticación de tiempo de ejecución de AIO](./assets/runtime/stage-auth-namespace.png)

Los valores de estas claves se pueden configurar emitiendo comandos de exportación desde la línea de comandos:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si los trabajadores de Asset compute requieren otras variables, como el almacenamiento en la nube, también deben exportarse como variables de entorno.

1. Una vez que todas las variables de entorno estén configuradas para que el espacio de trabajo de destino las implemente, ejecute el comando deploy:
   + `aio app deploy`
1. AEM Las direcciones URL de trabajo a las que hace referencia el perfil de procesamiento as a Cloud Service de la también están disponibles a través de:
   + `aio app get-url`.

Si la versión del proyecto de Asset compute cambia, las direcciones URL de trabajo también cambian para reflejar la nueva versión, y la URL deberá actualizarse en los perfiles de procesamiento.

## Aprovisionamiento de API de Workspace{#workspace-api-provisioning}

Cuándo [configuración del proyecto del Generador de aplicaciones en Adobe I/O](../set-up/app-builder.md) para apoyar el desarrollo local, se creó un nuevo espacio de trabajo de desarrollo y __Asset compute, eventos de E/S__ y __API de administración de eventos de E/S__ se le han añadido.

El __Asset compute, eventos de E/S__ y __API de administración de eventos de E/S__ Las API solo se añaden explícitamente a los espacios de trabajo utilizados para el desarrollo local. AEM Los espacios de trabajo que se integran (exclusivamente) con entornos as a Cloud Service de la lo hacen __no__ AEM necesitan que se agreguen estas API explícitamente, ya que las API están disponibles de forma natural para los as a Cloud Service de la.
