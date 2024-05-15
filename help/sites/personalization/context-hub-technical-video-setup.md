---
title: Configuración de ContextHub para la personalización con AEM Sites
description: ContextHub es un marco de trabajo para almacenar, manipular y presentar datos de contexto. La API de JavaScript de ContextHub le permite acceder a las tiendas para crear, actualizar y eliminar datos según sea necesario. De este modo, ContextHub representa una capa de datos en las páginas. AEM En esta página se describe cómo agregar context hub a las páginas del sitio de la.
feature: Context Hub
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 357
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 2%

---

# Configuración de ContextHub para personalización {#set-up-contexthub}

ContextHub es un marco de trabajo para almacenar, manipular y presentar datos de contexto. La API de JavaScript de ContextHub le permite acceder a las tiendas para crear, actualizar y eliminar datos según sea necesario. De este modo, ContextHub representa una capa de datos en las páginas. AEM En esta página se describe cómo agregar context hub a las páginas del sitio de la.

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>AEM Utilizamos el sitio de referencia de WKND para este vídeo y no forma parte de la versión de la versión de la aplicación de la que se dispone en la actualidad. Puede descargar el [última versión aquí](https://github.com/adobe/aem-guides-wknd/releases).

Agregue ContextHub a sus páginas para habilitar las funciones de ContextHub y para vincular a las bibliotecas de JavaScript de ContextHub. La API de JavaScript de ContextHub proporciona acceso a los datos de contexto que administra ContextHub.

## Adición de ContextHub a un componente de página {#adding-contexthub-to-a-page-component}

Para habilitar las funciones de ContextHub y vincular a las bibliotecas de JavaScript de ContextHub, incluya la `contexthub` componente en la `<head>` de la página web. El código HTL del componente de página es similar al siguiente ejemplo:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## Segmentos de configuración del sitio y ContextHub {#site-configuration-and-contexthub-segments}

ContextHub incluye un motor de segmentación que administra segmentos y determina qué segmentos se resuelven para el contexto actual. Se definen varios segmentos. Puede utilizar la API de JavaScript para lo siguiente [determinar segmentos resueltos](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). Habilite los segmentos de ContextHub para su sitio en [[!UICONTROL Explorador de configuración]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=es).

## Crear segmentos {#create-segments}

AEM Cree segmentos de que actúen como reglas para los teasers. Es decir, definen cuándo aparece el contenido de un teaser en una página web. El contenido puede personalizarse específicamente para satisfacer las necesidades y los intereses del visitante, según los segmentos con los que coincidan.

## Asignación de la configuración de nube, la ruta del segmento y la ruta de ContextHub al sitio {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Asigne la ruta de configuración de nube, la ruta de segmentación y la ruta de ContextHub al nodo raíz del sitio para que pueda crear una experiencia personalizada para la audiencia. Con ContextHub, puede manipular los datos de contexto y probar los segmentos resueltos.

![CRXDE Lite](assets/crx-de-properties.png)

Puede leer más sobre ContextHub y la segmentación a continuación:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Adición de ContextHub a las tiendas de páginas y acceso a ellas](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Información acerca de la segmentación](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Configuración de la segmentación con ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
