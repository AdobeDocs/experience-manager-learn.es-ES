---
title: Filtrado de aplicación de valor
description: Una aplicación de valor simple que filtra las aventuras de WKND modeladas con fragmentos de contenido.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Filtrado de aplicación de valor

Explorar AEM API de GraphQL sin encabezado [Valor](https://vuejs.org/) aplicación. Esta aplicación React crea una lista de aventuras WKND filtrables por Tipo de actividad.

Este código muestra el uso de Adobe [AEM cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas de GraphQL persistentes desde Vue. Esta aplicación utiliza la variable `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividad disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa al `wknd-shared/adventures-by-activity` consulta persistente y recupera los detalles de aventura solo para aquellas aventuras del tipo de actividad especificado.

Este código:

+ Se conecta a un servicio de AEM Publish y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-activity`
