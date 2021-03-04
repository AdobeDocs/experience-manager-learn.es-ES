---
title: Uso de ofertas de fragmentos de experiencia de AEM en Adobe Target
seo-title: Uso de ofertas de fragmentos de experiencia de AEM en Adobe Target
description: Adobe Experience Manager 6.4 reimagina el flujo de trabajo de personalización entre AEM y Target. Las experiencias creadas dentro de AEM ahora se pueden entregar directamente a Adobe Target como ofertas HTML. Permite a los especialistas en marketing probar y personalizar contenido sin problemas en los distintos canales.
seo-description: Adobe Experience Manager 6.4 reimagina el flujo de trabajo de personalización entre AEM y Target. Las experiencias creadas dentro de AEM ahora se pueden entregar directamente a Adobe Target como ofertas HTML. Permite a los especialistas en marketing probar y personalizar contenido sin problemas en los distintos canales.
sub-product: content-services
feature: Fragmentos de experiencias
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: Personalización
role: Profesional empresarial
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 11%

---


# Uso de ofertas de fragmentos de experiencias en Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4 reimagina el flujo de trabajo de personalización entre AEM y Target. Las experiencias creadas dentro de AEM ahora se pueden entregar directamente a Adobe Target como ofertas HTML. Permite a los especialistas en marketing probar y personalizar contenido sin problemas en los distintos canales.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Se recomienda utilizar la biblioteca de cliente de at.js y la práctica recomendada es utilizar soluciones de administración de etiquetas como Launch By Adobe, Adobe DTM o cualquier solución de administración de etiquetas de terceros para agregar bibliotecas de destino a las páginas del sitio

>[!NOTE]
>
>Las ofertas de fragmentos de experiencia de AEM en Adobe Target también están disponibles como paquete de funciones para los usuarios de AEM 6.3. Consulte la siguiente sección sobre paquetes de funciones y dependencias.


* El mecanismo de creación de contenido potente y fácil de usar de Adobe Experience Manager, junto con la inteligencia artificial (IA) y el aprendizaje automático de Adobe Target, ayudan a los autores de contenido a crear y administrar el contenido de todos los canales en una ubicación centralizada. Con la capacidad de exportar fragmentos de experiencias a Adobe Target como ofertas HTML, los especialistas en marketing ahora tienen más flexibilidad para crear una experiencia más personalizada mediante estas ofertas y ahora pueden probar y escalar cada experiencia que crean.
* La principal diferencia entre las ofertas HTML y las ofertas de fragmento de experiencia es que la edición para el futuro solo se puede realizar en AEM y, a continuación, sincronizarse con Adobe Target
* La configuración del servicio de nube de Target aplicada a la carpeta de fragmentos de experiencias hereda de todos los fragmentos de experiencias creados directamente en la carpeta principal. La carpeta secundaria no hereda la configuración de los servicios de nube principales.
* Para crear una oferta personalizada, ahora podemos aprovechar fácilmente el contenido almacenado en AEM.
* Puede crear tipos de actividades de Target, incluidas las actividades con tecnología Sensei como Asignación automática, Segmentación automática y Personalización automatizada

## Paquetes de funciones y dependencias de AEM 6.3 {#aem-feature-packs-and-dependencies}

| Paquete de funciones de AEM 6.3 | Dependencias |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## Recursos adicionales {#additional-resources}

* [Documentación de fragmentos de experiencias](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Uso de fragmentos de experiencias](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
