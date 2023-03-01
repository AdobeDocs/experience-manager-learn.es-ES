---
title: Aplicación básica Next.js
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
source-git-commit: 772acab316ba2ff463fa5cacff02013bea920579
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---


# Aplicación básica Next.js

Esta [Next.js](https://nextjs.org/) AEM La aplicación muestra cómo consultar contenido mediante API de GraphQL de mediante consultas persistentes. Esta aplicación presenta un filtro de WKND Adventures, y al seleccionar una aventura, muestra las aventuras con todos los detalles.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`

Para obtener una descripción más detallada de cómo se crea esta aplicación Next.js, consulte la [ejemplo de documentación de la aplicación Next.js](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io no admite la edición de la aplicación Next.js en el IDE incrustado. Para editar este ejemplo de código, [abra la aplicación Next.js directamente en codesandbox.io.](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
