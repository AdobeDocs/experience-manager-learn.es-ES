---
title: Filtrado de aplicaciones Angular
description: Una aplicación de Angular sencilla que filtra las aventuras de WKND modeladas con fragmentos de contenido.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtrado de aplicaciones Angular

Explore la capacidad de las API de GraphQL de AEM sin encabezado para filtrar datos mediante la aplicación [Angular](https://angular.io/). Esta aplicación de Angular crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra el uso de [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) de Adobe para invocar consultas de GraphQL persistentes desde Angular. Esta aplicación usa la consulta persistente `wknd-shared/adventures-all` para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa a la consulta persistente `wknd-shared/adventures-by-activity` y recupera los detalles de la aventura solo para aquellas aventuras del Tipo de actividad especificado.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-activity`
