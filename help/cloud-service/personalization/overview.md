---
title: Información general de personalización
description: Obtenga información sobre cómo personalizar sitios web de AEM as a Cloud Service mediante aplicaciones de Adobe Target y Adobe Experience Platform.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 6%

---

# Información general de personalización

Descubra cómo AEM as a Cloud Service (AEMCS) se integra con Adobe Target y Adobe Experience Platform (AEP). Descubra cómo ofrecer experiencias personalizadas con pruebas A/B, segmentar usuarios según su comportamiento o personalizar contenido mediante perfiles de cliente.

## Requisitos previos

Para mostrar varios escenarios de personalización, este tutorial utiliza el proyecto de muestra [AEM WKND](https://github.com/adobe/aem-guides-wknd/). Para continuar, necesita lo siguiente:

- Una organización de Adobe con acceso a:
   - **Entorno de AEM as a Cloud Service**: para crear y administrar contenido
   - **Adobe Target**: para componer y entregar experiencias personalizadas
   - **aplicaciones de Adobe Experience Platform**: para administrar perfiles y audiencias de clientes
   - **Etiquetas (anteriormente Launch) en AEP**: para implementar Web SDK y JavaScript personalizado para la recopilación y personalización de datos

- Una comprensión básica de los componentes de AEM y los fragmentos de experiencias

- El proyecto [AEM WKND](https://github.com/adobe/aem-guides-wknd/) se implementó en su entorno AEM as a Cloud Service.

## Introducción

Antes de explorar casos de uso específicos, primero debe configurar AEM as a Cloud Service para la personalización. Comience por integrar Adobe Target y Etiquetas para habilitar la personalización del lado del cliente mediante AEP Web SDK. Estos pasos básicos permiten que sus páginas de AEM admitan la experimentación, la segmentación de audiencias y la personalización en tiempo real.

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content as Adobe Target offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="Integración con Adobe Target" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="Integración con Adobe Target"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="Integración con Adobe Target">Integrar Adobe Target</a>
                    </p>
                    <p class="is-size-6">Integre AEM CS con Adobe Target para activar contenido personalizado como ofertas de Adobe Target.</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Integrar Target</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="Integrar etiquetas" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="Integrar etiquetas"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="Integrar etiquetas">Integrar etiquetas</a>
                    </p>
                    <p class="is-size-6">Integre AEM CS con etiquetas para insertar el Web SDK y el JavaScript personalizado para la recopilación y personalización de datos.</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Integrar etiquetas</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## Casos de uso

Explore los siguientes casos de uso de personalización comunes compatibles con AEM CS, Adobe Target y Adobe Experience Platform.

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
  {title = Experimentation (A/B Testing)}
  {description = Learn how to test different content variations in AEMCS using Adobe Target for A/B testing.}
  {image = ./assets/use-cases/experiment/experimentation.png}
  {cta = Learn Experimentation}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="Experimentación (prueba A/B)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="Experimentación (prueba A/B)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="Experimentación (prueba A/B)">Experimentación (prueba A/B)</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo probar diferentes variaciones de contenido en AEM CS mediante Adobe Target para pruebas A/B.</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimentación de aprendizaje</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



















