---
title: AEM integraciones as a Cloud Service con Adobe Experience Cloud
description: Obtenga información sobre las integraciones compatibles con AEM as a Cloud Service con otros productos de Adobe Experience Cloud.
version: Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
kt: 10718
thumbnail: KT-10718.jpeg
source-git-commit: 1c4ebdf78dd7107c7587b50e7476ea4b7ca3e812
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 11%

---


# AEM integraciones as a Cloud Service con Adobe Experience Cloud

Obtenga información sobre las integraciones compatibles con AEM as a Cloud Service con otros productos de Adobe Experience Cloud.
Haga clic en el producto Experience Cloud para obtener documentación sobre cómo configurar y utilizar las integraciones.

|  | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |  |  | š |
| [Análisis](#adobe-analytics) | š | š | š |
| Audience Manager |  |  |  |
| [Campaign Classic](#adobe-campaign-classic) | š |  |  |
| Campaign Standard |  |  |  |
| [Comercio](#adobe-commerce) | š | š |  |
| Customer Journey Analytics |  |  |  |
| [Experience Platform](#adobe-experience-platform-tags) | š |  | š |
| [Journey Optimizer](#adobe-journey-optimizer) |  | š |  |
| Administrador de aprendizaje |  |  |  |
| Marketo Engage |  |  |  |
| CDP en tiempo real |  |  |  |
| [Sensei](#adobe-sensei) | š | š | š |
| [Destino](#adobe-target) | š |  |  |
| [Workfront](#adobe-workfront) |  | š |  |


## Adobe Acrobat Sign

Adobe Acrobat Sign (anteriormente Adobe Sign) habilita los flujos de trabajo de firma electrónica para los formularios adaptables de AEM Forms mediante la mejora de los flujos de trabajo para procesar documentos para áreas legales, de ventas, de nómina, HR y otras.

### AEM Forms

+ [Configuración de la integración con Adobe Acrobat Sign](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html)
+ [Tutorial de AEM Forms y Adobe Acrobat Sign](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html)

## Adobe Analytics

La integración de Adobe Analytics con AEM as a Cloud Service le permite realizar un seguimiento de la actividad del contenido y analizar los datos desde cualquier lugar del recorrido del cliente. Además, obtenga informes versátiles, inteligencia predictiva y mucho más.

### AEM Sites

+ [Configuración de la integración con Adobe Analytics](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html)
+ [Tutorial de AEM Sites y Analytics](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html?lang=es)
+ Capa de datos del cliente de Adobe (ACDL)

   + [Ampliación de la ACDL en AEM componentes principales de WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)
   + [Integrar ACDL con AEM componentes principales de WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html)
   + [Gestión de datos basada en eventos con ACDL](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html)
   + [Tutorial sobre la capa de datos del cliente de Adobe (ACDL)](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es)

### AEM Assets

+ [Información general sobre Assets Insights](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html)
+ [Configuración de Assets Insights](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html#configure-asset-insights)
+ [Tutorial de Assets Insights](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html)

### AEM Forms

+ [Configuración de la integración con Adobe Analytics](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html)

## Adobe Campaign Classic

La integración de Adobe Campaign Classic con AEM as a Cloud Service le permite administrar el contenido y los formularios del envío de correo electrónico directamente en Adobe Experience Manager, mientras que utiliza Adobe Campaign Classic para personalizar y enviar correos electrónicos.

### AEM Sites

+ [Configuración de la integración con Adobe Campaign Classic](https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/campaignonpremise.html)
   + __Vínculos de documentación a AEM 6.5; sin embargo, los pasos son los mismos en AEM as a Cloud Service__
+ [Integración de AEM Sites con Adobe Campaign Classic](https://github.com/adobe/aem-core-email-components/wiki/Integrating-AEM-with-ACC)
+ [Envío de correos electrónicos desde AEM Sites con Adobe Campaign Classic](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/aem-adobe-campaign/campaign.html)
+ [Documentación de los componentes principales de AEM correo electrónico](https://github.com/adobe/aem-core-email-components#aem-email-core-components)


## Adobe Commerce

La integración de Adobe Commerce con AEM as a Cloud Service permite a las marcas escalar e innovar más rápidamente para diferenciar las experiencias comerciales y capturar un gasto en línea acelerado. AEM con Comercio combina las experiencias inmersivas, omnicanal y personalizadas en Experience Manager con cualquier cantidad de soluciones de comercio para ofrecer experiencias diferenciadas en todas las partes del recorrido de compras, reducir el tiempo de respuesta al valor e impulsar una mayor conversión.

### AEM Sites

+ [Guía del usuario de contenido y comercio de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html)


## Etiquetas de Adobe Experience Platform

Las etiquetas de Adobe Experience Platform (anteriormente Adobe Launch, DTM) se integran perfectamente con AEM, lo que proporciona una forma sencilla de implementar y administrar [analytics](#adobe-analytics), [segmentación](#adobe-target), las etiquetas de marketing y publicidad necesarias para atraer experiencias de los clientes.

### AEM Sites

+ [Guía del usuario de etiquetas del Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Tutorial sobre etiquetas de Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

## AEM Forms

+ [Guía del usuario de etiquetas del Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Tutorial sobre etiquetas de Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)


## Adobe Workfront

Las integraciones de Adobe Workfront con AEM como Cloud Service optimizan el proceso de creación de recursos digitales, colaboración y administración del ciclo vital.

## AEM Assets

+ [Configuración del conector mejorado de Workfront](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html?lang=es)
+ [Vídeos de conector mejorados de Workfront](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html)
+ AEM Assets Essentials

   + [Guía del usuario de Adobe Workfront para Assets Essentials](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Vídeos de Adobe Workfront y Assets Essentials](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)

## Adobe Sensei

Adobe Sensei proporciona tecnología de aprendizaje automático y de IA para transformar el proceso de administración de contenido mediante etiquetas inteligentes, recorte inteligente, búsqueda visual y mucho más.

### AEM Sites

+ [Resumir texto en fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html#summarizing-text)

### AEM Assets

+ [Etiquetas inteligentes para imágenes](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html)
+ [Etiquetas inteligentes personalizadas para imágenes](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html)
+ [Etiquetas inteligentes para vídeos](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html)
+ [Recorte inteligente](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html)
+ [Búsqueda visual](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html)

### AEM Forms

+ [Servicio de automated forms conversion](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html)


## Adobe Target

Adobe Target se integra con AEM as a Cloud Service para ofrecer una experiencia web optimizada para todos los usuarios finales, todo ello basado en el contenido de AEM.

### AEM Sites

+ [Configuración de la integración con Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
+ Fragmentos de experiencias a Target

   + [Publicar fragmentos de experiencias en Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
   + [Publicar fragmentos de experiencias como JSON en Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)

+ [Uso de AEM ContextHub con Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html#creating-an-adobe-target-audience-using-the-audience-console)
+ [Tutorial de AEM Sites y Target](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html)
