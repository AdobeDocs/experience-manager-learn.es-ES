---
title: Otras herramientas para depurar el SDK de AEM
description: Hay otras herramientas que pueden ayudar a depurar el inicio rápido local del SDK de AEM.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 5%

---


# Otras herramientas para depurar el SDK de AEM

Hay otras herramientas que pueden ayudar a depurar la aplicación en el inicio rápido local del SDK de AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite es una interfaz basada en la web para interactuar con JCR, el repositorio de datos de AEM. CRXDE Lite proporciona una visibilidad completa del JCR, incluidos nodos, propiedades, valores de propiedad y permisos.

CRXDE Lite se encuentra en:

+ Herramientas > General > CRXDE Lite
+ o directamente en [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Explicar la consulta

![Explicar la consulta](./assets/other-tools/explain-query.png)

Explique la herramienta basada en web Query en el inicio rápido local del SDK de AEM, que proporciona perspectivas clave sobre cómo AEM interpreta y ejecuta consultas, y una herramienta invaluable para garantizar que AEM ejecute las consultas de forma eficaz.

Explicar que la consulta se encuentra en:

+ Herramientas > Diagnóstico > Rendimiento de la consulta > Explicar la pestaña Consulta
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)  > Explicar la pestaña Consulta

## Depurador de QueryBuilder

![Depurador de QueryBuilder](./assets/other-tools/query-debugger.png)

QueryBuilder debugger es una herramienta basada en la web que le ayuda a depurar y comprender las consultas de búsqueda utilizando la sintaxis [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) de AEM.

QueryBuilder Debugger se encuentra en:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log Tracer y el complemento AEM Chrome

![Sling Log Tracer y el complemento de AEM Chrome](./assets/other-tools/log-tracer.png)

[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html), que se envía con el inicio rápido local del SDK de AEM, permite un seguimiento en profundidad de las solicitudes HTTP, exponiendo información de depuración en profundidad por solicitud. La configuración OSGi [Log Tracer debe configurarse](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1) para habilitar esta función.

El complemento [AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US) de código abierto para el [explorador web de Google Chrome](https://www.google.com/chrome/) se integra con Log Tracer, exponiendo la información de depuración directamente en las herramientas de desarrollo de Chrome.

_El complemento de AEM Chrome es una herramienta de código abierto y Adobe no proporciona soporte para ella._

