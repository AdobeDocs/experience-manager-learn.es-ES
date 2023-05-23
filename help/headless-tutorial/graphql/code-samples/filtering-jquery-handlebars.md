---
title: Filtrado con jQuery y Handlebars
description: Implementación de JavaScript que utiliza jQuery y Handlebars que filtra WKND Adventures para mostrar. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 1%

---

# Filtrado con jQuery y Handlebars

AEM Explore la capacidad de las API de GraphQL sin encabezado de para filtrar datos mediante una aplicación de JavaScript que utiliza [jQuery](https://jquery.com/) y [Manillar](https://handlebarsjs.com/). Esta aplicación crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra el uso de Adobe [AEM Cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas de GraphQL persistentes. Esta aplicación usa el `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa al `wknd-shared/adventures-by-activity` persistió en la consulta y recupera los detalles de la aventura solo para aquellas aventuras del tipo de actividad especificado.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-activity`
