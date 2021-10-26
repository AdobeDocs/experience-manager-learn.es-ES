---
title: Configuración de Dispatcher al moverse a AEM as a Cloud Service
description: Obtenga información sobre cambios importantes en AEM Dispatcher para AEM as a Cloud Service, la herramienta de conversión de Dispatcher y cómo utilizar el SDK de herramientas de Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 2%

---

# Dispatcher

Obtenga información sobre AEM Dispatcher para AEM as a Cloud Service, centrándose en cambios importantes de Dispatcher para AEM 6, la herramienta de conversión de Dispatcher y cómo utilizar el SDK de herramientas de Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Como parte de la refactorización de la base de código, utilice la variable [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) para refactorizar las configuraciones existentes in situ o Adobe Managed Services de Dispatcher a AEM configuración as a Cloud Service compatible de Dispatcher.

### Actividades clave

* Utilice la variable [Herramienta Dispatcher Converter de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) para migrar una configuración de Dispatcher existente.
* Haga referencia al módulo de Dispatcher desde el [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) como práctica recomendada.
* [Configuración de las herramientas locales de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) para validar Dispatcher, antes de realizar pruebas en un entorno de Cloud Service.


