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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---

# Aplicación React básica

Esta aplicación [React](https://reactjs.org/) muestra cómo consultar contenido mediante las API de GraphQL de AEM usando consultas persistentes. Esta aplicación presenta un filtro de WKND Adventures, y al seleccionar una aventura, muestra las aventuras con todos los detalles.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`

Para obtener una revisión más detallada de cómo se crea esta aplicación Next.js, revise la [documentación de la aplicación React de ejemplo](../example-apps/react-app.md).
