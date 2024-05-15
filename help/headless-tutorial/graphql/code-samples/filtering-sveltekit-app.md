---
title: Aplicación Simple SveltKit
description: Una aplicación simple de SveltKit que muestra aventuras WKND modeladas con fragmentos de contenido.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Filtrado de la aplicación SveltKit

AEM Explore la capacidad de las API de GraphQL sin encabezado de para enumerar datos mediante una [SveltKit](https://kit.svelte.dev/) aplicación. Esta aplicación de SveltKit crea una lista de las aventuras de WKND, que se pueden seleccionar para mostrar los detalles de la aventura.

Este código muestra el uso de Adobe [AEM Cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas de GraphQL persistentes desde SetKit. Esta aplicación usa el `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Los detalles de la aventura se solicitan a través de la `wknd-shared/adventures-by-slug` consulta persistente.

Este código:

+ AEM Se conecta a un servicio de publicación de y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`
