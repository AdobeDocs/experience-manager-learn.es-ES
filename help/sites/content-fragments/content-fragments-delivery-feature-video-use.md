---
title: Envío de fragmentos de contenido en AEM
seo-title: Envío de fragmentos de contenido en Adobe Experience Manager
description: Los fragmentos de contenido, independientemente del diseño, se pueden utilizar directamente en AEM Sites con componentes principales o se pueden entregar de forma automática a canales descendentes.
seo-description: Los fragmentos de contenido, independientemente del diseño, se pueden utilizar directamente en AEM Sites con componentes principales o se pueden entregar de forma automática a canales descendentes.
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---


# Entrega de fragmentos de contenido {#delivering-content-fragments}

Los fragmentos de contenido de Adobe Experience Manager (AEM) son contenido editorial basado en texto que puede incluir algunos elementos de datos estructurados asociados pero considerados contenido puro sin información de diseño o diseño. Los fragmentos de contenido se suelen crear como contenido no agnóstico del canal, que está diseñado para utilizarse y reutilizarse en varios canales, lo que a su vez envuelve el contenido en una experiencia específica del contexto.

Los fragmentos de contenido, independientemente del diseño, se pueden utilizar directamente en AEM Sites con componentes principales o se pueden entregar de forma automática a canales descendentes.

Esta serie de vídeos cubre las opciones de envío para utilizar fragmentos de contenido. Los detalles sobre la definición y [creación de fragmentos de contenido se encuentran aquí](content-fragments-feature-video-use.md).

1. Uso de fragmentos de contenido en páginas web
2. Exposición de fragmentos de contenido como JSON mediante los servicios de contenido de AEM
3. Uso de la API HTTP de Assets

## Uso de fragmentos de contenido en páginas web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Los fragmentos de contenido se pueden usar en páginas de AEM Sites o de forma similar en fragmentos de experiencias utilizando los componentes principales de WCM de AEM [Fragmento de contenido](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html).

Los componentes de fragmento de contenido se pueden diseñar utilizando el sistema de estilos de AEM para mostrar el contenido según sea necesario.

## Exposición de fragmentos de contenido como JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

Los servicios de contenido de AEM facilitan la creación de puntos finales HTTP basados en páginas de AEM que representen contenido en un formato JSON normalizado.

El vídeo anterior utiliza el [Componente de fragmento de contenido](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html) para exponer fragmentos de contenido individuales. El [Componente de lista de fragmentos de contenido](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) es un componente nuevo que permite a un autor definir una consulta que rellenará dinámicamente la página con una lista de fragmentos de contenido. Se prefiere el componente Lista de fragmentos de contenido cuando es necesario exponer varios fragmentos de contenido.

*Ejemplo de carga útil JSON de punto final de Content Services:*\
**[atletas.json](assets/athletes.json)**

## Uso de la API HTTP de Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

La primera vez que se presenta en AEM 6.5, se mejora la compatibilidad con fragmentos de contenido con la API HTTP de recursos. Esto proporciona una manera fácil para que los desarrolladores realicen operaciones de Crear, Leer, Actualizar y Eliminar (CRUD) con fragmentos de contenido.

*Ejemplos de solicitudes POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Qué método de envío utilizar

### Canal web

El método para enviar un fragmento de contenido a través de un canal web es sencillo mediante el uso del componente Fragmento de contenido con AEM Sites.

### Sin encabezado

Existen dos opciones para exponer fragmento de contenido como JSON para admitir un canal de terceros en un caso de uso remoto:

1. Utilice los servicios de contenido de AEM y las páginas de API proxy (vídeo 2) cuando el caso de uso principal sea la entrega de fragmentos de contenido para su consumo (solo lectura) por un canal de terceros. El marco de servicios de contenido proporciona más flexibilidad y opciones sobre qué datos se exponen. Los desarrolladores también pueden ampliar el marco de servicios de contenido para aumentar o enriquecer los datos.

2. Utilice la API HTTP de recursos (vídeo n.º 3) cuando el canal de terceros necesite modificar o actualizar fragmentos de contenido. Un caso de uso típico es la ingesta de contenido de terceros en un entorno de creación de AEM.

## Recursos adicionales {#additional-resources}

* [Creación de fragmentos de contenido](content-fragments-feature-video-use.md)
* [Componentes principales de WCM de AEM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)
* [Componente de fragmento de contenido principal de WCM de AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

Para descargar e instalar el paquete siguiente en una instancia de AEM 6.4+ para el estado final de la serie de vídeos:\
**[aem_demo_fluidos-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
