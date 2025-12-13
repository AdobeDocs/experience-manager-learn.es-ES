---
title: 'Introducción a AEM Headless: Servicios de contenido'
description: Un tutorial completo que ilustra cómo crear y exponer contenido mediante AEM sin encabezado.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 100%

---

# Introducción a AEM Headless: Servicios de contenido

Los servicios de contenido de AEM aprovechan las páginas tradicionales de AEM para componer puntos finales de API de REST sin encabezado y los componentes de AEM definen o hacen referencia al contenido que se va a exponer en estos puntos finales.

AEM Content Services permite utilizar las mismas abstracciones de contenido utilizadas para crear páginas web en AEM Sites para definir el contenido y los esquemas de estas API HTTP. El uso de Páginas de AEM y Componentes de AEM permite a los especialistas en marketing componer y actualizar rápidamente API de JSON flexibles que pueden activar cualquier aplicación.

## Tutorial de servicios de contenido

Un tutorial completo que ilustra cómo crear y exponer contenido mediante AEM y consumido por una aplicación móvil nativa, en un escenario de CMS sin encabezado.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

Este tutorial explora cómo se pueden utilizar los Servicios de contenido de AEM para potenciar la experiencia de una aplicación móvil que muestra información de eventos (música, actuaciones, arte, etc.) depurada por el equipo de WKND.

Este tutorial abarcará los siguientes temas:

* Creación de contenido que represente un evento mediante fragmentos de contenido
* Defina los puntos finales de los Servicios de contenido de AEM con las plantillas de AEM Sites y las páginas que exponen los datos del evento como JSON
* Descubra cómo se pueden utilizar los componentes principales de AEM WCM para permitir que los especialistas en marketing creen puntos finales de JSON
* Consumir el JSON de Servicios de contenido de AEM desde una aplicación móvil
   * El uso de Android se debe a que tiene un emulador multiplataforma que todos los usuarios (Windows, macOS y Linux) de este tutorial pueden utilizar para ejecutar la aplicación nativa.

## Proyecto de GitHub

El código fuente y los paquetes de contenido están disponibles en [AEM Guides - Proyecto WKND Mobile de GitHub](https://github.com/adobe/aem-guides-wknd-mobile).

Si encuentra algún problema con el tutorial o el código, deje un [problema de GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## GraphQL de AEM y servicios de contenido de AEM

|                                | API de GraphQL de AEM | Servicios de contenido de AEM |
|--------------------------------|:-----------------|:---------------------|
| Definición de esquema | Modelos de fragmento de contenido estructurado | Componentes de AEM |
| Contenido | Fragmentos de contenido | Componentes de AEM |
| Detección de contenido | Por consulta de GraphQL | Por página de AEM |
| Formato de distribución | GraphQL JSON | JSON del exportador de componentes de AEM |
