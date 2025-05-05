---
title: Vídeos y tutoriales de AEM Sites
description: Adobe Experience Manager (AEM) Sites es una plataforma de administración de experiencias líder. Esta guía del usuario contiene vídeos y tutoriales sobre las numerosas funciones y funcionalidades de AEM Sites.
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 5bd752ec257763e6d5da99c3da4b44fa9a43fd25
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 5%

---

# Vídeos y tutoriales de AEM Sites {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites es una plataforma de administración de experiencias líder. Esta guía del usuario contiene vídeos y tutoriales sobre las numerosas funciones y funcionalidades de AEM Sites.

## Tres formas de compilar con AEM Sites

AEM Sites ofrece tres formas de crear y distribuir experiencias. Tanto si desea crear páginas completas, optimizar el rendimiento de Edge como impulsar aplicaciones sin encabezado, AEM Sites ofrece opciones flexibles para satisfacer las necesidades de su proyecto:

1. **Los sitios tradicionales** utilizan el Editor de páginas del autor de AEM para crear contenido, que luego se activa y se entrega a los usuarios finales mediante las páginas web de AEM Publish as HTML.
1. Los sitios web de **Edge Delivery Services** aprovechan la creación basada en documentos o el Editor universal de Adobe para crear contenido, que luego se activa y luego se entrega a los usuarios finales mediante páginas web de Edge Delivery Services as a HTML.
1. Las experiencias web **sin encabezado/API-first** utilizan el Editor de fragmentos de contenido o el Editor universal para crear contenido, que luego se activa y se entrega mediante la publicación de AEM como JSON.

Estas opciones están diseñadas para satisfacer las diversas necesidades de las organizaciones de marketing, para ofrecer experiencias personalizadas y atractivas a alta velocidad y a escala en cualquier canal o dispositivo.

El diagrama siguiente ilustra las diferentes rutas:

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### Comparar las formas de generar con AEM Sites

La siguiente tabla proporciona una comparación de alto nivel de las tres rutas. Se centra en la creación de contenido y en los matices de entrega de experiencias de cada ruta.

|            | Sitios tradicionales | Edge Delivery Services | Sin encabezado/API-primero |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **Herramientas de creación** | Editor de página | Creación basada en documentos, editor universal | Fragmentos de contenido, editor universal |
| **Almacén de contenido creado** | AEM Author (JCR) | Documentos para el autor de AEM (JCR) | AEM Author (JCR) |
| **Envío** | Publicación de AEM (con Adobe CDN + Dispatcher) | Edge Delivery Services | Publicación de AEM (con Adobe CDN + Dispatcher) |
| **Almacén de contenido de entrega** | Publicación de AEM (JCR) | Edge Delivery Services | Publicación de AEM (JCR) |
| **Formato de envío** | HTML | HTML | JSON |
| **Tecnología de desarrollo** | Java™, JavaScript, CSS | JavaScript, CSS | Cualquiera (por ejemplo, Swift, React, etc.) |

## Tutoriales

Obtenga información sobre cada una de las tres rutas para compilar con AEM Sites a través de los siguientes tutoriales:

<!-- CARDS

* https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional Sites - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with EDS.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional Sites - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="Sitios tradicionales: tutorial de WKND" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="Sitios tradicionales: tutorial de WKND"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="Sitios tradicionales: tutorial de WKND">Sitios tradicionales - Tutorial de WKND</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo crear un proyecto de AEM Sites de muestra mediante el tutorial de WKND. Esta guía explica la configuración del proyecto, los componentes principales, las plantillas editables, las bibliotecas del lado del cliente y el desarrollo de componentes.</p>
                </div>
                <a href="https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services - Guías" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services - Guías"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - Guías">Edge Delivery Services - Guías</a>
                    </p>
                    <p class="is-size-6">Explore Edge Delivery Services con guías completas. Las guías de compilación, publicación y lanzamiento cubren todo lo necesario para comenzar a utilizar EDS.</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Sin encabezado/API-First: tutoriales" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Sin encabezado/API-First: tutoriales"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Sin encabezado/API-First: tutoriales">Sin encabezado/API-First: tutoriales</a>
                    </p>
                    <p class="is-size-6">Aprenda a crear aplicaciones sin encabezado impulsadas por contenido de AEM. Los tutoriales cubren marcos de trabajo como iOS, Android™ y React, y permiten elegir lo que se adapta a su pila.</p>
                </div>
                <a href="https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Recursos adicionales

* [Documentación de creación de AEM Sites](https://experienceleague.adobe.com/es/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [Documentación de desarrollo de AEM Sites](https://experienceleague.adobe.com/es/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [Documentación de administración de AEM Sites](https://experienceleague.adobe.com/es/docs/experience-manager-65/content/sites/administering/home)
* [Documentación de implementación de AEM Sites](https://experienceleague.adobe.com/es/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [Tutoriales de AEM as a Cloud Service](/help/cloud-service/overview.md)
* [Tutoriales para AEM Assets](/help/assets/overview.md)
* [Tutoriales de AEM Forms](/help/forms/overview.md)
* [Tutoriales de AEM Foundation](/help/foundation/overview.md)
