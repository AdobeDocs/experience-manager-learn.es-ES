---
title: Filtrado de consultas
description: Implementación de JavaScript que permite seleccionar fragmentos de contenido específicos y luego mostrar sus detalles .
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
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Filtrado de consultas

Este ejemplo de JavaScript y Handlebars ilustra cómo filtrar los resultados de GraphQL y mostrar los resultados seleccionados.

Este código:

+ Conecta a [wknd.site](https://wknd.site)El servicio AEM Publish de y no requiere autenticación
+ Utiliza las consultas persistentes: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`
