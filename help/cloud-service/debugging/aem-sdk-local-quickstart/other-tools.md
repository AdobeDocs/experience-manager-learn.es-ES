---
title: AEM Otras herramientas para depurar el SDK de la
description: AEM Hay otras herramientas que pueden ayudar a depurar el inicio rápido local del SDK de la.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
duration: 148
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 1%

---

# AEM Otras herramientas para depurar el SDK de la

AEM Hay otras herramientas que pueden ayudarle a depurar la aplicación en el inicio rápido local del SDK de la.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite AEM es una interfaz basada en web para interactuar con el repositorio de datos JCR, de la red de distribución de datos (JCR) de la red de distribución de datos (). CRXDE Lite proporciona una visibilidad completa del JCR, incluidos nodos, propiedades, valores de propiedad y permisos.

El CRXDE Lite se encuentra en:

+ Herramientas > General > CRXDE Lite
+ o directamente en [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Depuración de contenido

El CRXDE Lite de proporciona acceso directo al JCR. El contenido visible mediante CRXDE Lite está limitado por los permisos otorgados al usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR según su acceso.

+ La estructura JCR se navega y manipula mediante el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se exponen las propiedades del nodo en el panel inferior.
   + Las propiedades se pueden agregar, eliminar o cambiar en el panel
+ Al hacer doble clic en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Pulse el botón Guardar todo en la parte superior izquierda para mantener los cambios, o la flecha hacia abajo junto a Guardar todo para revertir los cambios no guardados.

![CRXDE Lite - Depuración de contenido](./assets/other-tools/crxde-lite__debugging-content.png)

AEM Cualquier cambio realizado directamente en el SDK de la a través del CRXDE Lite puede resultar difícil de rastrear y controlar. Si corresponde, asegúrese de que los cambios realizados mediante CRXDE Lite AEM regresen a los paquetes de contenido mutable del proyecto de la (`ui.content`) y se ha comprometido con Git. AEM AEM Lo ideal es que todos los cambios en el contenido de la aplicación se originen en la base de código y fluyan hacia el SDK de la aplicación a través de implementaciones, en lugar de realizar cambios directamente en el SDK de la aplicación a través de un CRXDE Lite.

### Depuración de controles de acceso

CRXDE Lite proporciona una forma de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola de Control de acceso de prueba en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar control de acceso ...

![CRXDE Lite - Probar control de acceso](./assets/other-tools/crxde-lite__test-access-control.png)

1. En el campo Ruta, seleccione una Ruta JCR para evaluar
1. En el campo Principal, seleccione el usuario o el grupo con el que desea evaluar la ruta
1. Pulse el botón Probar

Los resultados se muestran a continuación:

+ __Ruta__ reitera la ruta evaluada
+ __Principal__ reitera el usuario o grupo para el que se evaluó la ruta
+ __Entidades__ enumera todas las entidades de seguridad de las que forma parte la entidad de seguridad seleccionada.
   + Esto resulta útil para comprender las pertenencias a grupos transitivas que pueden proporcionar permisos a través de la herencia
+ __Privilegios en la ruta__ enumera todos los permisos JCR que tiene la entidad de seguridad seleccionada en la ruta evaluada

## Explicar la consulta

![Explicar consulta](./assets/other-tools/explain-query.png)

AEM AEM AEM Explicar la herramienta basada en web de Query en el inicio rápido local de SDK, que proporciona información clave sobre cómo interpreta y ejecuta consultas, así como una herramienta muy valiosa para garantizar que las consultas se ejecutan de forma eficaz por parte de los usuarios de SDK de la forma que más se ha.

Explicar consulta se encuentra en:

+ Herramientas > Diagnóstico > Rendimiento de la consulta > Explicar la pestaña de consulta
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > pestaña Explicar consulta

## QueryBuilder Debugger

![QueryBuilder Debugger](./assets/other-tools/query-debugger.png)

AEM QueryBuilder Debugger es una herramienta basada en la web que le ayuda a depurar y comprender las consultas de búsqueda mediante el uso de la función de depuración de [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) sintaxis.

QueryBuilder Debugger se encuentra en:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
