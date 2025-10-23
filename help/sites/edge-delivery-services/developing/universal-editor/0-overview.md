---
title: Tutorial para desarrolladores con Edges Delivery Services y el editor universal
description: Conozca los conceptos básicos del desarrollo de un nuevo sitio web creado en el editor universal de AEM y suministrado con Edge Delivery Services.
short-description: Un tutorial para desarrolladores para crear sitios web de AEM que combinen la creación avanzada con el editor universal y una entrega rápida con Edge Delivery Services. Aprenda a crear un nuevo proyecto, configurar un entorno de desarrollo local y crear bloques personalizados.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Catalog
jira: KT-15832
duration: 88
exl-id: aeac08a2-75a0-4adb-b32e-0e7f85e7eb1d
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '658'
ht-degree: 94%

---

# Tutorial para desarrolladores con Edges Delivery Services y el editor universal

![Tutorial para desarrolladores con Edge Delivery Services y el editor universal](./assets/0-overview/hero.png)

En este tutorial, aprenderá los aspectos básicos de la creación de un sitio web de AEM que combine la creación avanzada con el editor universal y una distribución rápida con Edge Delivery Services. Al final, tendrá conocimientos básicos sobre cómo crear un nuevo proyecto, configurar un entorno de desarrollo local y crear un nuevo bloque.

## Configuración del proyecto

Obtenga información sobre cómo crear un proyecto de código y configurar un nuevo sitio en AEM as a Cloud Service. Esta configuración permite un desarrollo sin problemas con el editor universal para la creación de contenido y la distribución rápida de contenido de Edge Delivery Services.

<!-- CARDS 

* ./1-new-code-project.md
* ./2-new-aem-site.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a code project">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./1-new-code-project.md" title="Crear un proyecto de código" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/1-new-project/new-project.png" alt="Crear un proyecto de código"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./1-new-code-project.md" target="_blank" rel="referrer" title="Crear un proyecto de código">Crear un proyecto de código</a>
                    </p>
                    <p class="is-size-6">Cree un proyecto de código para Edge Delivery Services y edítelo con el editor universal.</p>
                </div>
                <a href="./1-new-code-project.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create an AEM site">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./2-new-aem-site.md" title="Crear un sitio de AEM" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/2-new-aem-site/new-site.png" alt="Crear un sitio de AEM"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./2-new-aem-site.md" target="_blank" rel="referrer" title="Crear un sitio de AEM">Crear un sitio de AEM</a>
                    </p>
                    <p class="is-size-6">Cree un sitio en AEM Sites para Edge Delivery Services editable con el Editor universal.</p>
                </div>
                <a href="./2-new-aem-site.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Configuración de desarrollo

Aprenda a configurar su entorno de desarrollo local para permitir un desarrollo rápido de sitios web. Esta configuración permite crear sitios sin problemas con el editor universal y suministrar contenido de forma eficaz a través de Edge Delivery Services, lo que garantiza un flujo de trabajo de desarrollo fluido y optimizado.
<!-- CARDS 

* ./3-local-development-environment.md
* ./4-website-branding.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up a local development environment">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./3-local-development-environment.md" title="Configuración de un entorno de desarrollo local" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3443978/?format=jpeg&nocache=1753811862249" alt="Configuración de un entorno de desarrollo local"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./3-local-development-environment.md" target="_blank" rel="referrer" title="Configuración de un entorno de desarrollo local">Configuración de un entorno de desarrollo de local</a>. 
                    </p>
                    <p class="is-size-6">Configure un entorno de desarrollo local para los sitios distribuidos con Edge Delivery Services y edítelo con el editor universal.</p>
                </div>
                <a href="./3-local-development-environment.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Add website branding">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./4-website-branding.md" title="Añadir promoción de la marca en el sitio web" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/4-website-branding/github-issues.png" alt="Añadir promoción de la marca en el sitio web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./4-website-branding.md" target="_blank" rel="referrer" title="Añadir promoción de la marca en el sitio web">Añadir personalización de la marca en el sitio web</a>
                    </p>
                    <p class="is-size-6">Defina las variables CSS globales y CSS, y las fuentes web para un sitio de Edge Delivery Services.</p>
                </div>
                <a href="./4-website-branding.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Bloquear desarrollo

