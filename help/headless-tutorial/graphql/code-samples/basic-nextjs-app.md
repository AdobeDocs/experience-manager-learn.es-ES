---
title: Aplicación Next.js básica
description: Una aplicación básica de Next.js que muestra una lista de las aventuras de WKND y sus detalles
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 22
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 6%

---

# Aplicación Next.js básica

Esta aplicación [Next.js](https://nextjs.org/) muestra cómo consultar contenido mediante las API GraphQL de AEM usando consultas persistentes. Esta aplicación presenta un filtro de WKND Adventures, y al seleccionar una aventura, muestra las aventuras con todos los detalles.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`

>[!IMPORTANT]
>
> Codesandbox.io no admite la edición de la aplicación Next.js en el IDE incrustado. Para editar este ejemplo de código, [abra la aplicación Next.js directamente en codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
