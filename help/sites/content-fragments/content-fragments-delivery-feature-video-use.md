---
title: Entrega de fragmentos de contenido en AEM
description: Los fragmentos de contenido, independientemente del diseño, se pueden utilizar directamente en AEM Sites con los componentes principales o se pueden entregar sin encabezado a los canales descendentes.
feature: Content Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 878
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---

# Entrega de fragmentos de contenido {#delivering-content-fragments}

Los fragmentos de contenido de Adobe Experience Manager (AEM) son contenidos editoriales basados en texto que pueden incluir algunos elementos de datos estructurados asociados, pero que se consideran contenido puro sin información de diseño. Los fragmentos de contenido generalmente se crean como contenido no basado en canales; está pensado para utilizarse y reutilizarse en todos los canales, lo que a su vez envuelve el contenido en una experiencia específica del contexto.

Los fragmentos de contenido, independientemente del diseño, se pueden utilizar directamente en AEM Sites con los componentes principales o se pueden entregar sin encabezado a los canales descendentes.

Esta serie de vídeos trata las opciones de envío para utilizar fragmentos de contenido. Encontrará detalles sobre la definición y [creación de fragmentos de contenido aquí](content-fragments-feature-video-use.md).

1. Uso de fragmentos de contenido en páginas web
2. Exposición de fragmentos de contenido como JSON mediante servicios de contenido de AEM
3. Uso de la API HTTP de Assets

## Uso de fragmentos de contenido en páginas web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

Los fragmentos de contenido se pueden usar en páginas de AEM Sites o, de manera similar, en fragmentos de experiencias, con el [componente de fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es) de los componentes principales de WCM de AEM.

Los componentes de fragmento de contenido se pueden diseñar con el sistema de estilos de AEM para mostrar el contenido según sea necesario.

## Exponer fragmentos de contenido como JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services facilita la creación de puntos finales HTTP basados en páginas de AEM que procesan contenido en un formato JSON normalizado.

El vídeo anterior utiliza el [componente Fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es) para exponer fragmentos de contenido individuales. El [componente Lista de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html?lang=es) es un nuevo componente que permite a un autor definir una consulta que rellenará dinámicamente la página con una lista de fragmentos de contenido. Se prefiere el componente Lista de fragmentos de contenido cuando es necesario exponer varios fragmentos de contenido.

*Carga útil JSON de extremo de Content Services de ejemplo:*\
**[atletas.json](assets/athletes.json)**

## Uso de la API HTTP de Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

Presentado por primera vez en AEM 6.5, es compatible de forma mejorada con los fragmentos de contenido con la API HTTP de Assets. Esto proporciona a los desarrolladores una forma sencilla de realizar operaciones de Crear, Leer, Actualizar y Eliminar (CRUD) con fragmentos de contenido.

*Ejemplos de solicitudes de POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Qué método de envío utilizar

### Canal web

El método para entregar un fragmento de contenido a través de un canal web es sencillo mediante el uso del componente Fragmento de contenido con AEM Sites.

### Headless

Existen dos opciones para exponer un fragmento de contenido como JSON para admitir un canal de terceros en un caso de uso sin encabezado:

1. Utilice las páginas de API de proxy y servicios de contenido de AEM (#2 de vídeo) cuando el caso de uso principal sea Enviar fragmentos de contenido para su consumo (solo lectura) por un canal de terceros. El marco de Content Services proporciona más flexibilidad y opciones en cuanto a los datos que se exponen. Los desarrolladores también pueden ampliar el marco de servicios de contenido para aumentar o enriquecer los datos.

2. Utilice la API HTTP de Assets (#3 de vídeo) cuando el canal de terceros necesite modificar o actualizar fragmentos de contenido. Un caso de uso típico es la ingesta de contenido de terceros en un entorno de creación de AEM.

## Recursos adicionales {#additional-resources}

* [Creación de fragmentos de contenido](content-fragments-feature-video-use.md)
* [Componentes principales de WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [Componente de fragmento de contenido principal de AEM WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es)

Para descargar e instalar el paquete siguiente en una instancia de AEM 6.4 o posterior para el estado final de la serie de vídeos:\
**[aem_demo_fluido-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
