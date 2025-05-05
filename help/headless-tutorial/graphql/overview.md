---
title: 'Introducción a AEM sin encabezado: GraphQL'
description: Obtenga información acerca de las API de Experience Manager GraphQL y sus funcionalidades.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 10%

---

# Introducción a AEM Headless: GraphQL {#getting-started-with-aem-headless}

API de GraphQL de AEM para fragmentos de contenido
admite escenarios de CMS sin encabezado en los que aplicaciones cliente externas procesan experiencias con contenido administrado en AEM.

Una API moderna de entrega de contenido es clave para la eficacia y el rendimiento de las aplicaciones de front-end basadas en Javascript. El uso de una API de REST presenta desafíos:

* Gran número de solicitudes para recuperar un objeto a la vez
* A menudo, el contenido &quot;rebosa&quot;, lo que significa que la aplicación recibe más de lo que necesita

Para superar estos desafíos, GraphQL proporciona una API basada en consultas que permite a los clientes consultar AEM solo el contenido que necesita y recibir mediante una sola llamada de API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Este vídeo es una descripción general de la API de GraphQL implementada en AEM. La API de GraphQL en AEM está diseñada principalmente para ofrecer fragmentos de contenido de AEM a aplicaciones de flujo descendente como parte de una implementación sin encabezado.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Introducción a AEM Headless: GraphQL"
>abstract="Aprenda a enviar fragmentos de contenido mediante GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618/?captions=spa" text="Información general sobre GraphQL en AEM"

## Serie de vídeos GraphQL AEM sin encabezado

Obtenga información acerca de las funcionalidades de GraphQL de AEM a través de la explicación detallada de los fragmentos de contenido y las API de GraphQL y las herramientas de desarrollo de AEM.

* [Serie de vídeos GraphQL AEM sin encabezado](./video-series/modeling-basics.md)

## Tutorial práctico de AEM Headless GraphQL

Explore las funcionalidades de GraphQL de AEM creando una aplicación de React que consume fragmentos de contenido a través de las API de GraphQL de AEM.

* [Tutorial práctico de AEM Headless GraphQL](./multi-step/overview.md)

## AEM GraphQL frente a AEM Content Services

|                                | API de AEM GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definición del esquema | Modelos de fragmento de contenido estructurado | Componentes de AEM |
| Contenido | Fragmentos de contenido | Componentes de AEM |
| Detección de contenido | Por consulta de GraphQL | Por página de AEM |
| Formato de envío | GRAPHQL JSON | JSON del exportador de componentes de AEM |
