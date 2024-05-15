---
title: AEM Entrega de fragmentos de contenido en la
description: Los fragmentos de contenido, independientemente del diseño, se pueden utilizar directamente en AEM Sites con los componentes principales o se pueden entregar sin encabezado a los canales descendentes.
feature: Content Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 878
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---

# Entrega de fragmentos de contenido {#delivering-content-fragments}

Los fragmentos de contenido de Adobe Experience Manager AEM () son contenidos editoriales basados en texto que pueden incluir algunos elementos de datos estructurados asociados, pero que se consideran contenido puro sin información de diseño. Los fragmentos de contenido generalmente se crean como contenido no basado en canales; está pensado para utilizarse y reutilizarse en todos los canales, lo que a su vez envuelve el contenido en una experiencia específica del contexto.

Los fragmentos de contenido, independientemente del diseño, se pueden utilizar directamente en AEM Sites con los componentes principales o se pueden entregar sin encabezado a los canales descendentes.

Esta serie de vídeos trata las opciones de envío para utilizar fragmentos de contenido. Detalles sobre la definición y [Creación de fragmentos de contenido se puede encontrar aquí](content-fragments-feature-video-use.md).

1. Uso de fragmentos de contenido en páginas web
2. AEM Exposición de fragmentos de contenido como JSON mediante servicios de contenido de
3. Uso de la API HTTP de Assets

## Uso de fragmentos de contenido en páginas web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

Los fragmentos de contenido se pueden utilizar en páginas de AEM Sites AEM o, de forma similar, en Fragmentos de experiencias, utilizando los componentes principales de WCM de la manera más sencilla de usar, como los componentes principales de WCM de la página de la aplicación de la aplicación. [Componente Fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es).

AEM Los componentes de fragmento de contenido se pueden diseñar con el sistema de estilos de para mostrar el contenido según sea necesario.

## Exponer fragmentos de contenido como JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM AEM Los servicios de contenido facilitan la creación de puntos finales HTTP basados en páginas de que representen el contenido en un formato JSON normalizado.

El vídeo anterior utiliza el [Componente Fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es) para exponer fragmentos de contenido individuales. El [Componente Lista de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) es un nuevo componente que permite a un autor definir una consulta que rellenará dinámicamente la página con una lista de fragmentos de contenido. Se prefiere el componente Lista de fragmentos de contenido cuando es necesario exponer varios fragmentos de contenido.

*Ejemplo de carga útil JSON de extremo de Content Services:*\
**[athletes.json](assets/athletes.json)**

## Uso de la API HTTP de Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

AEM Introducido por primera vez en la versión 6.5, se ha mejorado la compatibilidad con los fragmentos de contenido con la API HTTP de Assets. Esto proporciona a los desarrolladores una forma sencilla de realizar operaciones de Crear, Leer, Actualizar y Eliminar (CRUD) con fragmentos de contenido.

*Ejemplos de solicitudes de POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Qué método de envío utilizar

### Canal web

El método para entregar un fragmento de contenido a través de un canal web es sencillo mediante el uso del componente Fragmento de contenido con AEM Sites.

### Headless

Existen dos opciones para exponer un fragmento de contenido como JSON para admitir un canal de terceros en un caso de uso sin encabezado:

1. AEM Utilice las páginas de servicios de contenido y API de proxy (#2 de vídeo) cuando el caso de uso principal sea Enviar fragmentos de contenido para su consumo (solo lectura) por un canal de terceros. El marco de Content Services proporciona más flexibilidad y opciones en cuanto a los datos que se exponen. Los desarrolladores también pueden ampliar el marco de servicios de contenido para aumentar o enriquecer los datos.

2. Utilice la API HTTP de Assets (#3 de vídeo) cuando el canal de terceros necesite modificar o actualizar fragmentos de contenido. AEM Un caso de uso típico es la ingesta de contenido de terceros en un entorno de creación de.

## Recursos adicionales {#additional-resources}

* [Creación de fragmentos de contenido](content-fragments-feature-video-use.md)
* [Componentes principales de WCM de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [AEM Componente de fragmento de contenido principal de WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es)

AEM Para descargar e instalar el paquete siguiente en una instancia de la versión 6.4 o posterior de para el estado final de la serie de vídeos:\
**[aem_demo_workflow-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
