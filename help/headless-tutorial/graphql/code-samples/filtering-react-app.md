---
title: Filtrado de la aplicación React
description: Una aplicación React simple que filtra aventuras WKND modeladas con fragmentos de contenido.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 1eb9487e-a82a-4d15-a776-cf004f2e3f01
duration: 42
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtrado de la aplicación React

AEM Explore la capacidad de las API de GraphQL sin encabezado de para filtrar datos mediante una [Reaccionar](https://reactjs.org/) aplicación. Esta aplicación de React crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra el uso de Adobe [AEM Cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas de GraphQL persistentes desde React. Esta aplicación usa el `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa al `wknd-shared/adventures-by-activity` persistió en la consulta y recupera los detalles de la aventura solo para aquellas aventuras del tipo de actividad especificado.

Este código:

+ AEM Se conecta a un servicio de publicación de y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-activity`
