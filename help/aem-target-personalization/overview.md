---
title: Introducción a AEM y Adobe Target
seo-title: Introducción a AEM y Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer experiencias personalizadas con Adobe Experience Manager y Adobe Target. En este tutorial, también aprenderá sobre diferentes personalidades involucradas en el proceso de extremo a extremo y cómo colaboran entre sí
seo-description: Un tutorial completo que muestra cómo crear y ofrecer una experiencia personalizada con Adobe Experience Manager y Adobe Target. En este tutorial, también aprenderá sobre diferentes personalidades involucradas en el proceso de extremo a extremo y cómo colaboran entre sí
translation-type: tm+mt
source-git-commit: c4ddafe392f74be8401f3ef6e07fc9d463d7620a
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 2%

---


# Introducción a AEM y Adobe Target {#getting-started-with-aem-target}

AEM y Destinatario son soluciones poderosas con capacidades aparentemente superpuestas. A veces, los clientes tienen dificultades para comprender cómo y cuándo utilizar estos productos en conjunto para ofrecer una experiencia personalizada. Para ofrecer una experiencia optimizada para cada usuario final, los diferentes equipos de su organización deben trabajar de cerca y definir quién hace qué.

En este tutorial, tratamos tres escenarios diferentes para AEM y Destinatario, lo que le ayuda a comprender qué es lo que mejor funciona para su organización y cómo colaboran los distintos equipos.

* Escenario 1: Personalización mediante fragmentos de experiencias AEM
* Escenario 2: Personalización mediante el Compositor de experiencias visuales
* Escenario 3: Personalización de las experiencias de página Web completa

## Personalización mediante fragmentos de experiencia AEM {#personalization-using-aem-experience-fragment}

Para este escenario, vamos a usar AEM y Destinatario. Claramente, ambos productos tienen sus propias fortalezas, y cuando se trata de ofrecer experiencias personalizadas a los usuarios del sitio, necesita **contenido personalizado (contenido de AEM)** y una **manera inteligente (Destinatario)** para ofrecer este contenido basado en un usuario específico.

AEM le ayuda a crear contenido personalizado, reuniendo todo su contenido y recursos en una ubicación central para impulsar su estrategia de personalización. AEM le permite crear fácilmente contenido para equipos de escritorio, tabletas y dispositivos móviles en un solo lugar sin necesidad de escribir código. No es necesario crear páginas para cada dispositivo, AEM ajusta automáticamente cada experiencia con el contenido. También puede exportar el contenido de AEM a Adobe Target como ofertas pulsando un botón.

Ahora tenemos contenido personalizado en forma de Ofertas de AEM en Destinatario. Destinatario le permite ofrecer estas ofertas a escala basándose en una combinación de enfoques de aprendizaje automático basados en reglas y dirigidos por AI que incorporan variables de comportamiento, contextuales y sin conexión.  Con Destinatario, puede configurar y ejecutar fácilmente actividades A/B y multivariadas (MVT) para determinar las mejores ofertas, contenido y experiencias.

**Los** fragmentos de experiencia representan un gran paso adelante para vincular a los creadores de contenido y experiencia con los profesionales de personalización que dirigen los resultados empresariales mediante el uso del Destinatario.

* Autores del editor de contenido de AEM personalizados como fragmentos de experiencia y sus variaciones
* AEM exporta HTML de fragmento de experiencia a &#x200B; de Destinatario
* Destinatario &#x200B; utiliza AEM marca de fragmento de experiencia como Ofertas en Actividades
* Destinatario ofrece HTML de fragmento de experiencia, AEM proporciona imágenes a las que se hace referencia

   ![Personalización mediante el diagrama de fragmentos de experiencia](assets/personalization-use-case-1/use-case-1-diagram.png)

**Para implementar este escenario, debe:**

* [Integración de AEM y Adobe Target con Launch y Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM y Adobe Target con Cloud Services heredados](./implementation.md#integrating-aem-target-options)

***Después de implementar las integraciones anteriores, exploremos el  [escenario en detalle](./personalization-use-case-1.md).***

## Personalización mediante el Compositor de experiencias visuales

Los especialistas en marketing pueden realizar cambios rápidos en su sitio web sin cambiar ningún código para ejecutar una prueba con el Compositor de experiencias visuales (VEC) de Adobe Target. El VEC es una interfaz de usuario WYSIWYG (lo que se ve es lo que se obtiene) que le permite crear y probar fácilmente experiencias y Ofertas personalizadas en el contexto del sitio. Puede crear experiencias y Ofertas para actividades de Destinatario arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web (o Oferta) o página web móvil.

VEC es una de las principales características de Adobe Target. El VEC permite a los comerciantes y diseñadores crear y cambiar contenido mediante una interfaz visual. Se pueden realizar muchas opciones de diseño sin necesidad de editar directamente el código. También es posible editar HTML y JavaScript con las opciones de edición disponibles en el compositor.

* El contenido reside en AEM y los editores de contenido crean y administran las páginas del sitio
* Destinatario utiliza AEM páginas del sitio hospedadas para ejecutar pruebas y personalización
* Destinatario ofrece contenido personalizado
* Se crea contenido nuevo neto con Adobe Target VEC
* Se aplica tanto a sitios alojados AEM como a sitios alojados no AEM

   ![Personalización mediante el diagrama del Compositor de experiencias visuales](assets/personalization-use-case-3/use-case-diagram-3.png)

**Para implementar este escenario, debe:**

* [Integración de AEM y Adobe Target con Launch y Adobe I/O](./implementation.md#integrating-aem-target-options)

***Después de implementar la integración anterior, exploremos el  [escenario en detalle.](./personalization-use-case-3.md)***

## Personalización de las experiencias de página Web completa

La integración de Adobe Experience Manager con Adobe Target le ayuda a ofrecer una experiencia personalizada a los usuarios del sitio. Además, ayuda a comprender mejor qué versiones del contenido del sitio web mejoran las conversiones durante un período de prueba específico. Por ejemplo: una prueba A/B compara dos o más versiones del contenido del sitio web para ver cuál aumenta mejor las conversiones, las ventas u otras métricas que identifique. Un especialista en mercadotecnia puede crear actividades dentro de Adobe Target para comprender cómo los usuarios interactúan con el contenido del sitio y cómo afecta las métricas del mismo.

* El contenido reside en AEM y los editores de contenido crean y administran las páginas del sitio
* Destinatario utiliza AEM páginas del sitio hospedadas para ejecutar pruebas y personalización
* Destinatario ofrece contenido personalizado
* No se crea ningún contenido nuevo neto aquí
* Se aplica a sitios AEM y no AEM

   ![diagrama](assets/personalization-use-case-2/use-case-2-diagram.png)

**Para implementar este escenario, debe:**

* [Integración de AEM y Adobe Target con Launch y Adobe I/O](./implementation.md#integrating-aem-target-options)

***Después de implementar la integración anterior, exploremos el  [escenario en detalle.](./personalization-use-case-2.md)***
