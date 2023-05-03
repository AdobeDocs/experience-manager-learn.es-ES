---
title: Ampliación de las propiedades de página en AEM Sites
description: Obtenga información sobre cómo ampliar los campos de metadatos de Propiedades de página en Adobe Experience Manager Sites. Este vídeo detalla la manera más efectiva de lograr esto usando las características de la fusión de recursos de Sling.
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# Ampliación de las propiedades de página {#extending-page-properties-in-aem-sites}

La personalización de los campos de metadatos para las Propiedades de página es un requisito común en cualquier implementación de Sites. Este vídeo detalla la manera más efectiva de lograr esto usando las características de la fusión de recursos de Sling.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

El vídeo de arriba muestra cómo personalizar las propiedades de página para la variable [Sitio de referencia de WKND](https://github.com/adobe/aem-guides-wknd).

## Ejemplo de paquete de propiedades de página WKND

Puede utilizar el [ejemplo de paquete de propiedades de página WKND](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) contain **WKND** y **Básico** personalizaciones de ficha que se muestran en el vídeo anterior. La variable **SocialMedia** la personalización de pestañas no se proporciona como [Componente de página WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) ahora utiliza la versión V3 de los componentes principales de WCM y, en la versión V3, la variable [uso compartido en redes sociales está obsoleto](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Sin embargo, para fines de aprendizaje, puede señalar el componente Página WKND a la versión V2 de los componentes principales de WCM mediante el `sling:resourceSuperType` valor de propiedad y superponga la variable [Medios sociales](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) pestaña . Para obtener más información, consulte [Configuración de las propiedades de página](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Este paquete de muestra debe instalarse en el SDK de AEM local o en la instancia AEM 6.X.X para fines de aprendizaje.
