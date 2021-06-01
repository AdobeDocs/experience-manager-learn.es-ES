---
title: 'Introducción a AEM sin encabezado: servicios de contenido'
description: Un tutorial completo que ilustra cómo crear y exponer contenido mediante AEM sin encabezado.
feature: Fragmentos de contenido, API
topic: Sin objetivos, Administración de contenido
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 4%

---


# Introducción a AEM sin encabezado: servicios de contenido

AEM Content Services aprovecha las páginas AEM tradicionales para componer extremos de API de REST sin encabezado y AEM Componentes definen, o hacen referencia, el contenido que se expone en estos extremos.

AEM Content Services permite las mismas abstracciones de contenido utilizadas para crear páginas web en AEM Sites, para definir el contenido y los esquemas de estas API HTTP. El uso de AEM páginas y componentes de AEM permite a los especialistas en marketing componer y actualizar rápidamente API de JSON flexibles que pueden impulsar cualquier aplicación.

## Tutorial de servicios de contenido

Un tutorial completo que ilustra cómo crear y exponer contenido mediante AEM y consumido por una aplicación móvil nativa en un escenario de CMS remoto.

>[!VIDEO](https://video.tv.adobe.com/v/28315/?quality=12&learn=on)

Este tutorial explora cómo se puede utilizar AEM Content Services para impulsar la experiencia de una aplicación móvil que muestra información de eventos (música, rendimiento, arte, etc.) que es depurado por el equipo de WKND.

Este tutorial tratará los siguientes temas:

* Crear contenido que represente un evento usando fragmentos de contenido
* Definir puntos finales de servicios de contenido de AEM mediante las plantillas y páginas de AEM Sites que exponen los datos de evento como JSON
* Descubra cómo se pueden utilizar AEM componentes principales de WCM para permitir que los especialistas en marketing creen puntos finales de JSON
* Consumir AEM JSON de Content Services desde una aplicación móvil
   * El uso de Android se debe a que tiene un emulador entre plataformas que todos los usuarios (Windows, macOS y Linux) de este tutorial pueden utilizar para ejecutar la aplicación nativa.

## Proyecto de GitHub

El código fuente y los paquetes de contenido están disponibles en las [Guías de AEM - Proyecto GitHub móvil de WKND](https://github.com/adobe/aem-guides-wknd-mobile).

Si encuentra algún problema con el tutorial o el código, deje un [problema de GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL y servicios de contenido AEM

|  | API de AEM GraphQL | Servicios de contenido AEM |
|--------------------------------|:-----------------|:---------------------|
| Definición del esquema | Modelos de fragmento de contenido estructurados | Componentes AEM |
| Contenido | Fragmentos de contenido | Componentes AEM |
| Descubrimiento de contenido | Por consulta de GraphQL | Por AEM página |
| Formato de entrega | JSON de GraphQL | AEM JSON de ComponentExporter |
