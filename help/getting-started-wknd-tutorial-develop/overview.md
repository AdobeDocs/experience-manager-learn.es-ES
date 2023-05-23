---
title: 'Introducción a AEM Sites: tutorial de WKND'
description: AEM Obtenga información sobre cómo implementar un sitio de para una marca ficticia de estilo de vida llamada WKND. Obtenga información detallada sobre temas fundamentales para el Experience Manager, como la configuración de proyectos, los tipos de archivo Maven, los componentes principales, las plantillas editables, las bibliotecas de cliente y el desarrollo de componentes.
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---

# Introducción a AEM Sites: tutorial de WKND {#introduction}

Bienvenido a un tutorial de varias partes diseñado para desarrolladores nuevos en Adobe Experience Manager AEM (). AEM Este tutorial explora la implementación de un sitio de trabajo para una marca ficticia de estilo de vida, WKND. El tutorial abarca temas fundamentales como la configuración del proyecto, los componentes principales, las plantillas editables, las bibliotecas del lado del cliente y el desarrollo de componentes con Adobe Experience Manager Sites.

## Información general {#wknd-tutorial-overview}

El objetivo de este tutorial en varias partes es enseñar a un desarrollador a implementar un sitio web utilizando los últimos estándares y tecnologías en Adobe Experience Manager AEM (). AEM Después de completar este tutorial, un desarrollador debe comprender los fundamentos básicos de la plataforma y los patrones de diseño comunes en la creación de informes de diseño de la plataforma y de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la plataforma de diseño de la aplicación.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Opciones para iniciar un proyecto de Sites

Existen dos enfoques básicos para iniciar un proyecto de AEM Sites.

**AEM Tipo de archivo del proyecto** AEM AEM - Enfoque tradicional para el desarrollo de la mediante la generación de un proyecto mínimo de Maven utilizando una plantilla. AEM AEM Este es el enfoque recomendado para proyectos de la versión 6.5/6.4 de la y proyectos as a Cloud Service que anticipan una fuerte personalización. AEM El tutorial ofrece una profundización en el desarrollo de la.

[AEM Iniciar el tutorial con el tipo de archivo del proyecto de](./project-archetype/overview.md)

**AEM Plantillas de sitio de** AEM : también conocido como Creación rápida de sitios, un método de bajo código para generar un sitio de la red mediante el uso de una plantilla de sitio predefinida. Utilice componentes y plantillas listas para usar para poner en marcha rápidamente un sitio. Utilice un flujo de trabajo de temas para aplicar estilos y personalizaciones específicos de la marca solo con CSS y JavaScript. Recomendado para nuevos proyectos y desarrolladores. AEM Solo disponible para as a Cloud Service de la.

[Inicio del tutorial con una plantilla del sitio](./site-template/create-site.md)

## Kit de IU de Adobe XD

Para acercar este tutorial a los talentosos diseñadores de experiencia de usuario de un Adobe del mundo real, crearon las maquetas para el sitio utilizando [Adobe XD](https://www.adobe.com/products/xd.html). AEM A lo largo del tutorial, se implementan varias piezas de los diseños en un sitio de creación totalmente compatible con la creación de un sitio de creación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de diseño de la aplicación. Agradecimientos especiales a **Lorenzo Buosi** y **Kilian Amendola** que creó el hermoso diseño para el sitio WKND.

XD Descargue los kits de interfaz de usuario de:

* [AEM Kit de IU de componentes principales](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit de IU de WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Sitio de referencia {#reference-site}

También hay disponible una versión final del sitio WKND como referencia: [https://wknd.site/](https://wknd.site/)

AEM El tutorial abarca las principales habilidades de desarrollo necesarias para un desarrollador de la, pero *no* cree todo el sitio de principio a fin. El sitio de referencia terminado es otro buena AEM recurso para explorar y ver más de las capacidades listas para usar de la versión más reciente de la aplicación y de las que se pueden obtener más datos de forma predeterminada.

Para probar el código más reciente antes de saltar al tutorial, descargue e instale el **[última versión de GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Tecnología de Adobe Stock

Muchas de las imágenes del sitio web de referencia de WKND son de [Adobe Stock](https://stock.adobe.com/) y son materiales de terceros, tal como se definen en las condiciones adicionales del recurso de demostración en [https://www.adobe.com/legal/terms.html](https://www.adobe.com/es/legal/terms.html). Si desea utilizar una imagen de Adobe Stock para otros fines más allá de la visualización de este sitio web de demostración, como su inclusión en un sitio web o en materiales de marketing, puede adquirir una licencia de Adobe Stock.

Con Adobe Stock, tiene acceso a más de 140 millones de imágenes de alta calidad y libres de derechos de autor, incluidas fotos, gráficos, vídeos y plantillas, para impulsar sus proyectos creativos.

## Pasos siguientes {#next-steps}

¿Qué estás esperando?! Obtenga información sobre cómo [Generar un nuevo proyecto de Adobe Experience Manager AEM con el tipo de archivo del proyecto de](./project-archetype/overview.md) o [crear un sitio con una plantilla de sitio](./site-template/create-site.md).
