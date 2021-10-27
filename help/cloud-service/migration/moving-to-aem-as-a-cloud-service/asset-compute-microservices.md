---
title: AEM Assets Microservices y pasar a AEM as a Cloud Service
description: Descubra cómo los microservicios de asset compute de AEM Assets as a Cloud Service le permiten generar de forma automática y eficaz cualquier representación para sus recursos, sustituyendo esta función del flujo de trabajo AEM tradicional.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 1%

---

# Microservicios de AEM Assets: paso a AEM as a Cloud Service

Descubra cómo los microservicios de asset compute de AEM Assets as a Cloud Service le permiten generar de forma automática y eficaz cualquier representación para sus recursos, sustituyendo esta función del flujo de trabajo AEM tradicional.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Herramienta de migración del flujo de trabajo

![Herramienta de Migración del flujo de trabajo de recursos](./assets/asset-workflow-migration.png)

Como parte de la refactorización de la base de código, utilice la variable [Herramienta Migración del flujo de trabajo de recursos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) para migrar flujos de trabajo existentes para utilizar los microservicios de Asset compute en AEM as a Cloud Service.

### Actividades clave

* Utilice la variable [Migración del flujo de trabajo de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) herramienta para migrar flujos de trabajo de procesamiento de recursos para utilizar los microservicios de Asset compute.
* Configure un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) e implemente los flujos de trabajo actualizados. Puede ser necesario realizar un ajuste manual para flujos de trabajo complejos.
* Continúe iterando en un entorno de desarrollo local mediante el SDK de AEM hasta que el flujo de trabajo actualizado coincida con la paridad de características.
* Implemente la base de código actualizada en un entorno de desarrollo as a Cloud Service AEM y continúe validando.

