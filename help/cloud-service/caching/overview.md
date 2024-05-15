---
title: AEM almacenamiento en caché as a Cloud Service
description: AEM Información general sobre el almacenamiento en caché as a Cloud Service de.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# AEM almacenamiento en caché as a Cloud Service

AEM En as a Cloud Service, comprender el almacenamiento en caché es crucial. El almacenamiento en caché implica almacenar y reutilizar los datos recuperados anteriormente para mejorar la eficacia del sistema y reducir los tiempos de carga. Este mecanismo acelera significativamente la entrega de contenido, aumenta el rendimiento del sitio web y optimiza la experiencia del usuario.

AEM as a Cloud Service tiene varias capas de almacenamiento en caché y estrategias que difieren entre los servicios de creación y publicación.

![AEM Resumen del almacenamiento en caché as a Cloud Service](./assets/overview/all.png){align="center"}

## AEM almacenamiento en caché de

AEM as a Cloud Service AEM cuenta con una estrategia de almacenamiento en caché de varias capas sólida y configurable, que incluye una CDN, un Dispatcher y, opcionalmente, una CDN administrada por el cliente. AEM El almacenamiento en caché en todas las capas se puede ajustar para optimizar el rendimiento, lo que garantiza que solo ofrezca las mejores experiencias. AEM Tiene diferentes problemas de almacenamiento en caché para los servicios de autor y publicación. Explore las estrategias de almacenamiento en caché para cada servicio a continuación.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Servicio de publicación de" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM Almacenamiento en caché del servicio de publicación">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM Almacenamiento en caché del servicio de publicación">AEM Almacenamiento en caché del servicio de publicación</a></p>
            <p class="is-size-6">AEM AEM El servicio de publicación de datos utiliza una CDN administrada y Dispatcher de para optimizar las experiencias web de los usuarios finales.</p>
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
                <a href="./author.md" title="AEM Almacenamiento en caché del servicio de autor" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEM Almacenamiento en caché del servicio de autor">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEM Almacenamiento en caché del servicio de autor">AEM Almacenamiento en caché del servicio de autor</a></p>
                <p class="is-size-6">AEM El servicio de creación de utiliza una CDN administrada para ofrecer experiencias de creación optimizadas.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
