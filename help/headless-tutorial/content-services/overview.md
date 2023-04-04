---
title: 'Introducción a AEM sin encabezado: servicios de contenido'
description: Un tutorial completo que ilustra cómo crear y exponer contenido mediante AEM sin encabezado.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 5aa32791-861a-48e3-913c-36028373b788
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 6%

---

# Introducción a AEM sin encabezado - Content Services

AEM Content Services aprovecha las páginas AEM tradicionales para componer extremos de API de REST sin encabezado y AEM Componentes definen, o hacen referencia, el contenido que se expone en estos extremos.

AEM Content Services permite las mismas abstracciones de contenido utilizadas para crear páginas web en AEM Sites, para definir el contenido y los esquemas de estas API HTTP. El uso de AEM páginas y componentes de AEM permite a los especialistas en marketing componer y actualizar rápidamente API de JSON flexibles que pueden impulsar cualquier aplicación.

## Tutorial de servicios de contenido

Un tutorial completo que ilustra cómo crear y exponer contenido mediante AEM y consumido por una aplicación móvil nativa en un escenario de CMS remoto.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

Este tutorial explora cómo se puede utilizar AEM Content Services para impulsar la experiencia de una aplicación móvil que muestra información de eventos (música, rendimiento, arte, etc.) que es depurado por el equipo de WKND.

Este tutorial tratará los siguientes temas:

* Crear contenido que represente un evento usando fragmentos de contenido
* Definir puntos finales de servicios de contenido de AEM mediante las plantillas y páginas de AEM Sites que exponen los datos de evento como JSON
* Descubra cómo se pueden utilizar AEM componentes principales de WCM para permitir que los especialistas en marketing creen puntos finales de JSON
* Consumir AEM JSON de Content Services desde una aplicación móvil
   * El uso de Android se debe a que tiene un emulador entre plataformas que todos los usuarios (Windows, macOS y Linux) de este tutorial pueden utilizar para ejecutar la aplicación nativa.

## Proyecto de GitHub

El código fuente y los paquetes de contenido están disponibles en la [Guías de AEM: proyecto de WKND Mobile GitHub](https://github.com/adobe/aem-guides-wknd-mobile).

Si encuentra algún problema con el tutorial o el código, deje un [Problema de GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM de GraphQL y servicios de contenido AEM

|  | API de AEM GraphQL | Servicios de contenido AEM |
|--------------------------------|:-----------------|:---------------------|
| Definición del esquema | Modelos de fragmento de contenido estructurados | Componentes AEM |
| Contenido | Fragmentos de contenido | Componentes AEM |
| Descubrimiento de contenido | Por consulta de GraphQL | Por AEM página |
| Formato de entrega | GraphQL JSON | AEM JSON de ComponentExporter |
