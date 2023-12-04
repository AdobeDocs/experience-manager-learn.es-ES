---
title: AEM Depuración del SDK de mediante la consola web de OSGi
description: AEM AEM AEM El inicio rápido local del SDK de la tiene una consola web OSGi que proporciona una variedad de información e introspecciones en el tiempo de ejecución de la aplicación local, que son útiles para comprender cómo la aplicación es reconocida por las funciones y dentro de las funciones de la aplicación.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 516
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 1%

---

# AEM Depuración del SDK de mediante la consola web de OSGi

AEM AEM AEM El inicio rápido local del SDK de la tiene una consola web OSGi que proporciona una variedad de información e introspecciones en el tiempo de ejecución de la aplicación local, que son útiles para comprender cómo la aplicación es reconocida por las funciones y dentro de las funciones de la aplicación.

AEM AEM proporciona muchas consolas OSGi, cada una de las cuales proporciona perspectivas clave sobre diferentes aspectos de la aplicación, aunque las siguientes son generalmente las más útiles para depurar la aplicación.

## Paquetes

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

AEM La consola Paquetes es un catálogo de los paquetes OSGi, y sus detalles, implementados para la creación de listas de paquetes, junto con la capacidad ad hoc para iniciarlos y detenerlos.

La consola Paquetes se encuentra en:

+ Herramientas > Operaciones > Consola web > OSGi > Paquetes
+ O directamente en: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Al hacer clic en cada paquete, se proporcionan detalles que ayudan a depurar la aplicación.

+ Validando que el paquete OSGi esté presente
+ Validación si un paquete OSGi está activo
+ Determinar si un paquete OSGi tiene importaciones no satisfechas que impidan que se inicie

## Componentes

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

AEM La consola Componentes es un catálogo de todos los componentes OSGi implementados para la aplicación y proporciona toda la información sobre ellos, desde su ciclo de vida definido del componente OSGi, hasta a qué servicios OSGi pueden hacer referencia

La consola Componentes se encuentra en:

+ Herramientas > Operaciones > Consola web > OSGi > Componentes
+ O directamente en: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Aspectos clave que ayudan con las actividades de depuración:

+ Validando que el paquete OSGi esté presente
+ Validación si un paquete OSGi está activo
+ Determinar si un paquete OSGi tiene importaciones no satisfechas que impidan que se inicie
+ Obtener el PID del componente para crear configuraciones OSGi para ellos en Git
+ Identificación de los valores de propiedad OSGi enlazados a la configuración OSGi activa

## Modelos Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

La consola Modelos de Sling se encuentra en:

+ Herramientas > Operaciones > Consola web > Estado > Modelos Sling
+ O directamente en: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Aspectos clave que ayudan con las actividades de depuración:

+ La validación de los modelos Sling está registrada en el tipo de recurso adecuado
+ La validación de modelos Sling se puede adaptar desde los objetos correctos (Resource o SlingHttpRequestServlet)
+ La validación de los exportadores de modelos Sling está registrada correctamente
