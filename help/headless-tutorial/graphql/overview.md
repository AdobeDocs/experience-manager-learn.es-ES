---
title: 'AEM Introducción a la tecnología sin encabezado de: GraphQL'
description: Obtenga información acerca de las API de Experience Manager GraphQL y sus funciones.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 654
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 10%

---

# Introducción a AEM Headless: GraphQL {#getting-started-with-aem-headless}

{{aem-headless-trials-promo}}

AEM Las API de GraphQL AEM para fragmentos de contenido admiten escenarios de CMS sin encabezado en los que las aplicaciones cliente externas procesan experiencias mediante contenido administrado en.

Una API moderna de entrega de contenido es clave para la eficacia y el rendimiento de las aplicaciones de front-end basadas en Javascript. El uso de una API de REST presenta desafíos:

* Gran número de solicitudes para recuperar un objeto a la vez
* A menudo, el contenido &quot;rebosa&quot;, lo que significa que la aplicación recibe más de lo que necesita

Para superar estos desafíos, GraphQL AEM proporciona una API basada en consultas que permite a los clientes consultar solo el contenido que necesita y recibir mediante una sola llamada a la API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Este vídeo es una descripción general de la API de GraphQL AEM implementada en la práctica de la. La API de GraphQL AEM AEM en la de está diseñada principalmente para proporcionar fragmentos de contenido de la aplicación de la aplicación de la aplicación de flujo descendente como parte de una implementación sin encabezado.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Introducción a AEM Headless: GraphQL"
>abstract="Aprenda a enviar fragmentos de contenido mediante GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618/?captions=spa" text="Información general sobre GraphQL en AEM"

## AEM Serie de vídeos GraphQL sin encabezado

AEM Obtenga información acerca de las funcionalidades de GraphQL AEM de la aplicación mediante la explicación detallada de los fragmentos de contenido y las API de GraphQL y las herramientas de desarrollo de la aplicación de.

* [AEM Serie de vídeos GraphQL sin encabezado](./video-series/modeling-basics.md)

## AEM Tutorial práctico de GraphQL sin encabezado

AEM Explore las funcionalidades de GraphQL AEM de la manera más sencilla posible creando una aplicación de React que consume fragmentos de contenido a través de las API de GraphQL de la aplicación de.

* [AEM Tutorial práctico de GraphQL sin encabezado](./multi-step/overview.md)

## AEM Servicios de contenido de GraphQL AEM vs.

|                                | AEM API de GraphQL | AEM Servicios de contenido |
|--------------------------------|:-----------------|:---------------------|
| Definición del esquema | Modelos de fragmento de contenido estructurado | AEM Componentes de |
| Contenido | Fragmentos de contenido | AEM Componentes de |
| Detección de contenido | Por consulta de GraphQL | AEM Por página de |
| Formato de envío | GRAPHQL JSON | AEM ComponenteExportadorJSON |
