---
title: Aplicación React básica
description: Una aplicación básica de React que muestra una lista de las aventuras de WKND y sus detalles
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
duration: 17
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---

# Aplicación React básica

Esta [Reaccionar](https://reactjs.org/) AEM La aplicación muestra cómo consultar contenido mediante API de GraphQL de mediante consultas persistentes. Esta aplicación presenta un filtro de WKND Adventures, y al seleccionar una aventura, muestra las aventuras con todos los detalles.

Este código:

+ AEM Se conecta a un servicio de publicación de y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`

Para obtener una descripción más detallada de cómo se crea esta aplicación Next.js, consulte la [ejemplo de documentación de la aplicación React](../example-apps/react-app.md).
