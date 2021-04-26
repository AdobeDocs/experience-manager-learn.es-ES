---
title: Plantillas de página
seo-title: 'Introducción a AEM Sites: plantillas de página'
description: Obtenga información sobre cómo crear y modificar plantillas de página. Comprenda la relación entre una plantilla de página y una página. Obtenga información sobre cómo configurar las políticas de una plantilla de página para proporcionar control granular y coherencia de marca para el contenido.  Se creará una plantilla de artículo de revista bien estructurada basada en una maqueta de Adobe XD.
sub-product: sitios
version: Cloud Service
type: Tutorial
topic: Administración de contenido
feature: Componentes principales, plantillas editables, editor de páginas
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 3%

---


# Plantillas de página {#page-templates}

>[!CAUTION]
>
> Las funciones de creación rápida de sitios que se muestran aquí se lanzarán en el segundo semestre de 2021. La documentación relacionada está disponible con fines de vista previa.

En este capítulo analizaremos la relación entre una plantilla de página y una página. Crearemos una plantilla de artículo de revista sin estilo basada en algunas maquetas de [AdobeXD](https://www.adobe.com/products/xd.html). En el proceso de creación de la plantilla, se tratan los componentes principales y las configuraciones de políticas avanzadas.

## Requisitos previos {#prerequisites}

Este es un tutorial en varias partes y se da por hecho que los pasos descritos en el capítulo [Creación de contenido y publicación de cambios](./author-content-publish.md) se han completado.

## Objetivo

1. Inspect es un diseño de página creado en Adobe XD y asignado a componentes principales.
1. Comprenda los detalles de las plantillas de página y cómo se pueden usar las políticas para aplicar el control granular del contenido de la página.
1. Descubra cómo se vinculan las plantillas y las páginas.

## Qué va a generar {#what-you-will-build}

En esta parte del tutorial, creará una nueva plantilla Página de artículos de revistas que se puede utilizar para crear nuevos artículos de revistas y se ajusta a una estructura común. La plantilla se basará en diseños y en un kit de interfaz de usuario creado en Adobe XD. Este capítulo se centra únicamente en la construcción de la estructura o el esqueleto de la plantilla. No se implementará ningún estilo, pero la plantilla y las páginas funcionarán.

## Planificación de IU con Adobe XD {#adobexd}

En la mayoría de los casos, la planificación de un nuevo sitio web comienza con maquetas y diseños estáticos. [Adobe ](https://www.adobe.com/products/xd.html) XD es una herramienta de diseño que crea experiencias de usuario. A continuación, analizaremos un kit de IU y maquetas para ayudar a planificar la estructura de la plantilla de página de artículos.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Descargue el archivo de diseño de artículos  [WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> También hay disponible un [AEM Kit de interfaz de usuario de componentes principales genérico](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) como punto de partida para proyectos personalizados.

## Crear la plantilla de página de artículo de la revista

Al crear una página, debe seleccionar una plantilla, que se utilizará como base para crear la página nueva. La plantilla define la estructura de la página resultante, el contenido inicial y los componentes permitidos.

Existen tres áreas principales de [Plantillas de página](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **Estructura** : define los componentes que forman parte de la plantilla. Los autores de contenido no podrán editarlos.
1. **Contenido inicial** : define los componentes con los que comenzará la plantilla, que los autores de contenido pueden editar o eliminar
1. **Políticas** : define las configuraciones sobre cómo se comportarán los componentes y las opciones que tendrán disponibles los autores.

A continuación, cree una nueva plantilla en AEM que coincida con la estructura de las maquetas. Esto ocurre en una instancia local de AEM. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### Paquete de soluciones

Se puede descargar e instalar una solución [de la plantilla de revista](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip) terminada mediante el Administrador de paquetes.

## Actualizar el encabezado y el pie de página con fragmentos de experiencias {#experience-fragments}

Una práctica habitual al crear contenido global, como un encabezado o pie de página, es utilizar un [fragmento de experiencia](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragmentos de experiencia, permite a los usuarios combinar varios componentes para crear un único componente que se pueda referenciar. Los fragmentos de experiencias tienen la ventaja de admitir la administración de varios sitios y la [localización](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

La plantilla Sitio generó un encabezado y un pie de página. A continuación, actualice los fragmentos de experiencias para que coincidan con las maquetas. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Pasos de alto nivel para el siguiente vídeo:

1. Descargue el paquete de contenido de ejemplo **[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**.
1. Cargue e instale el paquete de contenido mediante el Administrador de paquetes.
1. Actualice los fragmentos de experiencia de encabezado y pie de página para utilizar el logotipo de WKND

## Creación de una página de artículos de una revista

A continuación, cree una nueva página con la plantilla Página de artículos de la revista . Cree el contenido de la página para que coincida con las maquetas del sitio. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## Felicitaciones! {#congratulations}

Felicidades, acaba de crear una nueva plantilla y página con Adobe Experience Manager Sites.

### Pasos siguientes {#next-steps}

En este punto, la página de artículos de la revista y el sitio no coinciden con los estilos de marca de WKND. Siga el tutorial [Theming](theming.md) para conocer las prácticas recomendadas para actualizar el código de front-end de CSS y Javascript utilizado para aplicar estilos globales al sitio.

### Paquete de soluciones

Hay disponible un paquete de soluciones para este capítulo para descargar: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
