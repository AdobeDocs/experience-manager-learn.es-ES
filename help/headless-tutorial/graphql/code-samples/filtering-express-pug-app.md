---
title: Filtrado de la aplicación Express
description: Una aplicación Express sencilla que filtra las aventuras de WKND modeladas con fragmentos de contenido.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
source-git-commit: ac2b3a766caea1013165aedd3478bf859212cc89
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# Filtrado de la aplicación Express

Explorar AEM API de GraphQL sin encabezado capacidad para filtrar datos mediante un [Express](https://expressjs.com/) y [Pug](https://pugjs.org/) aplicación. Esta aplicación Express crea una lista de aventuras WKND filtrables por tipo de actividad.

Este código muestra el uso de Adobe [AEM cliente sin encabezado para NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) para invocar consultas de GraphQL persistentes mediante JavaScript basado en Node.js. Esta aplicación utiliza la variable `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividad disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa al `wknd-shared/adventures-by-activity` consulta persistente y recupera los detalles de aventura solo para aquellas aventuras del tipo de actividad especificado. Los detalles de aventura se recuperan de AEM a través de la variable `wknd-shared/adventures-by-slug` consulta persistente.

Este código:

+ Se conecta a un servicio de AEM Publish y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`y `wknd-shared/adventures-by-slug`
