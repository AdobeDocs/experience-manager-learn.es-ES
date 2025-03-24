---
title: Aplicación Simple SveltKit
description: Una aplicación simple de SveltKit que muestra aventuras WKND modeladas con fragmentos de contenido.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 22
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Filtrado de la aplicación SveltKit

Explore la capacidad de las API de GraphQL de AEM sin encabezado para enumerar datos mediante una aplicación [SveltKit](https://kit.svelte.dev/). Esta aplicación de SveltKit crea una lista de las aventuras de WKND, que se pueden seleccionar para mostrar los detalles de la aventura.

Este código muestra el uso de [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) de Adobe para invocar consultas de GraphQL persistentes desde SetKit. Esta aplicación usa la consulta persistente `wknd-shared/adventures-all` para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Los detalles de la aventura se solicitan mediante la consulta persistente `wknd-shared/adventures-by-slug`.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`
