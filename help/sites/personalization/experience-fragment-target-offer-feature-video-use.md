---
title: Uso de Ofertas de fragmentos de experiencias AEM en Adobe Target
seo-title: Uso de Ofertas de fragmentos de experiencias AEM en Adobe Target
description: Adobe Experience Manager 6.4 reinventa el flujo de trabajo de personalización entre AEM y Destinatario. Las experiencias creadas en AEM ahora se pueden entregar directamente a Adobe Target como Ofertas HTML. Permite a los especialistas en marketing probar y personalizar contenido sin problemas en diferentes canales.
seo-description: Adobe Experience Manager 6.4 reinventa el flujo de trabajo de personalización entre AEM y Destinatario. Las experiencias creadas en AEM ahora se pueden entregar directamente a Adobe Target como Ofertas HTML. Permite a los especialistas en marketing probar y personalizar contenido sin problemas en diferentes canales.
sub-product: content-services
feature: experience-fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 10%

---


# Uso de Ofertas de fragmentos de experiencia dentro de Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4 reinventa el flujo de trabajo de personalización entre AEM y Destinatario. Las experiencias creadas en AEM ahora se pueden entregar directamente a Adobe Target como Ofertas HTML. Permite a los especialistas en marketing probar y personalizar contenido sin problemas en diferentes canales.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Se recomienda utilizar la biblioteca del cliente at.js y la mejor opción es utilizar soluciones de administración de etiquetas como Launch by Adobe, Adobe DTM o cualquier solución de administración de etiquetas de terceros para agregar bibliotecas de destinatarios a las páginas del sitio

>[!NOTE]
>
>AEM Ofertas de fragmentos de experiencia dentro de Adobe Target también está disponible como un paquete de funciones para usuarios de AEM 6.3. Consulte la siguiente sección para conocer los paquetes de funciones y las dependencias.


* Adobe Experience Manager ofrece un mecanismo de creación de contenido potente y fácil de usar junto con Adobe Target Artificial Intelligence (AI) y Machine Learning ayuda a los autores de contenido a crear y administrar contenido para todos los canales en una ubicación centralizada. Con la capacidad de exportar fragmentos de experiencias a Adobe Target como ofertas HTML, los especialistas en marketing ahora tienen más flexibilidad para crear una experiencia más personalizada mediante estas ofertas y ahora pueden probar y escalar cada experiencia que crean.
* La diferencia principal entre las ofertas HTML y las ofertas de fragmentos de experiencia es que la edición para las versiones posteriores solo se puede realizar en AEM y, a continuación, sincronizarse con Adobe Target
* La configuración del servicio de destinatario Cloud aplicada a la carpeta de fragmentos de experiencia hereda todos los fragmentos de experiencia creados directamente en la carpeta principal. La carpeta secundaria no hereda la configuración de los servicios en la nube principales.
* Para crear una oferta personalizada, ahora podemos aprovechar fácilmente el contenido almacenado en AEM.
* Puede crear tipos de actividades de Destinatario, incluidas las actividades Sensei, como la asignación automática, el Destinatario automático y Automated Personalization

## AEM 6.3 Feature Packs y Dependencias {#aem-feature-packs-and-dependencies}

| Paquete de funciones de AEM 6.3 | Dependencias |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/collect efixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/collect efixpack:aem-6.3.2-cfp:2.0 |

## Recursos adicionales {#additional-resources}

* [Personalización fluida: fragmentos de experiencia AEM en Adobe Target](https://www.youtube.com/watch?v=ohvKDjCb1yM)
* [Documentación de fragmentos de experiencia](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Uso de fragmentos de experiencias](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
