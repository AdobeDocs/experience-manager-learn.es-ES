---
title: Otras herramientas para depurar AEM SDK
description: Hay otras herramientas que pueden ayudar a depurar el inicio rápido local de AEM SDK.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
duration: 107
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 1%

---

# Otras herramientas para depurar AEM SDK

Hay otras herramientas que pueden ayudarle a depurar la aplicación en el inicio rápido local de AEM SDK.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite es una interfaz basada en web para interactuar con JCR, el repositorio de datos de AEM. CRXDE Lite proporciona una visibilidad completa del JCR, incluidos nodos, propiedades, valores de propiedad y permisos.

CRXDE Lite se encuentra en:

+ Herramientas > General > CRXDE Lite
+ o directamente en [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Depuración de contenido

CRXDE Lite proporciona acceso directo al JCR. El contenido visible a través de CRXDE Lite está limitado por los permisos otorgados al usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR según su acceso.

+ La estructura JCR se navega y manipula mediante el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se exponen las propiedades del nodo en el panel inferior.
   + Las propiedades se pueden agregar, eliminar o cambiar en el panel
+ Al hacer doble clic en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Pulse el botón Guardar todo en la parte superior izquierda para mantener los cambios, o la flecha hacia abajo junto a Guardar todo para revertir los cambios no guardados.

![CRXDE Lite - Depurando contenido](./assets/other-tools/crxde-lite__debugging-content.png)

Cualquier cambio realizado directamente en AEM SDK a través de CRXDE Lite puede resultar difícil de rastrear y controlar. Si corresponde, asegúrese de que los cambios realizados mediante CRXDE Lite regresen a los paquetes de contenido mutable del proyecto AEM (`ui.content`) y se confirmen en Git. Lo ideal es que todos los cambios en el contenido de la aplicación se originen en la base de código y se transfieran a AEM SDK mediante implementaciones, en lugar de realizar cambios directamente en AEM SDK mediante CRXDE Lite.

### Depuración de controles de acceso

CRXDE Lite proporciona una forma de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola de Control de acceso de prueba en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar control de acceso...

![CRXDE Lite - Probar control de acceso](./assets/other-tools/crxde-lite__test-access-control.png)

1. En el campo Ruta, seleccione una Ruta JCR para evaluar
1. En el campo Principal, seleccione el usuario o el grupo con el que desea evaluar la ruta
1. Pulse el botón Probar

Los resultados se muestran a continuación:

+ __Ruta__ reitera la ruta que se evaluó
+ __Principal__ reitera al usuario o grupo para el que se evaluó la ruta
+ __Principales__ enumera todas las principales de las que forma parte el principal seleccionado.
   + Esto resulta útil para comprender las pertenencias a grupos transitivas que pueden proporcionar permisos a través de la herencia
+ __Privilegios en la ruta__ enumera todos los permisos JCR que la entidad de seguridad seleccionada tiene en la ruta evaluada

## Explicar la consulta

![Explicar consulta](./assets/other-tools/explain-query.png)

Explique la herramienta basada en web Query en el inicio rápido local de AEM SDK, que proporciona información clave sobre cómo AEM interpreta y ejecuta consultas, y una herramienta inestimable para garantizar que AEM ejecuta las consultas de forma eficaz.

Explicar consulta se encuentra en:

+ Herramientas > Diagnóstico > Rendimiento de la consulta > Explicar la pestaña de consulta
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > pestaña Explicar consulta

## QueryBuilder Debugger

![Depurador de QueryBuilder](./assets/other-tools/query-debugger.png)

QueryBuilder Debugger es una herramienta basada en la web que le ayuda a depurar y comprender las consultas de búsqueda mediante la sintaxis [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) de AEM.

QueryBuilder Debugger se encuentra en:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
