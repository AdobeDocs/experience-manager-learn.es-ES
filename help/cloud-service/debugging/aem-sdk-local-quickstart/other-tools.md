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
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 2%

---

# Otras herramientas para depurar AEM SDK

Hay otras herramientas que pueden ayudar a depurar la aplicación en el inicio rápido local del SDK de AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite es una interfaz basada en web para interactuar con el repositorio de datos AEM JCR. CRXDE Lite proporciona una visibilidad completa del JCR, incluidos nodos, propiedades, valores de propiedad y permisos.

El CRXDE Lite se encuentra en:

+ Herramientas > General > CRXDE Lite
+ o directamente en [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Depuración de contenido

CRXDE Lite proporciona acceso directo al JCR. El contenido visible mediante CRXDE Lite está limitado por los permisos otorgados al usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR en función de su acceso.

+ La estructura JCR se navega y manipula con el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se muestra la propiedad del nodo en el panel inferior.
   + Las propiedades se pueden agregar, quitar o cambiar desde el panel
+ Al hacer doble clic en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Pulse el botón Guardar todo en la parte superior izquierda para mantener los cambios o la flecha hacia abajo situada junto a Guardar todo para revertir los cambios sin guardar.

![CRXDE Lite: Depuración de contenido](./assets/other-tools/crxde-lite__debugging-content.png)

Cualquier cambio realizado directamente en AEM SDK mediante CRXDE Lite puede ser difícil de rastrear y gobernar. Si procede, asegúrese de que los cambios realizados mediante el CRXDE Lite regresan a los paquetes de contenido mutable del proyecto de AEM (`ui.content`) y comprometido con Git. Lo ideal es que todos los cambios en el contenido de la aplicación se originen en la base de código y se transfieran a AEM SDK mediante implementaciones, en lugar de realizar cambios directamente en el SDK de AEM mediante CRXDE Lite.

### Depuración de controles de acceso

CRXDE Lite proporciona una forma de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola Control de acceso de prueba en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar control de acceso...

![CRXDE Lite - Control de acceso de prueba](./assets/other-tools/crxde-lite__test-access-control.png)

1. Con el campo Ruta , seleccione una ruta JCR para evaluar
1. Con el campo Principal, seleccione el usuario o grupo con el que valorar la ruta
1. Puntee en el botón Probar

Los resultados se muestran a continuación:

+ __Ruta__ reitera la ruta evaluada
+ __Principal__ reitera al usuario o grupo para el que se evaluó la ruta
+ __Principios__ enumera todas las entidades principales de las que forma parte la entidad de seguridad seleccionada.
   + Esto resulta útil para comprender las pertenencias transitivas a grupos que pueden proporcionar permisos mediante herencia
+ __Privilegios en ruta__ enumera todos los permisos JCR que la entidad de seguridad seleccionada tiene en la ruta evaluada

## Explicar la consulta

![Explicar la consulta](./assets/other-tools/explain-query.png)

Explique la herramienta basada en web Query en AEM inicio rápido local del SDK, que proporciona perspectivas clave sobre cómo AEM interpreta y ejecuta consultas, y una herramienta invaluable para garantizar que AEM ejecutan las consultas de una manera eficaz.

Explicar que la consulta se encuentra en:

+ Herramientas > Diagnóstico > Rendimiento de la consulta > Explicar la pestaña Consulta
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Explicar ficha Consulta

## Depurador de QueryBuilder

![Depurador de QueryBuilder](./assets/other-tools/query-debugger.png)

QueryBuilder Debugger es una herramienta basada en la web que ayuda a depurar y comprender las consultas de búsqueda mediante AEM [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) sintaxis.

QueryBuilder Debugger se encuentra en:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
