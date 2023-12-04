---
title: Plantillas de página
description: Aprenda a crear y modificar plantillas de página. Comprenda la relación entre una plantilla de página y una página. Aprenda a configurar las políticas de una plantilla de página para proporcionar un control granular y coherencia de marca para el contenido.  Se crea una plantilla de artículo de revista bien estructurada basada en una maqueta de Adobe XD.
version: Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1601
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 0%

---

# Plantillas de página {#page-templates}

En este capítulo exploraremos la relación entre una Plantilla de página y una Página. Crearemos una plantilla de artículo de revista sin estilo basada en algunas maquetas de [AdobeXD](https://www.adobe.com/products/xd.html). En el proceso de creación de la plantilla, se tratan los componentes principales y las configuraciones de directiva avanzadas.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que los pasos descritos en la sección [Creación de contenido y cambios de publicación](./author-content-publish.md) se han completado los capítulos.

## Objetivo

1. Conozca los detalles de las plantillas de página y cómo se pueden utilizar las políticas para aplicar un control granular del contenido de la página.
1. Descubra cómo se vinculan las plantillas y las páginas.
1. Cree una nueva plantilla y una página.

## Qué va a generar {#what-you-will-build}

En esta parte del tutorial, creará una nueva plantilla Página de artículo de revista que se puede utilizar para crear nuevos artículos de revista y se ajuste a una estructura común. La plantilla se basa en diseños y un kit de interfaz de usuario producido en AdobeXD. Este capítulo solo se centra en crear la estructura o el esqueleto de la plantilla. No se han implementado estilos, pero la plantilla y las páginas funcionan.

## Crear la plantilla de página de artículo de revista

Al crear una página, debe seleccionar una plantilla, que se utiliza como base para crear la nueva página. La plantilla define la estructura de la página resultante, el contenido inicial y los componentes permitidos.

Hay 3 áreas principales de [Plantillas de página](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=es):

1. **Estructura** : define los componentes que forman parte de la plantilla. Los autores de contenido no pueden editarlos.
1. **Contenido inicial** : define los componentes con los que comienza la plantilla, que los autores de contenido pueden editar o eliminar
1. **Políticas** : define configuraciones sobre cómo se comportan los componentes y qué opciones tendrán disponibles los autores.

AEM A continuación, cree una nueva plantilla en la que se adapte a la estructura de las maquetas. AEM Esto ocurrirá en una instancia local de. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

Puede utilizar la siguiente miniatura para identificar su plantilla (o cargar la suya propia).

![Miniatura de plantilla de página de artículo](./assets/page-templates/article-page-template-thumbnail.png)


### Paquete de soluciones

A terminado [solución de la plantilla de revista](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) se puede descargar e instalar mediante el Administrador de paquetes.

## Actualizar el encabezado y el pie de página con fragmentos de experiencias {#experience-fragments}

Una práctica habitual al crear contenido global, como un encabezado o pie de página, es utilizar un [Fragmento de experiencia](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Los fragmentos de experiencias permiten a los usuarios combinar varios componentes para crear un único componente referenciable. Los fragmentos de experiencias tienen la ventaja de admitir la administración de varios sitios y [localización](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

La plantilla del sitio generó un encabezado y un pie de página. A continuación, actualice los fragmentos de experiencias para que coincidan con las maquetas. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

Pasos de alto nivel para el siguiente vídeo:

1. Descargar el paquete de contenido de muestra **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Cargue e instale el paquete de contenido mediante el Administrador de paquetes.
1. Actualizar los fragmentos de experiencias de encabezado y pie de página para utilizar el logotipo de WKND

## Crear una página de artículo de revista

A continuación, cree una nueva página con la plantilla Página de artículo de la revista. Cree el contenido de la página para que coincida con las maquetas del sitio. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

Utilice el [texto proporcionado](./assets/page-templates/la-skateparks-copy.txt) para rellenar el cuerpo del artículo.

## Enhorabuena. {#congratulations}

Acaba de crear una plantilla y una página nuevas con Adobe Experience Manager Sites.

### Pasos siguientes {#next-steps}

En este punto, la página del artículo de la revista y el sitio no coinciden con los estilos de marca de WKND. Siga las [Temática](theming.md) tutorial para conocer las prácticas recomendadas para actualizar el código de front-end de CSS y Javascript utilizado para aplicar estilos globales al sitio.

### Paquete de soluciones

Hay disponible un paquete de soluciones para este capítulo para descargar: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
