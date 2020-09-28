---
title: Distribución de fragmentos de contenido en AEM
seo-title: Distribución de fragmentos de contenido en Adobe Experience Manager
description: Los fragmentos de contenido, independientemente de su diseño, se pueden utilizar directamente en AEM Sites con componentes principales o se pueden distribuir sin encabezado a canales posteriores.
seo-description: Los fragmentos de contenido, independientemente de su diseño, se pueden utilizar directamente en AEM Sites con componentes principales o se pueden distribuir sin encabezado a canales posteriores.
sub-product: content-services
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 6%

---


# Distribución de fragmentos de contenido {#delivering-content-fragments}

Los fragmentos de contenido de Adobe Experience Manager (AEM) son contenido editorial basado en texto que puede incluir algunos elementos de datos estructurados asociados pero considerados contenido puro sin información de diseño o diseño. Los fragmentos de contenido se suelen crear como contenido no apto para el canal, que se va a utilizar y reutilizar en canales, lo que a su vez envuelve el contenido en una experiencia específica para el contexto.

Los fragmentos de contenido, independientemente de su diseño, se pueden utilizar directamente en AEM Sites con componentes principales o se pueden distribuir sin encabezado a canales posteriores.

Esta serie de vídeos cubre las opciones de envío para usar fragmentos de contenido. Los detalles sobre la definición y [creación de fragmentos de contenido se pueden encontrar aquí](content-fragments-feature-video-use.md).

1. Uso de fragmentos de contenido en páginas web
2. Exposición de fragmentos de contenido como JSON mediante AEM Content Services
3. Uso de la API HTTP de Assets

## Uso de fragmentos de contenido en páginas Web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Los fragmentos de contenido se pueden utilizar en páginas de AEM Sites o de forma similar, en fragmentos de experiencia, con el componente [Fragmento de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)contenido de los componentes principales de WCM de AEM.

Los componentes de fragmento de contenido se pueden diseñar con AEM sistema de estilos para mostrar el contenido según sea necesario.

## Exposición de fragmentos de contenido como JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services facilita la creación de AEM extremos HTTP basados en páginas que representen contenido en un formato JSON normalizado.

El vídeo anterior utiliza el componente [Fragmento de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html) contenido para exponer fragmentos de contenido individuales. El componente [de Lista de fragmentos de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) contenido es un componente nuevo que permite a un autor definir una consulta que rellenará dinámicamente la página con una lista de fragmentos de contenido. El componente Lista de fragmentos de contenido es preferible cuando es necesario exponer varios fragmentos de contenido.

*Ejemplo de carga útil JSON de punto final de Content Services:*\
**[atletas.json](assets/athletes.json)**

## Uso de la API HTTP de Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

La primera vez que se introdujo en AEM 6.5, se ha mejorado la compatibilidad con fragmentos de contenido con la API HTTP de recursos. Esto permite a los desarrolladores realizar fácilmente operaciones de creación, lectura, actualización y eliminación (CRUD) con fragmentos de contenido.

*Ejemplo de solicitudes POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Qué método de envío utilizar

### Canal web

El método para enviar un fragmento de contenido mediante un canal web es sencillo mediante el uso del componente Fragmento de contenido con AEM Sites.

### Sin cabeza

Existen dos opciones para exponer Fragmento de contenido como JSON para admitir un canal de terceros en un caso de uso sin encabezado:

1. Utilice AEM Content Services y las páginas de API de proxy (vídeo 2) cuando el caso de uso principal sea la entrega de fragmentos de contenido para consumo (solo lectura) por un canal de terceros. El marco de servicios de contenido proporciona más flexibilidad y opciones sobre qué datos se exponen. Los desarrolladores también pueden ampliar el marco de servicios de contenido para aumentar o enriquecer los datos.

2. Utilice la API HTTP de recursos (vídeo nº 3) cuando el canal de terceros necesite modificar o actualizar fragmentos de contenido. Un caso de uso típico es la ingesta de contenido de terceros en un entorno de autor AEM.

## Recursos adicionales {#additional-resources}

* [Creación de fragmentos de contenido](content-fragments-feature-video-use.md)
* [Componentes principales de AEM WCM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)
* [Componente de fragmento de contenido principal de AEM WCM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

Para descargar e instalar el paquete siguiente en una instancia de AEM 6.4+ para el estado final de la serie de vídeos:\
**[aem_demo_rich-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
