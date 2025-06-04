---
title: 'Introducción a AEM sin encabezado: GraphQL'
description: Obtenga información sobre las API GraphQL de Experience Manager y sus funcionalidades.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '266'
ht-degree: 100%

---

# Introducción a AEM sin encabezado: GraphQL {#getting-started-with-aem-headless}

Las API GraphQL de AEM para fragmentos de contenido
admiten escenarios de CMS sin encabezado en los que las aplicaciones de cliente externas procesan experiencias con contenido administrado en AEM.

Una API moderna de suministro de contenido es clave para la eficacia y el rendimiento de las aplicaciones de front-end basadas en Javascript. El uso de una API de REST presenta desafíos:

* Un gran número de solicitudes para recuperar un objeto a la vez
* A menudo, el contenido “entrega de más”, lo que significa que la aplicación recibe más de lo que necesita

Para superar estos desafíos, GraphQL proporciona una API basada en consultas que permite a los clientes consultar en AEM únicamente el contenido que necesita y recibir mediante una sola llamada de la API.

>[!VIDEO](https://video.tv.adobe.com/v/3452882?quality=12&learn=on&captions=spa)

Este vídeo ofrece información general sobre la API GraphQL implementada en AEM. La API GraphQL de AEM está diseñada principalmente para ofrecer fragmentos de contenido de AEM a aplicaciones posteriores como parte de una implementación sin encabezado.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Introducción a AEM sin encabezado: GraphQL"
>abstract="Aprenda a enviar fragmentos de contenido mediante GraphQL."
>additional-url="https://video.tv.adobe.com/v/3452882?captions=spa" text="Información general sobre GraphQL en AEM"

## Serie de vídeos sobre GraphQL de AEM sin encabezado

Obtenga información sobre las funcionalidades de GraphQL de AEM en la guía de fragmentos de contenido y de las API de GraphQL y las herramientas de desarrollo de AEM.

* [Serie de vídeos sobre GraphQL de AEM sin encabezado](./video-series/modeling-basics.md)

## Tutorial práctico sobre GraphQL de AEM sin encabezado

Explore las funcionalidades de GraphQL de AEM creando una aplicación React que consume fragmentos de contenido a través de las API de GraphQL de AEM.

* [Tutorial práctico sobre GraphQL de AEM sin encabezado](./multi-step/overview.md)

## GraphQL de AEM y servicios de contenido de AEM

|                                | API de GraphQL de AEM | Servicios de contenido de AEM |
|--------------------------------|:-----------------|:---------------------|
| Definición de esquema | Modelos de fragmento de contenido estructurado | Componentes de AEM |
| Contenido | Fragmentos de contenido | Componentes de AEM |
| Detección de contenido | Por consulta de GraphQL | Por página de AEM |
| Formato de distribución | GraphQL JSON | JSON del exportador de componentes de AEM |
