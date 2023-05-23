---
title: AEM Introducción a los servicios de contenido sin encabezado de
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

AEM AEM AEM Los servicios de contenido aprovechan las páginas de tradicionales para componer extremos de API de REST sin encabezado, y los componentes definen, o hacen referencia, al contenido que se va a exponer en estos extremos.

AEM Los servicios de contenido permiten las mismas abstracciones de contenido utilizadas para crear páginas web en AEM Sites para definir el contenido y los esquemas de estas API HTTP. AEM AEM El uso de Páginas de y Componentes de permite a los especialistas en marketing componer y actualizar rápidamente API de JSON flexibles que pueden activar cualquier aplicación.

## Tutorial de servicios de contenido

AEM Un tutorial completo que ilustra cómo crear y exponer contenido mediante y consumido por una aplicación móvil nativa, en un escenario de CMS sin encabezado.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

AEM Este tutorial explora cómo se pueden utilizar los servicios de contenido de la para potenciar la experiencia de una aplicación móvil que muestra información de eventos (música, rendimiento, arte, etc.) que está depurado por el equipo de WKND.

Este tutorial abarca los siguientes temas:

* Creación de contenido que represente un evento mediante fragmentos de contenido
* AEM Defina un punto final de servicios de contenido de AEM mediante plantillas de AEM Sites y páginas que exponen los datos del evento como JSON
* AEM Descubra cómo se pueden utilizar los componentes principales de WCM para que los especialistas en marketing puedan crear puntos finales JSON de forma predeterminada
* AEM Consumir JSON de servicios de contenido de desde una aplicación móvil
   * El uso de Android se debe a que tiene un emulador multiplataforma que todos los usuarios (Windows, macOS y Linux) de este tutorial pueden utilizar para ejecutar la aplicación nativa.

## Proyecto de GitHub

El código fuente y los paquetes de contenido están disponibles en la [AEM Guías de - Proyecto WKND Mobile GitHub](https://github.com/adobe/aem-guides-wknd-mobile).

Si encuentra algún problema con el tutorial o el código, deje un [Problema de GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM Servicios de contenido de GraphQL AEM vs.

|  | AEM API de GraphQL | AEM Servicios de contenido |
|--------------------------------|:-----------------|:---------------------|
| Definición del esquema | Modelos de fragmento de contenido estructurado | AEM Componentes de |
| Contenido | Fragmentos de contenido | AEM Componentes de |
| Detección de contenido | Por consulta de GraphQL | AEM Por página de |
| Formato de envío | GRAPHQL JSON | AEM ComponenteExportadorJSON |
