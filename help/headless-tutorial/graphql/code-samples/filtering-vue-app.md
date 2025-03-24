---
title: Filtrado de la aplicación Vue
description: Una aplicación de Vue simple que filtra aventuras WKND modeladas con fragmentos de contenido.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 8f96093a-4449-4249-9257-028e2ffd979b
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtrado de la aplicación Vue

Explore la capacidad de las API de GraphQL de AEM sin encabezado para filtrar datos mediante la aplicación [Vue](https://vuejs.org/). Esta aplicación de React crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra el uso de [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) de Adobe para invocar consultas de GraphQL persistentes desde Vue. Esta aplicación usa la consulta persistente `wknd-shared/adventures-all` para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa a la consulta persistente `wknd-shared/adventures-by-activity` y recupera los detalles de la aventura solo para aquellas aventuras del Tipo de actividad especificado.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-activity`
