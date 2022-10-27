---
title: Filtrado con jQuery y controladores
description: Implementación de JavaScript mediante jQuery y Handlebars que filtra las aventuras de WKND para mostrar. .
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
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---


# Filtrado con jQuery y controladores

Explorar AEM API de GraphQL sin encabezado y poder filtrar datos mediante una aplicación de JavaScript que utilice [jQuery](https://jquery.com/) y [Manillares](https://handlebarsjs.com/). Esta aplicación crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra el uso de Adobe [AEM cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas de GraphQL persistentes. Esta aplicación utiliza la variable `wknd-shared/adventures-all` consulta persistente para recopilar todas las aventuras y derivar una lista de tipos de actividad disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa al `wknd-shared/adventures-by-activity` consulta persistente y recupera los detalles de aventura solo para aquellas aventuras del tipo de actividad especificado.

Este código:

+ Se conecta a un servicio de AEM Publish y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-activity`
