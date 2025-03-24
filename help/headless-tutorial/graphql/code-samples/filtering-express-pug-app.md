---
title: Filtrado de aplicaciones Express
description: Una aplicación Express simple que filtra aventuras WKND modeladas con fragmentos de contenido.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 29
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Filtrado de aplicaciones Express

Explore la capacidad de las API de GraphQL sin encabezado de AEM para filtrar datos mediante una aplicación [Express](https://expressjs.com/) y [Pug](https://pugjs.org/). Esta aplicación rápida crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra el uso de [AEM Headless Client for NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) de Adobe para invocar consultas de GraphQL persistentes mediante JavaScript basado en Node.js. Esta aplicación usa la consulta persistente `wknd-shared/adventures-all` para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa a la consulta persistente `wknd-shared/adventures-by-activity` y recupera los detalles de la aventura solo para aquellas aventuras del Tipo de actividad especificado. Los detalles de la aventura se recuperan de AEM mediante la consulta persistente `wknd-shared/adventures-by-slug`.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity` y `wknd-shared/adventures-by-slug`
