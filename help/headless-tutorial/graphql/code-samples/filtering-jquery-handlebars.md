---
title: Filtrado con jQuery y Handlebars
description: Implementación de JavaScript con jQuery y Handlebars que filtra WKND Adventures para mostrar. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Filtrado con jQuery y Handlebars

AEM Explore la capacidad de las API de GraphQL sin encabezado para filtrar datos mediante una aplicación de JavaScript que usa [jQuery](https://jquery.com/) y [Handlebars](https://handlebarsjs.com/). Esta aplicación crea una lista de las aventuras de WKND que se pueden filtrar por Tipo de actividad.

Este código muestra cómo usar el cliente sin encabezado [de Adobe AEM para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas de GraphQL persistentes. Esta aplicación usa la consulta persistente `wknd-shared/adventures-all` para recopilar todas las aventuras y derivar una lista de tipos de actividades disponibles. Cuando un usuario selecciona un Tipo de actividad, el tipo seleccionado se pasa a la consulta persistente `wknd-shared/adventures-by-activity` y recupera los detalles de la aventura solo para aquellas aventuras del Tipo de actividad especificado.

Este código:

+ AEM Se conecta a un servicio de Publish de y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-activity`
