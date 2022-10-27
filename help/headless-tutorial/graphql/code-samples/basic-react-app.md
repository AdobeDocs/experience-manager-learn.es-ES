---
title: Aplicación React básica
description: Una aplicación React básica que muestra una lista de las aventuras de WKND y sus detalles
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 2%

---


# Aplicación React básica

Esta [React](https://reactjs.org/) muestra cómo consultar contenido mediante las API de GraphQL de AEM mediante consultas persistentes. Esta aplicación presenta un filtro de WKND Adventures, y al seleccionar una aventura, muestra las aventuras con todos los detalles.

Este código:

+ Se conecta a un servicio de AEM Publish y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`

Para obtener una descripción más detallada sobre cómo se crea esta aplicación Next.js, consulte la [ejemplo de documentación de la aplicación React](../example-apps/react-app.md).
