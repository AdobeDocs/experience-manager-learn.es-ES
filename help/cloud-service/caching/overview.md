---
title: Almacenamiento en caché de AEM as a Cloud Service
description: Información general de almacenamiento en caché de AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '206'
ht-degree: 100%

---

# Almacenamiento en caché de AEM as a Cloud Service

En AEM as a Cloud Service, es crucial comprender el almacenamiento en caché. El almacenamiento en caché implica almacenar y reutilizar los datos recuperados anteriormente para mejorar la eficacia del sistema y reducir los tiempos de carga. Este mecanismo acelera significativamente la entrega de contenido, aumenta el rendimiento del sitio web y optimiza la experiencia del usuario.

AEM as a Cloud Service tiene varias capas de almacenamiento en caché y estrategias que difieren entre los servicios de creación y publicación.

![Información general de almacenamiento en caché de AEM as a Cloud Service](./assets/overview/all.png){align="center"}

## Almacenamiento en caché de AEM

AEM as a Cloud Service tiene una estrategia de almacenamiento en caché de varias capas sólida y configurable, que incluye una CDN, AEM Dispatcher y, opcionalmente, una CDN administrada por el cliente. El almacenamiento en caché entre capas se puede ajustar para optimizar el rendimiento, lo que garantiza que AEM solo ofrezca las mejores experiencias. AEM tiene diferentes problemas de almacenamiento en caché para los servicios de creación y publicación. Explore las estrategias de almacenamiento en caché para cada servicio a continuación.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="Servicio de AEM Publish" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Almacenamiento en caché del servicio AEM Publish">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Almacenamiento en caché del servicio AEM Publish">Almacenamiento en caché del servicio AEM Publish</a></p>
            <p class="is-size-6">El servicio de publicación de AEM utiliza una CDN administrada y AEM Dispatcher para optimizar las experiencias web de los usuarios finales.</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="Almacenamiento en caché del servicio AEM Author" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Almacenamiento en caché del servicio AEM Author">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Almacenamiento en caché del servicio AEM Author">Almacenamiento en caché del servicio AEM Author</a></p>
                <p class="is-size-6">El servicio de AEM Author utiliza una CDN administrada para ofrecer experiencias de creación optimizadas.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
