---
title: Ampliación de las propiedades de página en AEM Sites
description: Obtenga información sobre cómo ampliar los campos de metadatos de las propiedades de página en Adobe Experience Manager Sites. Este vídeo detalla la forma más eficaz de lograrlo mediante las funciones de la fusión de recursos de Sling.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# Ampliación de propiedades de página {#extending-page-properties-in-aem-sites}

La personalización de los campos de metadatos para las Propiedades de página es un requisito común en cualquier implementación de Sites. Este vídeo detalla la forma más eficaz de lograrlo mediante las funciones de la fusión de recursos de Sling.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

El vídeo anterior muestra cómo personalizar las propiedades de página para la variable [Sitio de referencia de WKND](https://github.com/adobe/aem-guides-wknd).

## Paquete de propiedades de página WKND de muestra

Puede utilizar el proporcionado [paquete de propiedades de página WKND de muestra](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) conteniendo **WKND** y **Básico** las personalizaciones de pestañas se muestran en el vídeo anterior. El **SocialMedia** la personalización de pestañas no se proporciona como [Componente de página WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) ahora utiliza la versión 3 de los componentes principales de WCM y en la versión 3 la [el uso compartido en redes sociales está obsoleto](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Sin embargo, con fines de aprendizaje, puede señalar el componente Página WKND a la versión V2 de los componentes principales de WCM mediante la variable `sling:resourceSuperType` valor de propiedad y superposición de [Medios sociales](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) pestaña. Para obtener más información, consulte [Configuración de las propiedades de página](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

AEM AEM Este paquete de muestra debe instalarse en la instancia local del SDK de la o en la instancia de 6.X.X con fines de aprendizaje.
