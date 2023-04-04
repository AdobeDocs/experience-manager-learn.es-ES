---
title: 'Introducción a AEM sin encabezado: GraphQL'
description: Obtenga información sobre las API de GraphQL de Experience Manager y sus funcionalidades.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 2%

---

# Introducción a AEM sin encabezado: GraphQL {#getting-started-with-aem-headless}

AEM API de GraphQL para fragmentos de contenido admite escenarios CMS sin encabezado en los que las aplicaciones cliente externas procesan experiencias mediante contenido administrado en AEM.

Una API de envío de contenido moderna es clave para la eficacia y el rendimiento de las aplicaciones de front-end basadas en JavaScript. El uso de una API de REST presenta desafíos:

* Gran número de solicitudes para recuperar un objeto a la vez
* A menudo, el contenido &quot;sobreentrega&quot;, lo que significa que la aplicación recibe más de lo que necesita

Para superar estos desafíos, GraphQL proporciona una API basada en consultas que permite a los clientes consultar AEM solo el contenido que necesitan y recibir mediante una sola llamada API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Este vídeo es una descripción general de la API de GraphQL implementada en AEM. La API de GraphQL en AEM está diseñada principalmente para entregar AEM fragmentos de contenido a aplicaciones descendentes como parte de una implementación sin objetivos.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Introducción a AEM sin encabezado: GraphQL"
>abstract="Aprenda a distribuir fragmentos de contenido mediante GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618/?captions=spa" text="Información general sobre GraphQL en AEM"

## Serie de vídeo de GraphQL sin AEM

Obtenga información sobre AEM capacidades de GraphQL mediante la descripción detallada de los fragmentos de contenido y las API de GraphQL y las herramientas de desarrollo de AEM.

* [Serie de vídeo de GraphQL sin AEM](./video-series/modeling-basics.md)

## Tutorial de mano AEM sin encabezado de GraphQL

Explore AEM capacidades de GraphQL creando una aplicación React que consume fragmentos de contenido a través de AEM API de GraphQL.

* [Tutorial de mano AEM sin encabezado de GraphQL](./multi-step/overview.md)

## AEM de GraphQL y servicios de contenido AEM

|  | API de AEM GraphQL | Servicios de contenido AEM |
|--------------------------------|:-----------------|:---------------------|
| Definición del esquema | Modelos de fragmento de contenido estructurados | Componentes AEM |
| Contenido | Fragmentos de contenido | Componentes AEM |
| Descubrimiento de contenido | Por consulta de GraphQL | Por AEM página |
| Formato de entrega | GraphQL JSON | AEM JSON de ComponentExporter |
