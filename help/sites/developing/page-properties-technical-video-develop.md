---
title: Ampliación de las propiedades de página en AEM Sites
description: Obtenga información sobre cómo ampliar los campos de metadatos de las propiedades de página en Adobe Experience Manager Sites. Este vídeo detalla la forma más eficaz de lograrlo mediante las funciones de la fusión de recursos de Sling.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Experience Manager as a Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 1%

---

# Ampliación de las propiedades de página {#extending-page-properties-in-aem-sites}

La personalización de los campos de metadatos para las Propiedades de página es un requisito común en cualquier implementación de Sites. Este vídeo detalla la forma más eficaz de lograrlo mediante las funciones de la fusión de recursos de Sling.

>[!VIDEO](https://video.tv.adobe.com/v/3410341?quality=12&learn=on&captions=spa)

El vídeo anterior muestra cómo personalizar las propiedades de página para el [Sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd).

## Paquete de propiedades de página WKND de muestra

Puede usar el [paquete de propiedades de página WKND de muestra](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) que contiene **WKND** y las personalizaciones de pestaña **Básico** que se muestran en el vídeo anterior. No se proporcionó la personalización de la pestaña **SocialMedia** porque [el componente de página WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) ahora usa la versión V3 de los componentes principales de WCM y en la versión V3 [el uso compartido en medios sociales está obsoleto](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Sin embargo, con fines de aprendizaje, puede señalar el componente Página WKND a la versión V2 de los componentes principales de WCM mediante el valor de propiedad `sling:resourceSuperType` y superponer la pestaña [Medios sociales](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95). Para obtener más información, consulte [Configuración de las propiedades de la página](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=es#configuring-your-page-properties)

Este paquete de muestra debe instalarse en la instancia local de AEM SDK o AEM 6.X.X para fines de aprendizaje.
