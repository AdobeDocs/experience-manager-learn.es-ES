---
title: Integración del SDK web del Experience Platform
description: Obtenga información sobre cómo integrar AEM as a Cloud Service con el SDK web de Experience Platform. Este paso fundamental es esencial para integrar productos de Adobe Experience Cloud, como Adobe Analytics, Target o productos innovadores recientes como Real-time Customer Data Platform, Customer Journey Analytics y Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
source-git-commit: 593ef5767a5f2321c689e391f9c9019de7c94672
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 1%

---


# Integración del SDK web del Experience Platform

Aprenda a integrar AEM as a Cloud Service con el Experience Platform [SDK web](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Este paso fundamental es esencial para integrar productos de Adobe Experience Cloud, como Adobe Analytics, Target o productos innovadores recientes como Real-time Customer Data Platform, Customer Journey Analytics y Journey Optimizer.

También aprenderá a recopilar y enviar [WKND: proyecto de Adobe Experience Manager de muestra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) vista de página de los datos de [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Después de completar esta configuración, puede implementar Experience Platform y aplicaciones relacionadas como [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=es), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html) y [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). Para lograr una mejor participación de los clientes mediante la estandarización de los datos web y de los clientes.

## Requisitos previos

A la hora de integrar el SDK web de Experience Platform, es necesario lo siguiente.

En **AEM como Cloud Service**:

+ AEM acceso del administrador a AEM entorno as a Cloud Service
+ Acceso del administrador de implementación a Cloud Manager
+ Clonar e implementar el [WKND: proyecto de Adobe Experience Manager de muestra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) a su entorno as a Cloud Service AEM.

En **Experience Platform**:

+ Acceso a la producción predeterminada, **Prod** simulador de pruebas.
+ Acceso a **Esquemas** en Administración de datos
+ Acceso a **Conjuntos de datos** en Administración de datos
+ Acceso a **Datastreams** en Recopilación de datos
+ Acceso a **Etiquetas** (anteriormente, Launch) en Recopilación de datos

En caso de que no tenga los permisos necesarios, el administrador del sistema utilizará [Adobe Admin Console](https://adminconsole.adobe.com/) puede conceder los permisos necesarios.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Crear esquema XDM: Experience Platform

El esquema del Modelo de datos de experiencia (XDM) le ayuda a estandarizar los datos de experiencia del cliente. Para recopilar el **WKND pageview** datos, crear un esquema XDM y utilizar los grupos de campos proporcionados por el Adobe `AEP Web SDK ExperienceEvent` para la recopilación de datos web.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Obtenga información sobre el esquema XDM y conceptos relacionados, como grupos de campos, tipos, clases y tipos de datos de [Información general del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

La variable [Información general del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) es un recurso bueno para obtener información sobre el esquema XDM y conceptos relacionados como grupos de campos, tipos, clases y tipos de datos. Proporciona una comprensión completa del modelo de datos XDM y cómo crear y administrar esquemas XDM para estandarizar los datos en toda la empresa. Explíquela para comprender mejor el esquema XDM y cómo puede beneficiar sus procesos de recopilación y administración de datos.

## Crear flujo de datos: Experience Platform

Un flujo de datos indica a la red perimetral de plataforma dónde enviar los datos recopilados. Por ejemplo, se puede enviar a Experience Platform, Analytics o Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Familiarícese con el concepto de Datastreams y temas relacionados como la administración y configuración de datos visitando el [Información general sobre Datastreams](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) página.

## Crear propiedad de etiqueta: Experience Platform

Obtenga información sobre cómo crear una propiedad tag (anteriormente conocida como Launch) en Experience Platform para añadir la biblioteca JavaScript del SDK web al sitio web de WKND. La propiedad de etiqueta recién definida tiene los siguientes recursos:

+ Extensiones de etiquetas: [Principal](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) y [SDK web de Adobe Experience Platform](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Elementos de datos: Los elementos de datos del tipo de código personalizado que extraen nombre de página, sección del sitio y nombre de host utilizando la capa de datos del cliente de Adobe del sitio WKND. Además, el elemento de datos de tipo objeto XDM que cumple con la nueva compilación del esquema WKND XDM creada anteriormente [Crear esquema XDM](#create-xdm-schema---experience-platform) paso a paso.
+ Regla: Enviar datos a Platform Edge Network cada vez que se visite una página web WKND mediante la capa de datos del cliente de Adobe activada `cmp:show` evento.


>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)

La variable [Información general sobre etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) proporciona información detallada sobre conceptos importantes como elementos de datos, reglas y extensiones.

Para obtener información adicional sobre la integración de AEM componentes principales con la capa de datos del cliente de Adobe, consulte la [Uso de la capa de datos del cliente de Adobe con AEM guía de componentes principales](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es).

## Conectar la propiedad Tag a AEM

Descubra cómo vincular la propiedad de etiqueta creada recientemente a AEM mediante IMS de Adobe y la configuración de Launch de Adobe en AEM. Cuando se crea un entorno as a Cloud Service AEM, se generan automáticamente varias configuraciones de cuenta técnica de Adobe IMS, incluido Adobe Launch. Sin embargo, para AEM versión 6.5, debe configurar una manualmente.

Después de vincular la propiedad tag , el sitio WKND puede cargar la biblioteca JavaScript de la propiedad tag en las páginas web mediante la configuración del servicio en la nube de Launch de Adobe.

### Comprobar carga de la propiedad Tag en WKND

Uso de Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) o [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) , compruebe si la propiedad tag se está cargando en las páginas WKND. Puede verificar,

+ Detalles de la propiedad de etiquetas, como extensión, versión, nombre y más.
+ Versión de la biblioteca del SDK web de plataforma, ID de almacén de datos
+ Objeto XDM como parte `events` en el SDK web de Experience Platform

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Crear conjunto de datos: Experience Platform

Los datos de vista de página recopilados mediante el SDK web se almacenan en el lago de datos del Experience Platform como conjuntos de datos. El conjunto de datos es una construcción de almacenamiento y administración para una recopilación de datos como una tabla de base de datos que sigue un esquema. Obtenga información sobre cómo crear un conjunto de datos y configurar el conjunto de datos creado anteriormente para enviar datos al Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

La variable [Información general sobre conjuntos de datos](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) proporciona más información sobre conceptos, configuraciones y otras funcionalidades de ingesta.


## Datos de vista de página WKND en el Experience Platform

Después de configurar el SDK web con AEM, especialmente en el sitio WKND, es hora de generar tráfico navegando por las páginas del sitio. A continuación, confirme que los datos de vista de página se están incorporando en el conjunto de datos de Experience Platform. En la interfaz de usuario del conjunto de datos, se muestran varios detalles, como registros totales, tamaño y lotes ingestados, junto con un gráfico de barras que resulta atractivo visualmente.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Resumen

¡Buen trabajo! Ha completado la configuración de AEM con el SDK web de Adobe Experience Platform (Experience Platform) para recopilar e introducir datos de un sitio web. Con esta base, ahora puede explorar más posibilidades para mejorar e integrar productos como Analytics, Target, Customer Journey Analytics (CJA) y muchos otros para crear experiencias personalizadas y enriquecidas para sus clientes. Siga aprendiendo y explorando para liberar todo el potencial de Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)
