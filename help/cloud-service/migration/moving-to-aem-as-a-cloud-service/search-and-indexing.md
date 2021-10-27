---
title: Búsqueda e indexación en AEM as a Cloud Service
description: Obtenga información sobre los índices de búsqueda de AEM as a Cloud Service, cómo convertir AEM 6 definiciones de índice y cómo implementar índices.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Búsqueda e indexación

Obtenga información sobre los índices de búsqueda de AEM as a Cloud Service, cómo convertir AEM 6 definiciones de índice para que sean compatibles con AEM as a Cloud Service y cómo implementar índices para AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Herramienta Conversor de índices

![Herramienta Conversor de índices](./assets/index-converter.png)

Como parte de la refactorización de la base de código, utilice la variable [Herramienta Conversor de índices](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para convertir definiciones de índice Oak personalizadas a AEM definiciones de índice compatibles as a Cloud Service.

### Actividades clave

* Utilice la variable [Migración del flujo de trabajo de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) herramienta para migrar flujos de trabajo de procesamiento de recursos para utilizar los microservicios de Asset compute.
* Configure un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) e implemente los índices personalizados. Asegúrese de que los índices actualizados estén actualizados.
* Implemente la base de código actualizada en un entorno de desarrollo as a Cloud Service AEM y continúe validando.
* Si se modifica un índice predeterminado **SIEMPRE** copie la definición de índice más reciente de un entorno as a Cloud Service AEM que se ejecute en la última versión. Modifique la definición del índice copiado para adaptarla a sus necesidades.