Aprenda a crear un nuevo bloque definiendo su modelo de contenido y configurando contenido de muestra para pruebas y desarrollo. Explore dos métodos para procesar el bloque y conozca cómo estructurarlo para obtener un rendimiento y una flexibilidad óptimos en AEM y Edge Delivery Services.

<!-- CARDS 

* ./5-new-block.md {image = ./assets/5-new-block/card.png}
* ./6-author-block.md {image = ./assets/6-author-block/card.png}
* ./7a-block-css.md {image = ./assets/7a-block-css/card.png}
* ./7b-block-js-css.md {image = ./assets/7b-block-js-css/card.png}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./5-new-block.md" title="Crear un bloque" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/5-new-block/card.png" alt="Crear un bloque"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./5-new-block.md" target="_blank" rel="referrer" title="Crear un bloque">Crear un bloque</a>
                    </p>
                    <p class="is-size-6">Cree un bloque para un sitio web de Edge Delivery Services que se pueda editar con el editor universal.</p>
                </div>
                <a href="./5-new-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Author a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./6-author-block.md" title="Crear un bloque" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/6-author-block/card.png" alt="Crear un bloque"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./6-author-block.md" target="_blank" rel="referrer" title="Crear un bloque">Crear un bloque</a>
                    </p>
                    <p class="is-size-6">Cree un bloque de Edge Delivery Services con el editor universal.</p>
                </div>
                <a href="./6-author-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7a-block-css.md" title="Desarrollo de un bloque con CSS" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/7a-block-css/card.png" alt="Desarrollo de un bloque con CSS"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                         <a href="./7a-block-css.md" target="_blank" rel="referrer" title="Desarrollo de un bloque con CSS">Desarrollo de un bloque con CSS</a>
                    </p>
                    <p class="is-size-6">Desarrolle un bloque con CSS para Edge Delivery Services, editable con el editor universal.</p>
                </div>
                <a href="./7a-block-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS and JS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7b-block-js-css.md" title="Desarrollo de un bloque con CSS y JS" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/7b-block-js-css/card.png" alt="Desarrollo de un bloque con CSS y JS"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7b-block-js-css.md" target="_blank" rel="referrer" title="Desarrollo de un bloque con CSS y JS">Desarrollo de un bloque con CSS y JS</a>
                    </p>
                    <p class="is-size-6">Desarrolle un bloque con CSS y JavaScript para Edge Delivery Services y edítelo con el editor universal.</p>
                </div>
                <a href="./7b-block-js-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Siguientes pasos

Ahora que ha completado este tutorial, aproveche lo que ha aprendido con estas guías específicas. Estas guías amplían el código y los conceptos que se tratan aquí, explorando casos de uso específicos de cada función, técnicas avanzadas y sugerencias adicionales para mejorar las habilidades de desarrollo de Edge Delivery Services y el editor universal.

<!-- CARDS 

* ./how-to/block-options.md
* ./how-to/header-and-footer.md
* ./how-to/local-extension-preview.md {image = ./how-to/assets/local-extension-preview/extension-loaded.png}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Block options">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/block-options.md" title="Opciones de bloque" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="how-to/assets/block-options/main.png" alt="Opciones de bloque"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/block-options.md" target="_blank" rel="referrer" title="Opciones de bloque">Opciones de bloque</a>
                    </p>
                    <p class="is-size-6">Aprenda a crear un bloque con varias opciones de visualización.</p>
                </div>
                <a href="./how-to/block-options.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header and Footer">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/header-and-footer.md" title="Encabezado y pie de página" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="how-to/assets/header-and-footer/hero.png" alt="Encabezado y pie de página"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/header-and-footer.md" target="_blank" rel="referrer" title="Encabezado y pie de página">Encabezado y pie de página</a>
                    </p>
                    <p class="is-size-6">Descubra cómo se desarrollan los encabezados y pies de página en Edge Delivery Services y el editor universal.</p>
                </div>
                <a href="./how-to/header-and-footer.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Preview a Universal Editor extension">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/local-extension-preview.md" title="Previsualización de una extensión de editor universal" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./how-to/assets/local-extension-preview/extension-loaded.png" alt="Previsualización de una extensión de editor universal"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/local-extension-preview.md" target="_blank" rel="referrer" title="Previsualización de una extensión de editor universal">Vista previa de una extensión de editor universal</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo previsualizar una extensión de editor universal que se ejecuta localmente durante el desarrollo.</p>
                </div>
                <a href="./how-to/local-extension-preview.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
