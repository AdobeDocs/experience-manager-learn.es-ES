---
title: Aplicación básica Next.js
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# Aplicación básica Next.js

Esta aplicación [Next.js](https://nextjs.org/) muestra cómo consultar contenido mediante las API GraphQL de AEM usando consultas persistentes. Esta aplicación presenta un filtro de WKND Adventures, y al seleccionar una aventura, muestra las aventuras con todos los detalles.

Este código:

+ Se conecta a un servicio de publicación de AEM y no requiere autenticación
+ Utiliza las consultas persistentes de WKND: `wknd-shared/adventures-all` y `wknd-shared/adventures-by-slug`

Para obtener una revisión más detallada de cómo se crea esta aplicación Next.js, consulte [ejemplo de documentación de la aplicación Next.js](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io no admite la edición de la aplicación Next.js en el IDE incrustado. Para editar este ejemplo de código, [abra la aplicación Next.js directamente en codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
