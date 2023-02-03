---
title: Aplicación SvelteKit simple
description: Una aplicación SvelteKit sencilla que muestra las aventuras de WKND modeladas con fragmentos de contenido.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# Filtrado de la aplicación SvelteKit

Explore AEM capacidad de las API de GraphQL sin encabezado para enumerar datos mediante un [SvelteKit](https://kit.svelte.dev/) aplicación. Esta aplicación SvelteKit crea una lista de aventuras WKND, que se pueden seleccionar para mostrar los detalles de la aventura.

Este código muestra el uso de Adobe [AEM cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas de GraphQL persistentes desde SvelteKit. Esta aplicación utiliza la variable `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividad disponibles. Los detalles de la aventura se solicitan a través del `wknd-shared/adventures-by-slug` consulta persistente.

Este código:

+ Se conecta a un servicio de AEM Publish y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`
