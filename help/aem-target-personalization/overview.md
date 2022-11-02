---
title: Introducción a AEM y Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer experiencias personalizadas con Adobe Experience Manager y Adobe Target. En este tutorial, también aprenderá sobre diferentes personalidades que participan en el proceso de principio a fin y cómo colaboran entre sí
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 1%

---

# Introducción a AEM y Adobe Target {#getting-started-with-aem-target}

AEM y Target son soluciones poderosas con capacidades aparentemente superpuestas. A veces, los clientes tienen dificultades para comprender cómo y cuándo utilizar estos productos en conjunto para ofrecer una experiencia personalizada. Para ofrecer una experiencia optimizada para cada usuario final, los distintos equipos de su organización deben trabajar de cerca y definir quién hace qué.

En este tutorial, tratamos tres escenarios diferentes para AEM y Target, lo que le ayuda a comprender qué funciona mejor para su organización y cómo colaboran los distintos equipos.

* Escenario 1 : Personalización mediante AEM fragmentos de experiencias
* Escenario 2 : Personalización mediante el Compositor de experiencias visuales
* Escenario 3 : Personalización de las experiencias de página web completa

## Personalización mediante AEM fragmentos de experiencias {#personalization-using-aem-experience-fragment}

Para este escenario, vamos a usar AEM y Target. Claramente, ambos productos tienen sus propias ventajas y, cuando se trata de ofrecer experiencias personalizadas a los usuarios del sitio, usted necesita **contenido personalizado (contenido de AEM)** y **forma inteligente (Target)** para ofrecer este contenido en función de un usuario específico.

AEM ayuda a crear contenido personalizado, que reúne todo el contenido y los recursos en una ubicación central para impulsar su estrategia de personalización. AEM permite crear fácilmente contenido para escritorios, tabletas y dispositivos móviles en un solo lugar sin tener que escribir código. No es necesario crear páginas para cada dispositivo: AEM ajusta automáticamente cada experiencia con el contenido. También puede exportar el contenido de AEM a Adobe Target como ofertas con solo pulsar un botón.

Ahora tenemos contenido personalizado en forma de ofertas de AEM en Target. Target le permite ofrecer estas ofertas a escala en función de una combinación de enfoques de aprendizaje automático basados en reglas y dirigidos por AI que incorporan variables de comportamiento, contextuales y sin conexión.  Con Target, puede configurar y ejecutar fácilmente actividades A/B y multivariable (MVT) para determinar las mejores ofertas, contenidos y experiencias.

**Fragmentos de experiencias** representan un gran paso adelante para vincular a los creadores de contenido/experiencia con los profesionales de personalización que dirigen los resultados comerciales mediante Target.

* AEM editor de contenido crea contenido personalizado como fragmentos de experiencias y sus variaciones
* AEM exporta el HTML de fragmentos de experiencia al &#x200B; de Target
* Target &#x200B; utiliza AEM marcado de fragmento de experiencia como ofertas en actividades
* Target ofrece el HTML de fragmentos de experiencia, AEM proporciona imágenes referenciadas

   ![Personalización mediante el diagrama de fragmentos de experiencias](assets/personalization-use-case-1/use-case-1-diagram.png)

**Para implementar este escenario, debe:**

* [Integración de AEM y Adobe Target con Launch y Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM y Adobe Target con Cloud Services heredados](./implementation.md#integrating-aem-target-options)

***Después de implementar las integraciones anteriores, exploremos el [escenario en detalle](./personalization-use-case-1.md).***

## Personalización mediante el Compositor de experiencias visuales

Los especialistas en marketing pueden realizar cambios rápidos en su sitio web sin cambiar ningún código para ejecutar una prueba mediante el Compositor de experiencias visuales (VEC) de Adobe Target. El VEC es WYSIWYG (lo que ve es lo que obtiene), una interfaz de usuario que permite crear y probar fácilmente experiencias y ofertas personalizadas en el contexto del sitio. Puede crear experiencias y ofertas para actividades de Target arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web (u oferta) o una página web móvil.

VEC es una de las características principales de Adobe Target. El VEC permite que los especialistas en marketing y los diseñadores creen y cambien contenido mediante una interfaz visual. Se pueden realizar muchas opciones de diseño sin necesidad de editar directamente el código. También es posible editar el HTML y JavaScript mediante las opciones de edición disponibles en el compositor.

* El contenido reside en AEM y los editores de contenido crean y administran las páginas del sitio
* Target usa AEM páginas del sitio alojadas para ejecutar pruebas y personalizaciones
* Target ofrece contenido personalizado
* El nuevo contenido neto se crea mediante el VEC de Adobe Target
* Se aplica tanto a sitios alojados AEM como a sitios alojados sin AEM

   ![Personalización mediante el diagrama del Compositor de experiencias visuales](assets/personalization-use-case-3/use-case-diagram-3.png)

**Para implementar este escenario, debe:**

* [Integración de AEM y Adobe Target con Launch y Adobe I/O](./implementation.md#integrating-aem-target-options)

***Después de implementar la integración anterior, exploremos la [en detalle.](./personalization-use-case-3.md)***

## Personalización de las experiencias de página web completa

La integración de Adobe Experience Manager con Adobe Target le ayuda a ofrecer una experiencia personalizada a los usuarios del sitio. Además, también le ayuda a comprender mejor qué versiones del contenido del sitio web mejoran las conversiones durante un periodo de prueba especificado. Por ejemplo, una prueba A/B compara dos o más versiones del contenido del sitio web para ver cuál aumenta más las conversiones, las ventas u otras métricas que identifique. Un especialista en marketing puede crear actividades dentro de Adobe Target para comprender cómo interactúan los usuarios con el contenido del sitio y cómo afecta a las métricas del mismo.

* El contenido reside en AEM y los editores de contenido crean y administran las páginas del sitio
* Target usa AEM páginas del sitio alojadas para ejecutar pruebas y personalizaciones
* Target ofrece contenido personalizado
* Aquí no se crea ningún contenido nuevo neto
* Se aplica a sitios AEM y no AEM

   ![diagrama](assets/personalization-use-case-2/use-case-2-diagram.png)

**Para implementar este escenario, debe:**

* [Integración de AEM y Adobe Target con Launch y Adobe I/O](./implementation.md#integrating-aem-target-options)

***Después de implementar la integración anterior, exploremos la [en detalle.](./personalization-use-case-2.md)***
