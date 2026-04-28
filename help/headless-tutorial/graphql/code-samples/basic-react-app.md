---
title: Aplicación React básica
description: Una aplicación básica de React que muestra una lista de las aventuras de WKND y sus detalles
version: Experience Manager as a Cloud Service
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
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 8%

---

# Aplicación React básica

Esta aplicación [React](https://reactjs.org/) muestra cómo consultar contenido mediante las API de GraphQL de AEM usando consultas persistentes. Esta aplicación presenta un filtro de WKND Adventures, y al seleccionar una aventura, muestra las aventuras con todos los detalles.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`

For a more indepth review of how this Next.js app is built, review the [example React app documentation](../example-apps/react-app.md).
