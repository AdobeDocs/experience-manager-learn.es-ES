---
title: Introducción a AEM y Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer experiencias personalizadas con Adobe Experience Manager y Adobe Target. En este tutorial, también aprenderá sobre las distintas personas involucradas en el proceso de extremo a extremo y cómo colaboran entre sí
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '846'
ht-degree: 0%

---

# Integración de AEM Sites y Adobe Target {#getting-started-with-aem-target}

AEM Tanto Target como Target son soluciones potentes con capacidades aparentemente superpuestas. A veces, los clientes tienen dificultades para comprender cómo y cuándo utilizar estos productos en conjunto para ofrecer una experiencia personalizada. Para ofrecer una experiencia optimizada para cada usuario final, los diferentes equipos de su organización deben trabajar de cerca y definir quién hace qué.

AEM En este tutorial, se explican tres escenarios diferentes para y Target, lo que le ayuda a comprender qué es lo que mejor funciona para su organización y cómo colaboran los distintos equipos.

* Escenario 1: Personalization AEM usa fragmentos de experiencias de la
* Escenario 2: Personalization con el Compositor de experiencias visuales
* Escenario 3 : Personalization de experiencias de página web completa

## Personalization AEM con fragmentos de experiencia de la {#personalization-using-aem-experience-fragment}

AEM Para este escenario, vamos a usar la opción de destino y la opción de segmentación de datos de. AEM Claramente, ambos productos tienen sus propias fortalezas, y cuando se trata de ofrecer experiencias personalizadas a los usuarios de su sitio, usted necesita **contenido personalizado (contenido de la lista de productos personalizados)** y una **manera inteligente (Target)** de ofrecer este contenido en función de un usuario específico.

AEM le ayuda a crear contenido personalizado, reuniendo todo el contenido y los recursos en una ubicación central para impulsar su estrategia de personalización. AEM le permite crear fácilmente contenido para equipos de escritorio, tabletas y dispositivos móviles en un solo lugar y sin tener que escribir código. AEM No es necesario crear páginas para cada dispositivo: ajusta automáticamente todas las experiencias con el uso de su contenido. AEM También puede exportar el contenido de a Adobe Target como ofertas con solo pulsar un botón.

AEM Ahora tenemos contenido personalizado en forma de Ofertas de los clientes de Target Target permite entregar estas ofertas a escala en función de una combinación de enfoques de aprendizaje automático basados en reglas y controlados por IA que incorporan variables de comportamiento, contextuales y sin conexión.  Target le permite configurar y ejecutar fácilmente actividades A/B y multivariable (MVT) para determinar las mejores ofertas, contenidos y experiencias.

Los **fragmentos de experiencias** representan un enorme paso adelante en el vínculo entre los creadores de contenido y experiencia y los profesionales de la personalización que mejoran los resultados empresariales con Target.

* AEM Los autores del editor de contenido personalizaron el contenido como Fragmentos de experiencias y sus variaciones
* AEM Exporta el HTML de fragmentos de experiencias a Target&#x200B;
* AEM Target&#x200B; utiliza el marcado del fragmento de experiencia de la como ofertas en las actividades
* Target ofrece un HTML AEM de Fragmentos de experiencias, mientras que proporciona imágenes a las que se hace referencia en el

  ![Personalization con el diagrama de fragmentos de experiencias](assets/personalization-use-case-1/use-case-1-diagram.png)

**Para implementar este escenario, debe:**

* [AEM Integración de y Adobe Target mediante etiquetas y Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM Adobe Target con Cloud Service heredados](./implementation.md#integrating-aem-target-options)

***Después de implementar las integraciones anteriores, vamos a explorar el [escenario en detalle](./personalization-use-case-1.md).***

## Personalization con el Compositor de experiencias visuales

Los especialistas en marketing pueden realizar cambios rápidos en su sitio web sin cambiar ningún código para ejecutar una prueba con el Compositor de experiencias visuales (VEC) de Adobe Target. El VEC es una interfaz de usuario WYSIWYG (lo que ve es lo que obtiene) que le permite crear y probar fácilmente experiencias y ofertas personalizadas en el contexto del sitio. Puede crear experiencias y ofertas para actividades de Target arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web (u oferta) o una página web móvil.

El VEC es una de las características principales de Adobe Target. El VEC permite que los especialistas en marketing y los diseñadores creen y cambien contenido mediante una interfaz visual. Se pueden realizar muchas opciones de diseño sin requerir la edición directa del código. HTML de edición y JavaScript también es posible mediante las opciones de edición disponibles en el compositor.

* AEM El contenido se encuentra en el sitio, y los editores de contenido crean y administran las páginas del sitio
* AEM Target utiliza páginas del sitio alojadas en el servidor para ejecutar pruebas y personalización
* Target ofrece contenido personalizado
* El nuevo contenido neto se crea mediante el VEC de Adobe Target
* AEM AEM Se aplica tanto a los sitios alojados en el sitio como a los sitios no alojados en el sitio que no están alojados en el servidor de la

  ![Personalization con el diagrama del Compositor de experiencias visuales](assets/personalization-use-case-3/use-case-diagram-3.png)

**Para implementar este escenario, debe:**

* [AEM Integración de y Adobe Target mediante etiquetas y Adobe I/O](./implementation.md#integrating-aem-target-options)

***Después de implementar la integración anterior, vamos a explorar el [escenario en detalle.](./personalization-use-case-3.md)***

## Personalization de experiencias de página web completa

La integración de Adobe Experience Manager con Adobe Target le ayuda a ofrecer una experiencia personalizada a los usuarios del sitio. Además, también le ayuda a comprender mejor qué versiones del contenido del sitio web mejoran mejor las conversiones durante un período de prueba especificado. Por ejemplo, una prueba A/B compara dos o más versiones del contenido del sitio web para ver cuál mejora más las conversiones, las ventas u otras métricas que identifique. Un especialista en marketing puede crear actividades dentro de Adobe Target para comprender cómo interactúan los usuarios con el contenido del sitio y cómo afectan a sus métricas.

* AEM El contenido se encuentra en el sitio, y los editores de contenido crean y administran las páginas del sitio
* AEM Target utiliza páginas del sitio alojadas en el servidor para ejecutar pruebas y personalización
* Target ofrece contenido personalizado
* Aquí no se crea ningún contenido nuevo neto
* AEM AEM Se aplica tanto a los sitios de como a los que no son de

  ![diagrama](assets/personalization-use-case-2/use-case-2-diagram.png)

**Para implementar este escenario, debe:**

* [AEM Integración de y Adobe Target mediante etiquetas y Adobe I/O](./implementation.md#integrating-aem-target-options)

***Después de implementar la integración anterior, vamos a explorar el [escenario en detalle.](./personalization-use-case-2.md)***
