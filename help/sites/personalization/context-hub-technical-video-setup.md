---
title: Configuración de ContextHub para personalización con AEM Sites
description: ContextHub es un marco para almacenar, manipular y presentar datos de contexto. La API de JavaScript de ContextHub le permite acceder a las tiendas para crear, actualizar y eliminar datos según sea necesario. Como tal, ContextHub representa una capa de datos en las páginas. En esta página se describe cómo agregar Context Hub a las páginas del sitio de AEM.
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 6%

---


# Configuración de ContextHub para personalización {#set-up-contexthub}

ContextHub es un marco para almacenar, manipular y presentar datos de contexto. La API de JavaScript de ContextHub le permite acceder a las tiendas para crear, actualizar y eliminar datos según sea necesario. Como tal, ContextHub representa una capa de datos en las páginas. En esta página se describe cómo agregar Context Hub a las páginas del sitio de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>Utilizamos el sitio de referencia WKND para este vídeo y no forma parte de la versión de AEM. Puede descargar la [última versión aquí](https://github.com/adobe/aem-guides-wknd/releases).

Agregue ContextHub a sus páginas para habilitar las funciones de ContextHub y vincular a las bibliotecas JavaScript de ContextHub. La API JavaScript de ContextHub proporciona acceso a los datos de contexto que administra ContextHub.

## Agregar ContextHub a un componente de página {#adding-contexthub-to-a-page-component}

Para habilitar las funciones de ContextHub y vincular las bibliotecas JavaScript de ContextHub, incluya el componente `contexthub` en la sección `<head>` de la página web. El código HTL para el componente de página se parece al siguiente ejemplo:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## Configuración del sitio y segmentos de ContextHub {#site-configuration-and-contexthub-segments}

ContextHub incluye un motor de segmentación que administra segmentos y determina qué segmentos se resuelven para el contexto actual. Se definen varios segmentos. Puede utilizar la API de Javascript para [determinar los segmentos resueltos](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). Habilite los segmentos de ContextHub para su sitio en [[!UICONTROL Explorador de configuración]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html).

## Crear segmentos {#create-segments}

Cree segmentos de AEM que actúen como reglas para los teasers. Es decir, definen cuándo aparece contenido dentro de un teaser en una página web. El contenido puede personalizarse específicamente para satisfacer las necesidades y los intereses del visitante, según los segmentos con los que coincidan.

## Asignación de la configuración de Cloud, la ruta de segmento y la ruta de ContextHub a su sitio {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Asignando la ruta de configuración de Cloud, la ruta de segmentación y la ruta de ContextHub al nodo raíz del sitio, puede crear una experiencia personalizada para la audiencia. Con ContextHub, puede manipular los datos de contexto y probar los segmentos resueltos.

![CRXDE Lite](assets/crx-de-properties.png)

Puede obtener más información sobre ContextHub y la segmentación a continuación:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Adición de ContextHub a la página y acceso a las tiendas](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Información acerca de la segmentación](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Configuración de la segmentación con ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
