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
duration: 32
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Filtrado de aplicaciones Express

AEM Explore la capacidad de las API de GraphQL sin encabezado de para filtrar datos mediante una [Express](https://expressjs.com/) y [Pug](https://pugjs.org/) aplicación. Esta aplicación rápida crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra el uso de Adobe [AEM Cliente sin encabezado para NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) para invocar consultas de GraphQL persistentes mediante JavaScript basado en Node.js. Esta aplicación usa el `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa al `wknd-shared/adventures-by-activity` persistió en la consulta y recupera los detalles de la aventura solo para aquellas aventuras del tipo de actividad especificado. AEM Los detalles de la aventura se recuperan de a través de la pestaña de la página de `wknd-shared/adventures-by-slug` consulta persistente.

Este código:

+ AEM Se conecta a un servicio de publicación de y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, y `wknd-shared/adventures-by-slug`
