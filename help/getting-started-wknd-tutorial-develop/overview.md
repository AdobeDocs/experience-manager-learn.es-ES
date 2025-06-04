---
title: 'Introducción a AEM Sites: Tutorial de WKND'
description: Aprenda a implementar un sitio de AEM para una marca ficticia de estilo de vida, WKND. Obtenga información detallada sobre temas fundamentales de Experience Manager, como la configuración de proyectos, los arquetipos de Maven, los componentes principales, las plantillas editables, las bibliotecas de cliente y el desarrollo de componentes.
version: Experience Manager as a Cloud Service
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
doc-type: Catalog
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '577'
ht-degree: 100%

---

# Introducción a AEM Sites: Tutorial de WKND {#introduction}

{{edge-delivery-services}}

Bienvenido al tutorial de varias partes diseñado para desarrolladores que se inician en Adobe Experience Manager (AEM).  Este tutorial recorre la implementación de un sitio de AEM para una marca ficticia de estilo de vida: WKND. El tutorial abarca temas fundamentales como la configuración del proyecto, los componentes principales, las plantillas editables, las bibliotecas del lado del cliente y el desarrollo de componentes con Adobe Experience Manager Sites.

## Información general {#wknd-tutorial-overview}

El objetivo de este tutorial en varias partes es enseñar a los desarrolladores a implementar un sitio web utilizando los últimos estándares y tecnologías de Adobe Experience Manager (AEM).  Después de completar este tutorial, los desarrolladores deberán comprender los fundamentos básicos de la plataforma y los patrones de diseño habituales de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Opciones para iniciar un proyecto de Sites

Existen dos enfoques básicos para iniciar un proyecto de AEM Sites.

**Arquetipo de proyecto de AEM**: enfoque tradicional para el desarrollo de AEM mediante la generación de un proyecto de AEM básico utilizando una plantilla de Maven. Este es el enfoque recomendado para los proyectos de AEM 6.5/6.4 y los proyectos de AEM as a Cloud Service que anticipan una gran personalización. El tutorial ofrece una profundización en el desarrollo de AEM.

[Inicie el tutorial con el arquetipo de proyecto de AEM](./project-archetype/overview.md)

**Plantillas de sitios de AEM**: también se conoce como Creación rápida de sitios y se trata de un enfoque de código bajo para generar un sitio de AEM mediante una plantilla de sitio predefinida. Utilice componentes y plantillas listos para usar para poner en marcha rápidamente un sitio. Utilice un flujo de trabajo de temas para aplicar estilos y personalizaciones específicos de la marca solo con CSS y JavaScript. Recomendado para nuevos proyectos y desarrolladores. Solo disponible para AEM as a Cloud Service.

[Inicie el tutorial con una plantilla de sitio](./site-template/create-site.md)

## Kit de IU de Adobe XD

Para que este tutorial se acerque más a un escenario real, los talentosos diseñadores de UX de Adobe han creado las maquetas del sitio utilizando [Adobe XD](https://www.adobe.com/products/xd.html). En el transcurso del tutorial se implementan varias piezas de los diseños en un sitio de AEM totalmente autorizable. Queremos dar las gracias especialmente a **Lorenzo Buosi** y **Kilian Amendola** por haber creado el hermoso diseño del sitio de WKND.

Descargue los kits de IU de XD:

* [Kit de IU de componente principal de AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit de IU de WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Sitio de referencia {#reference-site}

También hay disponible una versión final del sitio de WKND como referencia: [https://wknd.site/](https://wknd.site/)

El tutorial abarca las principales habilidades de desarrollo necesarias para un desarrollador de AEM, pero *no* se creará todo el sitio de principio a fin. El sitio de referencia finalizado es otro gran recurso para explorar y conocer mejor las capacidades de AEM listas para usar.

Para probar el código más reciente antes de saltar al tutorial, descargue e instale la **[última versión de GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Tecnología de Adobe Stock

Muchas de las imágenes del sitio web de referencia de WKND son de [Adobe Stock](https://stock.adobe.com/) y son materiales de terceros, tal como se definen en las condiciones adicionales del recurso de demostración en [https://www.adobe.com/legal/terms.html](https://www.adobe.com/es/legal/terms.html). Si desea utilizar una imagen de Adobe Stock para otros fines más allá de la visualización de este sitio web de demostración, como su inclusión en un sitio web o en materiales de marketing, puede adquirir una licencia de Adobe Stock.

Con Adobe Stock, tiene acceso a más de 140 millones de imágenes de alta calidad y libres de derechos de autor, incluidas fotos, gráficos, vídeos y plantillas, para impulsar sus proyectos creativos.

## Siguientes pasos {#next-steps}

¿A qué espera? Obtenga información sobre cómo [generar un nuevo proyecto de Adobe Experience Manager con el arquetipo de proyecto de AEM](./project-archetype/overview.md) o [cree un sitio con una plantilla de sitio](./site-template/create-site.md).
