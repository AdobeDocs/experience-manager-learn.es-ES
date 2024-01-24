---
title: Filtrado de aplicación Preact
description: Una aplicación de Preact simple que filtra aventuras WKND modeladas con fragmentos de contenido.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
exl-id: d2b7e8ab-8bbc-495f-94f1-362ea47b3853
duration: 28
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtrado de aplicación Preact

AEM Explore la capacidad de las API de GraphQL sin encabezado de para filtrar datos mediante una [Preact](https://preactjs.com/) aplicación. Esta aplicación de Preact crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra el uso de Adobe [AEM Cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas de GraphQL persistentes desde React. Esta aplicación usa el `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa al `wknd-shared/adventures-by-activity` persistió en la consulta y recupera los detalles de la aventura solo para aquellas aventuras del tipo de actividad especificado.

Este código:

+ AEM Se conecta a un servicio de publicación de y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-activity`
