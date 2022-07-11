---
title: 'Introducción a AEM Sites: tutorial de WKND'
description: Aprenda a implementar un sitio AEM para una marca ficticia de estilo de vida llamada WKND. Obtenga información detallada sobre los temas fundamentales del Experience Manager, como la configuración del proyecto, los arquetipos de maven, los componentes principales, las plantillas editables, las bibliotecas de cliente y el desarrollo de componentes.
sub-product: sites
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
source-git-commit: 72abe1cddcf6a012403887203d38509bde8f2d23
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 3%

---

# Introducción a AEM Sites: tutorial de WKND {#introduction}

Le damos la bienvenida a un tutorial de varias partes diseñado para desarrolladores que utilicen Adobe Experience Manager (AEM) por primera vez. Este tutorial recorre la implementación de un sitio AEM para una marca ficticia de estilo de vida, la WKND. El tutorial trata temas fundamentales como la configuración del proyecto, los componentes principales, las plantillas editables, las bibliotecas del lado del cliente y el desarrollo de componentes con Adobe Experience Manager Sites.

## Información general {#wknd-tutorial-overview}

El objetivo de este tutorial en varias partes es enseñar a los desarrolladores a implementar un sitio web utilizando los estándares y tecnologías más recientes en Adobe Experience Manager (AEM). Después de completar este tutorial, un desarrollador debe comprender la base básica de la plataforma y los patrones de diseño comunes en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Opciones para iniciar un proyecto de Sites

Existen dos enfoques básicos para iniciar un proyecto de AEM Sites.

**Tipo de archivo del proyecto AEM** - Enfoque tradicional para el desarrollo de AEM mediante la generación de un proyecto de AEM mínimo utilizando una plantilla Maven. Este es el enfoque recomendado para AEM proyectos de la versión 6.5/6.4 y AEM proyectos as a Cloud Service que prevén una gran personalización. El tutorial ofrece una profundización en AEM desarrollo.

[Inicie el tutorial con el tipo de archivo del proyecto AEM](./project-archetype/overview.md)

**Plantillas AEM sitio** - También conocido como Creación rápida del sitio, un enfoque de código bajo para generar un sitio AEM usando una plantilla de sitio predefinida. Utilice los componentes y las plantillas listas para usar para poner en marcha un sitio rápidamente. Utilice un flujo de trabajo temático para aplicar estilos y personalizaciones específicos de la marca solo con CSS y JavaScript. Recomendado para nuevos proyectos y desarrolladores. Solo disponible para AEM as a Cloud Service.

[Iniciar el tutorial con una plantilla de sitio](./site-template/create-site.md)

## Kit de interfaz de usuario de Adobe XD

Para hacer este tutorial más cercano a un escenario real, los talentosos diseñadores de experiencia de usuario crearon las maquetas para el sitio utilizando [Adobe XD](https://www.adobe.com/products/xd.html). A lo largo del tutorial, varias partes de los diseños se implementan en un sitio de AEM totalmente creativo. Gracias especiales **Lorenzo Buosi** y **Kilian Amendola** que creó el hermoso diseño para el sitio WKND.

Descargue los kits de interfaz de usuario de XD:

* [Kit de interfaz de usuario de los componentes principales de AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit de IU WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Sitio de referencia {#reference-site}

También está disponible como referencia una versión terminada del sitio WKND: [https://wknd.site/](https://wknd.site/)

El tutorial cubre las principales habilidades de desarrollo necesarias para un desarrollador AEM, pero *not* cree todo el sitio de principio a fin. El sitio de referencia terminado es otro recurso bueno para explorar y ver más AEM funcionalidades listas para usar.

Para probar el código más reciente antes de pasar al tutorial, descargue e instale el **[última versión de GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Con la tecnología de Adobe Stock

Muchas de las imágenes del sitio web de referencia WKND son de [Adobe Stock](https://stock.adobe.com/) y son Material de terceros tal como se define en los Términos adicionales de Demo Asset en [https://www.adobe.com/legal/terms.html](https://www.adobe.com/es/legal/terms.html). Si desea utilizar una imagen de Adobe Stock para otros fines más allá de ver este sitio web de demostración, como incluirla en un sitio web o en materiales de marketing, puede adquirir una licencia en Adobe Stock.

Con Adobe Stock, usted tiene acceso a más de 140 millones de imágenes de alta calidad, libres de derechos de autor, incluyendo fotos, gráficos, videos y plantillas para iniciar sus proyectos creativos.

## Siguientes pasos {#next-steps}

¡¿Qué estás esperando?! Obtenga información sobre cómo [genere un nuevo proyecto de Adobe Experience Manager con el tipo de archivo AEM proyecto](./project-archetype/overview.md) o [crear un sitio con una plantilla de sitio](./site-template/create-site.md).
