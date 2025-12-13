---
title: Almacenamiento en caché del servicio AEM Author
description: Información general sobre el almacenamiento en caché del servicio de AEM as a Cloud Service Author.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Performance
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: b8e09820-f1f2-4897-b454-16c0df5a0459
duration: 56
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 5%

---

# AEM Author

El autor de AEM tiene un almacenamiento en caché limitado debido a la naturaleza altamente dinámica y sensible a los permisos del contenido que sirve. En general, no se recomienda personalizar el almacenamiento en caché para AEM Author, sino basarse en las configuraciones de caché proporcionadas por Adobe para garantizar un buen rendimiento de la experiencia.

![Diagrama de información general sobre el almacenamiento en caché de AEM Author](./assets/author/author-all.png){align="center"}

Aunque se desaconseja personalizar el almacenamiento en caché en AEM Author, resulta útil saber que AEM Author tiene una CDN administrada por Adobe, pero no tiene una Dispatcher de AEM. Recuerde que todas las configuraciones de Dispatcher de AEM se omiten en AEM Author, ya que no tiene un Dispatcher de AEM.

## La red de distribución de contenido (CDN)

El servicio de creación de AEM utiliza una CDN, pero su propósito es mejorar la entrega de recursos de productos y no debe configurarse exhaustivamente, dejando que funcione tal cual.

![Diagrama de información general sobre almacenamiento en caché de publicación de AEM](./assets/author/author-cdn.png){align="center"}

La CDN de autor de AEM se encuentra entre el usuario final, normalmente un experto en marketing o autor de contenido, y el autor de AEM. Almacena en caché archivos inmutables, como recursos estáticos que alimentan la experiencia de creación de AEM, y no contenido creado.

La red de distribución de contenido (CDN) del autor de AEM almacena en caché varios tipos de recursos que pueden ser de interés, entre ellos un [TTL personalizable en consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) y un TTL [largo en bibliotecas de cliente personalizadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### Duración predeterminada de la caché

La CDN de autor de AEM almacena en caché los siguientes recursos de cara al cliente, que tienen la siguiente duración de caché predeterminada:

| Tipo de contenido | Duración predeterminada de la caché de CDN |
|:------------ |:---------- |
| [Consultas persistentes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1 minuto |
| [Bibliotecas de cliente (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 días |
| [Todo lo demás](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | No almacenado en caché |


## Dispatcher de AEM

El servicio de autor de AEM no incluye AEM Dispatcher y solo usa la [CDN](#cdn) para el almacenamiento en caché.
