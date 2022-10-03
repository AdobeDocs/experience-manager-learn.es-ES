---
title: 'Introducción a AEM sin encabezado: GraphQL'
description: Obtenga información sobre las API de Experience Manager GraphQL y sus funcionalidades.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
source-git-commit: 332ad831b6c49e8599aa2181caf978d5626c1aba
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 2%

---

# Introducción a AEM sin encabezado: GraphQL {#getting-started-with-aem-headless}

AEM las API de GraphQL para fragmentos de contenido admiten situaciones de CMS sin encabezado en las que las aplicaciones de cliente externas procesan experiencias con contenido administrado en AEM.

Una API de envío de contenido moderna es clave para la eficacia y el rendimiento de las aplicaciones de front-end basadas en JavaScript. El uso de una API de REST presenta desafíos:

* Gran número de solicitudes para recuperar un objeto a la vez
* A menudo, el contenido &quot;sobreentrega&quot;, lo que significa que la aplicación recibe más de lo que necesita

Para superar estos desafíos, GraphQL proporciona una API basada en consultas que permite a los clientes consultar AEM solo el contenido que necesitan y recibir mediante una sola llamada de API.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

Este vídeo es una descripción general de la API de GraphQL implementada en AEM. La API de GraphQL de AEM está diseñada principalmente para ofrecer AEM fragmento de contenido a aplicaciones descendentes como parte de una implementación sin objetivos.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Introducción a AEM sin encabezado"
>abstract="Aprenda a distribuir fragmentos de contenido mediante GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618" text="Descripción general de GraphQL en AEM"

## AEM serie de vídeos de GraphQL sin encabezado

Obtenga información sobre AEM capacidades de GraphQL mediante la explicación detallada de los fragmentos de contenido y las API de GraphQL y AEM herramientas de desarrollo.

* [AEM serie de vídeos de GraphQL sin encabezado](./video-series/modeling-basics.md)

## Tutorial de mano de AEM sin encabezado GraphQL

Explore AEM capacidades de GraphQL creando una aplicación React que consume fragmentos de contenido a través de las API de GraphQL de AEM.

* [Tutorial de mano de AEM sin encabezado GraphQL](./multi-step/overview.md)

## AEM GraphQL y servicios de contenido AEM

|  | API de AEM GraphQL | Servicios de contenido AEM |
|--------------------------------|:-----------------|:---------------------|
| Definición del esquema | Modelos de fragmento de contenido estructurados | Componentes AEM |
| Contenido | Fragmentos de contenido | Componentes AEM |
| Descubrimiento de contenido | Por consulta de GraphQL | Por AEM página |
| Formato de entrega | JSON de GraphQL | AEM JSON de ComponentExporter |
