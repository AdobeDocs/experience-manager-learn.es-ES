---
title: Aplicación Next.js básica
description: Una aplicación básica de Next.js que muestra una lista de las aventuras de WKND y sus detalles
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: e3fb145e7a9f33dd010f6c40e42573d41e54b302
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---


# Aplicación Next.js básica

Esta [Next.js](https://nextjs.org/) muestra cómo consultar contenido mediante las API de GraphQL de AEM mediante consultas persistentes. Esta aplicación presenta un filtro de WKND Adventures, y al seleccionar una aventura, muestra las aventuras con todos los detalles.

Este código:

+ Se conecta a un servicio de AEM Publish y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`

Para obtener una descripción más detallada sobre cómo se crea esta aplicación Next.js, consulte la [ejemplo de documentación de la aplicación Next.js](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io no admite la edición de la aplicación Next.js en el IDE incrustado. Para editar este ejemplo de código, [abra la aplicación Next.js directamente en codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-3n6zdv).
