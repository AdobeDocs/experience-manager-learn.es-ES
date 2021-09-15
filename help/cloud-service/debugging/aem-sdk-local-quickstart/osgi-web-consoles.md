---
title: Depuración AEM SDK mediante la consola web OSGi
description: El inicio rápido local del SDK de AEM tiene una consola web OSGi que proporciona una variedad de información e introspecciones en el tiempo de ejecución de AEM local que son útiles para comprender cómo se reconoce su aplicación y cómo funciona dentro de AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 2%

---

# Depuración AEM SDK mediante la consola web OSGi

El inicio rápido local del SDK de AEM tiene una consola web OSGi que proporciona una variedad de información e introspecciones en el tiempo de ejecución de AEM local que son útiles para comprender cómo se reconoce su aplicación y cómo funciona dentro de AEM.

AEM proporciona muchas consolas OSGi, cada una de las cuales proporciona perspectivas clave sobre diferentes aspectos de AEM, aunque las siguientes suelen ser las más útiles para depurar la aplicación.

## Paquetes

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

La consola Paquetes es un catálogo de los paquetes OSGi, y sus detalles, implementados en AEM, junto con la capacidad ad hoc para iniciarlos y detenerlos.

La consola Paquetes se encuentra en:

+ Herramientas > Operaciones > Consola web > OSGi > Paquetes
+ O directamente en: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Al hacer clic en cada paquete, se proporcionan detalles que ayudan a depurar la aplicación.

+ Validación del paquete OSGi presente
+ Validación si un paquete OSGi está activo
+ Determinar si un paquete OSGi tiene importaciones insatisfechas que impiden que se inicie

## Componentes

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

La consola Componentes es un catálogo de todos los componentes de OSGi implementados en AEM y proporciona toda la información sobre ellos, desde su ciclo de vida de componentes OSGi definido, hasta a qué servicios de OSGi pueden hacer referencia

La consola Componentes se encuentra en:

+ Herramientas > Operaciones > Consola web > OSGi > Componentes
+ O directamente en: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Aspectos clave que ayudan con las actividades de depuración:

+ Validación del paquete OSGi presente
+ Validación si un paquete OSGi está activo
+ Determinar si un paquete OSGi tiene importaciones insatisfechas que impiden que se inicie
+ Obtención del PID del componente, para crear configuraciones OSGi para ellos en Git
+ Identificación de valores de propiedades OSGi enlazados a la configuración OSGi activa

## Modelos Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

La consola Modelos de Sling se encuentra en:

+ Herramientas > Operaciones > Consola web > Estado > Modelos de Sling
+ O directamente en: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Aspectos clave que ayudan con las actividades de depuración:

+ La validación de los modelos Sling se registra con el tipo de recurso adecuado
+ La validación de los modelos Sling se puede adaptar desde los objetos correctos (Resource o SlingHttpRequestServlet)
+ La validación de los exportadores del modelo de Sling está correctamente registrada
