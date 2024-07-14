---
title: Filtrado de aplicaciones Express
description: Una aplicación Express simple que filtra aventuras WKND modeladas con fragmentos de contenido.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Filtrado de aplicaciones Express

AEM Explore la capacidad de las API de GraphQL sin encabezado para filtrar datos mediante la aplicación [Express](https://expressjs.com/) y [Pug](https://pugjs.org/). Esta aplicación rápida crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra cómo usar el cliente sin encabezado [de Adobe AEM para NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) para invocar consultas GraphQL persistentes mediante JavaScript basado en Node.js. Esta aplicación usa la consulta persistente `wknd-shared/adventures-all` para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa a la consulta persistente `wknd-shared/adventures-by-activity` y recupera los detalles de la aventura solo para aquellas aventuras del Tipo de actividad especificado. AEM Los detalles de la aventura se recuperan de la vista a través de la consulta persistente `wknd-shared/adventures-by-slug`.

Este código:

+ AEM Se conecta a un servicio de Publish de y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity` y `wknd-shared/adventures-by-slug`
