---
title: Otras herramientas para depurar AEM SDK
description: Otras herramientas pueden ayudar a depurar el inicio rápido local del SDK de AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 5%

---

# Otras herramientas para depurar AEM SDK

Hay otras herramientas que pueden ayudar a depurar la aplicación en el inicio rápido local del SDK de AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite es una interfaz basada en web para interactuar con el repositorio de datos AEM JCR. CRXDE Lite proporciona una visibilidad completa del JCR, incluidos nodos, propiedades, valores de propiedad y permisos.

El CRXDE Lite se encuentra en:

+ Herramientas > General > CRXDE Lite
+ o directamente en [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Explicar la consulta

![Explicar la consulta](./assets/other-tools/explain-query.png)

Explique la herramienta basada en web Query en AEM inicio rápido local del SDK, que proporciona perspectivas clave sobre cómo AEM interpreta y ejecuta consultas, y una herramienta invaluable para garantizar que AEM ejecutan las consultas de una manera eficaz.

Explicar que la consulta se encuentra en:

+ Herramientas > Diagnóstico > Rendimiento de la consulta > Explicar la pestaña Consulta
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)  > Explicar la pestaña Consulta

## Depurador de QueryBuilder

![Depurador de QueryBuilder](./assets/other-tools/query-debugger.png)

QueryBuilder debugger es una herramienta basada en la web que le ayuda a depurar y comprender las consultas de búsqueda mediante AEM sintaxis [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

QueryBuilder Debugger se encuentra en:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
