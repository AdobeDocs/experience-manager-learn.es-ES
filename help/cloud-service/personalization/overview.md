---
title: Información general de personalización
description: Obtenga información sobre cómo personalizar sitios web de AEM as a Cloud Service mediante aplicaciones de Adobe Target y Adobe Experience Platform.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
exl-id: c4fb11b9-b613-4522-b9da-18d7ae0826ec
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 7%

---

# Información general de personalización

Descubra cómo AEM as a Cloud Service (AEM CS) se integra con Adobe Target y Adobe Experience Platform (AEP) para ofrecer experiencias personalizadas. Mediante fragmentos de experiencias como contenido personalizado, descubra cómo ejecutar pruebas A/B, segmentar usuarios en función del comportamiento en tiempo real o personalizar contenido mediante perfiles de cliente unificados creados a partir de datos de varios sistemas.

## Requisitos previos

Para mostrar varios escenarios de personalización, este tutorial utiliza el proyecto de muestra [AEM WKND](https://github.com/adobe/aem-guides-wknd/). Para continuar, necesita lo siguiente:

- Una organización de Adobe con acceso a:
   - **Entorno de AEM as a Cloud Service**: para crear y administrar contenido
   - **Adobe Target**: para componer y entregar experiencias personalizadas
   - **aplicaciones de Adobe Experience Platform**: para administrar perfiles y audiencias de clientes
   - **Etiquetas (anteriormente Launch) en AEP**: para implementar Web SDK y JavaScript personalizado para la recopilación y personalización de datos

- Una comprensión básica de los componentes de AEM y los fragmentos de experiencias

- El proyecto [AEM WKND](https://github.com/adobe/aem-guides-wknd/) se implementó en su entorno AEM as a Cloud Service.

## Demostración en directo de casos de uso de Personalization

Experimente la personalización en acción en el [sitio web de habilitación de WKND](https://wknd.enablementadobe.com/us/en.html){target="wknd"}. El sitio de demostración muestra tres tipos de personalización: pruebas A/B, segmentación basada en el comportamiento y personalización de usuarios conocidos.

>[!TIP]
>
> Explorar la demostración en directo primero le ayuda a comprender el valor y las capacidades de cada técnica de personalización antes de invertir tiempo en la configuración y la implementación.

<!-- CARDS
{target = _self}

* ./live-demo.md
  {title = Live Demo of Personalization Use Cases}
  {description = Experience personalization in action on the [WKND Enablement website](https://wknd.enablementadobe.com/us/en.html). The demo site demonstrates three types of personalization: A/B testing, behavioral targeting, and known-user personalization.}
  {image = ./assets/live-demo/live-demo.png}
  {cta = Live Demo}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Live Demo of Personalization Use Cases">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./live-demo.md" title="Demostración en directo de casos de uso de Personalization" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/live-demo/live-demo.png" alt="Demostración en directo de casos de uso de Personalization"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./live-demo.md" target="_self" rel="referrer" title="Demostración en directo de casos de uso de Personalization">Demostración en vivo de casos de uso de Personalization</a>
                    </p>
                    <p class="is-size-6">Personalización de experiencias en acción en el sitio web de habilitación de WKND. El sitio de demostración muestra tres tipos de personalización: pruebas A/B, segmentación basada en el comportamiento y personalización de usuarios conocidos.</p>
                </div>
                <a href="./live-demo.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Demostración en vivo</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Introducción

Antes de explorar casos de uso específicos, primero debe configurar AEM as a Cloud Service para la personalización. Comience por integrar Adobe Target y Etiquetas para habilitar la personalización del lado del cliente mediante Web SDK. Estos pasos básicos permiten que sus páginas de AEM admitan la experimentación, la segmentación de audiencias y la personalización en tiempo real.

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content, such as Experience Fragments, as offers.}
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
                    <p class="is-size-6">Integre AEM CS con Adobe Target para activar contenido personalizado, como fragmentos de experiencias, como ofertas.</p>
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
    {description = Learn how to test different content variations on an AEMCS website using Adobe Target for A/B testing.}
    {image = ./assets/use-cases/experiment/experimentation.png}
    {cta = Learn Experimentation}

* ./use-cases/behavioral-targeting.md
    {title = Behavioral Targeting}
    {description = Learn how to personalize content based on user behavior using Adobe Experience Platform and Adobe Target.}
    {image = ./assets/use-cases/behavioral-targeting/behavioral-targeting.png}
    {cta = Learn Behavioral Targeting}

* ./use-cases/known-user-personalization.md
    {title = Known-user personalization}
    {description = Learn how to personalize content based on known user data by stitching information from multiple systems into a complete customer profile.}
    {image = ./assets/use-cases/known-user-personalization/known-user-personalization.png}
    {cta = Learn Known-user personalization}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="Experimentación (Pruebas A/B)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="Experimentación (Pruebas A/B)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="Experimentación (Pruebas A/B)">Experimentación (prueba A/B)</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo probar diferentes variaciones de contenido en un sitio web de AEM CS mediante Adobe Target para pruebas A/B.</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimentación de aprendizaje</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Behavioral Targeting">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/behavioral-targeting.md" title="Direccionamiento de comportamiento" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/behavioral-targeting/behavioral-targeting.png" alt="Direccionamiento de comportamiento"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" title="Direccionamiento de comportamiento">Segmentación basada en el comportamiento</a>
                    </p>
                    <p class="is-size-6">Aprenda a personalizar el contenido en función del comportamiento del usuario mediante Adobe Experience Platform y Adobe Target.</p>
                </div>
                <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender a segmentar según el comportamiento</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Known-user personalization">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/known-user-personalization.md" title="Personalización de usuario conocido" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/known-user-personalization/known-user-personalization.png" alt="Personalización de usuario conocido"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/known-user-personalization.md" target="_self" rel="referrer" title="Personalización de usuario conocido">Personalización de usuario conocido</a>
                    </p>
                    <p class="is-size-6">Aprenda a personalizar el contenido en función de datos de usuario conocidos vinculando información de varios sistemas a un perfil de cliente completo.</p>
                </div>
                <a href="./use-cases/known-user-personalization.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprenda a personalizar usuarios conocidos</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
