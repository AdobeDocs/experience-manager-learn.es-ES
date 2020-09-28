---
title: Configurar ContextHub para personalización con AEM Sites
description: ContextHub es un marco para almacenar, manipular y presentar datos de contexto. La API de JavaScript de ContextHub le permite acceder a las tiendas para crear, actualizar y eliminar datos según sea necesario. Como tal, ContextHub representa una capa de datos en las páginas. Esta página describe cómo agregar el concentrador de contexto a las páginas del sitio AEM.
feature: context-hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 5%

---


# Configurar ContextHub para personalización {#set-up-contexthub}

ContextHub es un marco para almacenar, manipular y presentar datos de contexto. La API de JavaScript de ContextHub le permite acceder a las tiendas para crear, actualizar y eliminar datos según sea necesario. Como tal, ContextHub representa una capa de datos en las páginas. Esta página describe cómo agregar el concentrador de contexto a las páginas del sitio AEM.

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>Utilizamos el sitio de referencia WKND para este vídeo y no forma parte de AEM versión. Puede descargar la [última versión aquí](https://github.com/adobe/aem-guides-wknd/releases).

Añada ContextHub en sus páginas para habilitar las funciones de ContextHub y vincular a las bibliotecas JavaScript de ContextHub. La API JavaScript de ContextHub proporciona acceso a los datos de contexto que administra ContextHub.

## Añadir ContextHub en un componente de página {#adding-contexthub-to-a-page-component}

Para habilitar las funciones de ContextHub y vincular a las bibliotecas JavaScript de ContextHub, incluya el `contexthub` componente en la `<head>` sección de la página web. El código HTML del componente de página se parece al siguiente ejemplo:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## Configuración del sitio y segmentos de ContextHub {#site-configuration-and-contexthub-segments}

ContextHub incluye un motor de segmentación que administra segmentos y determina qué segmentos se resuelven para el contexto actual. Se definen varios segmentos. Puede utilizar la API de JavaScript para [determinar los segmentos](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)resueltos. Habilite los segmentos de ContextHub para su sitio en [!UICONTROL Navegador]de configuración.

## Crear segmentos {#create-segments}

Cree segmentos de AEM que actúen como reglas para los teasers. Es decir, definen el momento en que el contenido de un teaser aparece en una página web. El contenido puede personalizarse específicamente para satisfacer las necesidades y los intereses del visitante, según los segmentos con los que coincidan.

## Asignación de la configuración de la nube, la ruta de segmentos y la ruta de ContextHub a su sitio {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Asignación de la ruta de configuración de la nube, la ruta de segmentación y la ruta de ContextHub al nodo raíz del sitio para que pueda crear una experiencia personalizada para su audiencia. Con ContextHub, puede manipular los datos de contexto y probar los segmentos resueltos.

![CRXDE Lite](assets/crx-de-properties.png)

Puede leer más sobre ContextHub y la segmentación a continuación:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Añadir Context Hub a la página y acceder a las tiendas](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Información acerca de la segmentación](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Configuración de la segmentación con ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
