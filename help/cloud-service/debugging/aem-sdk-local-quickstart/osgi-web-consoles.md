---
title: Depuración AEM SDK mediante la consola web OSGi
description: El inicio rápido local del SDK de AEM tiene una consola web OSGi que proporciona una variedad de información e introspecciones en el tiempo de ejecución de AEM local que son útiles para comprender cómo se reconoce la aplicación y funcionan dentro de AEM.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 1%

---


# Depuración AEM SDK mediante la consola web OSGi

El inicio rápido local del SDK de AEM tiene una consola web OSGi que proporciona una variedad de información e introspecciones en el tiempo de ejecución de AEM local que son útiles para comprender cómo se reconoce la aplicación y funcionan dentro de AEM.

AEM proporciona muchas consolas OSGi, cada una de las cuales proporciona perspectivas clave sobre diferentes aspectos de AEM, aunque las siguientes suelen ser las más útiles para depurar la aplicación.

## Paquetes

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

La consola Bundles es un catálogo de los paquetes OSGi, y sus detalles, implementados en AEM, junto con la capacidad ad hoc de inicio y parada de los mismos.

La consola Bundles se encuentra en:

+ Herramientas > Operaciones > Consola Web > OSGi > Paquetes
+ Or directly at: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Al hacer clic en cada paquete, se proporcionan detalles que ayudan a depurar la aplicación.

+ La validación del paquete OSGi está presente
+ Validación si un paquete OSGi está activo
+ Determinar si un paquete OSGi tiene importaciones insatisfechas que impiden que comience

## Componentes

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

La consola Componentes es un catálogo de todos los componentes OSGi implementados en AEM y proporciona toda la información sobre ellos, desde su ciclo de vida de componente OSGi definido hasta los servicios OSGi a los que pueden hacer referencia

La consola Componentes se encuentra en:

+ Herramientas > Operaciones > Consola web > OSGi > Componentes
+ Or directly at: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Aspectos clave que ayudan con las actividades de depuración:

+ La validación del paquete OSGi está presente
+ Validación si un paquete OSGi está activo
+ Determinar si un paquete OSGi tiene importaciones insatisfechas que impiden que comience
+ Obtención del PID del componente, para crear configuraciones OSGi para ellos en Git
+ Identificación de los valores de propiedad OSGi enlazados a la configuración OSGi activa

## Modelos Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

La consola Modelos Sling se encuentra en:

+ Herramientas > Operaciones > Consola Web > Estado > Modelos Sling
+ Or directly at: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Aspectos clave que ayudan con las actividades de depuración:

+ La validación de modelos Sling se registra en el tipo de recurso adecuado
+ La validación de modelos Sling se puede adaptar desde los objetos correctos (Resource o SlingHttpRequestServlet)
+ La validación de los exportadores del modelo de Sling está correctamente registrada
