---
title: Demostración en directo de casos de uso de Personalization
description: Personalización de experiencias en acción en el sitio web de habilitación de WKND con pruebas A/B, segmentación basada en el comportamiento y ejemplos de personalización de usuarios conocidos.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations
role: Developer, Architect, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-11-03T00:00:00Z
jira: KT-19546
thumbnail: KT-19546.jpeg
source-git-commit: ed7af09d747d54a84d2583073d3c731388b5f516
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 1%

---

# Demostración en directo de casos de uso de personalización

Visite el [sitio web de habilitación de WKND](https://wknd.enablementadobe.com/us/en.html){target="wknd"} para ver ejemplos reales de pruebas A/B, segmentación basada en el comportamiento y personalización de usuarios conocidos.

>[!VIDEO](https://video.tv.adobe.com/v/3476464/?captions=spa&learn=on&enablevpops)

Esta página le guía a través de demostraciones prácticas de cada escenario de personalización. Utilícelo para explorar las posibilidades antes de crear estas funciones en su propio sitio de AEM.

>[!IMPORTANT]
>
> Abra el sitio de demostración en varias ventanas del explorador o en modo de exploración incógnito/privada para experimentar diferentes variaciones personalizadas simultáneamente.
> Al utilizar el modo de exploración privada, Firefox y Safari pueden bloquear la cookie de ECID o, alternativamente, utilizar el modo de exploración normal o borrar las cookies antes de intentar un nuevo escenario de personalización.

## Casos de uso de demostración

El [sitio web de habilitación de WKND](https://wknd.enablementadobe.com/us/en.html){target="wknd"} muestra tres tipos de personalización:

| Tipo de Personalization | Qué va a ver | Programación |
|---------------------|-----------------|---------|
| **Segmentación basada en el comportamiento** | El contenido se adapta en función de su comportamiento de navegación e intereses. Normalmente conocida como _personalización de página siguiente o de página misma_ | Tiempo real y por lotes |
| **Personalization de usuario conocido** | Experiencias personalizadas basadas en perfiles de clientes completos creados a partir de datos de varios sistemas. Normalmente se denomina _personalización a escala_ | Tiempo real |
| **Pruebas A/B** | Se han probado diferentes variaciones de contenido para encontrar el mejor ejecutante. Normalmente conocida como _experimentación_ | Tiempo real |

## Direccionamiento por comportamiento

El contenido se adapta automáticamente en función de las acciones y los intereses de los visitantes durante su sesión de navegación. Esto se conoce comúnmente como _personalización de página siguiente o de página misma_.

### Páginas de Inicio, Aventuras y Revistas

Estas experiencias aparecen inmediatamente en función de su comportamiento de navegación actual (personalización en tiempo real). Adobe Experience Platform Edge Network se utiliza para tomar decisiones de personalización en tiempo real.

| Página | Lo que va a ver | Cómo realizar las pruebas | Experiencia |
|------|-----------------|-------------|------------|
| [Hogar](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | Un banner personalizado **para toda la familia como héroe de aventuras**, que incluye una familia montando en bicicleta junto al lago, y que promueve experiencias guiadas que crean recuerdos compartidos | Visite el [Campamento de Surf de Bali](https://wknd.enablementadobe.com/us/en/adventures/bali-surf-camp.html){target="wknd"} o el [Tour Gastronómico por el Marais](https://wknd.enablementadobe.com/us/en/adventures/gastronomic-marais-tour.html){target="wknd"}, y luego regrese a la página principal | ![Hogar - Héroe de aventuras familiares](./assets/live-demo/behavioral-home-family-hero.png){width="200" zoomable="yes"} |
| [Aventuras](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Un héroe promocional **&quot;Free Bike Tune Up&quot; centrado en el ciclismo** con mensajes &quot;We&#39;ve Got You Covered&quot; y oferta de mantenimiento de bicicleta gratis de los socios expertos de WKND | Visite cualquier aventura relacionada con el ciclismo (por ejemplo, [Ciclismo en Toscana](https://wknd.enablementadobe.com/us/en/adventures/cycling-tuscany.html){target="wknd"}) y luego navegue hasta la página Aventuras | ![Aventuras - Héroe de puesta a punto gratis](./assets/live-demo/behavioral-adventures-bike-hero.png){width="200" zoomable="yes"} |
| [Aventuras](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | **héroe de la colección de engranajes** con temática de campamento que muestra el equipo de campamento esencial (sacos de dormir, chaquetas, botas) con mensajes &quot;Tu próxima aventura comienza con el engranaje adecuado&quot; | Visite cualquier aventura relacionada con el campamento (por ejemplo, [Mochilero Yosemite](https://wknd.enablementadobe.com/us/en/adventures/yosemite-backpacking.html){target="wknd"}) y luego navegue hasta la página Aventuras | ![Aventuras - Acampar Gear Collection Hero](./assets/live-demo/behavioral-adventures-camp-hero.png){width="200" zoomable="yes"} |
| [Revista](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | Una **promoción de venta de revista** que tiene en cuenta el tiempo y que incluye revistas WKND destacadas con el lema &quot;VENTA!&quot; insignias y precios especiales para lectores sobre números y colecciones al aire libre | Lea uno o más artículos de revistas (por ejemplo, [Recorrido de esquí](https://wknd.enablementadobe.com/us/en/magazine/ski-touring.html){target="wknd"}) y luego navegue hasta la página de aterrizaje de la revista | ![Revista - Héroe de ventas](./assets/live-demo/behavioral-magazine-sale-hero.png){width="200" zoomable="yes"} |

### Páginas de aventuras y revistas (lote)

Estas experiencias se basan en el comportamiento histórico y aparecen en la siguiente visita o más tarde el mismo día (personalización por lotes). Los datos se agregan y procesan en atributos de perfil y se activan en Adobe Experience Platform Edge Network.

| Página | Lo que va a ver | Cómo realizar las pruebas | Experiencia |
|------|-----------------|-------------|------------|
| [Aventuras](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Un héroe con tema de surf y **tablas de surf coloridas bajo las palmeras** con mensajes de &quot;Tu Recorrido de surf empieza aquí&quot; y contenido seleccionado de destinos de surf basado en tus intereses | Visita varias [aventuras relacionadas con el surf](https://wknd.enablementadobe.com/us/en/adventures.html#tabs-b4210c6ff3-item-b411b19941-tab){target="wknd"} y vuelve a la página de aventuras al día siguiente | ![Aventuras - Héroe del surf](./assets/live-demo/behavioral-adventures-surfing-hero.png){width="200" zoomable="yes"} |
| [Revista](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | Una **oferta personalizada de suscripción a una revista** que incluye destinos de viajes por el mundo con una furgoneta clásica de Volkswagen, haciendo hincapié en &quot;Tu experiencia personalizada en la revista&quot; con beneficios exclusivos para suscriptores | Lea tres o más [artículos de revista](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} y vuelva a la página de aterrizaje de la revista al día siguiente | ![Revista - Suscribirse a héroe](./assets/live-demo/behavioral-magazine-subscribe-hero.png){width="200" zoomable="yes"} |

**Más información:** ¿Listo para implementar la segmentación por comportamiento en tu propio sitio de AEM? Empiece con el [Tutorial de segmentación por comportamiento](./use-cases/behavioral-targeting.md) para conocer el proceso de configuración completo.

## Personalización de usuario conocido

Experiencias personalizadas basadas en perfiles de clientes completos creados a partir de datos de varios sistemas, incluido el historial de compras y la fase del ciclo de vida del cliente. Adobe Experience Platform Edge Network se utiliza para tomar decisiones de personalización en tiempo real.

### Héroe de página de inicio

El banner a pantalla completa de la página de inicio de WKND se personaliza según los perfiles de usuario autenticados. Realice pruebas con estas cuentas de demostración para ver una experiencia personalizada:

| Página | Lo que va a ver | Cómo realizar las pruebas | Contexto de perfil | Experiencia |
|------|-----------------|-------------|-----------------|------------|
| [Hogar](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | Interior de una tienda de esquí con **equipo de esquí premium con un &quot;EXTRA 25% de descuento&quot;** de promoción, con consejos de embalaje de expertos para prepararse para su próxima aventura de esquí | Iniciar sesión con `rwilson/rwilson` y actualizar la página | Aventuras de esquí compradas recientemente, con lo que se vende equipamiento de esquí | ![Venta adicional de equipo de esquí](./assets/live-demo/known-user-ski-gear-hero.png){width="200" zoomable="yes"} |

**Más información:** ¿Listo para implementar la personalización de usuarios conocidos en su propio sitio AEM? Empiece con el [Tutorial de Personalization de usuario conocido](./use-cases/known-user-personalization.md) para conocer el proceso de configuración completo.

## Pruebas A/B (experimentación)

Pruebe diferentes variaciones de contenido para determinar cuál ofrece el mejor rendimiento para sus objetivos empresariales. Adobe Target proporciona aleatoriamente diferentes variaciones a los visitantes y rastrea cuáles funcionan mejor. Esto se conoce comúnmente como _experimentación_.

### Artículo destacado de la página de inicio

La página principal de WKND ejecuta una prueba A/B activa con tres variaciones del artículo destacado _Camping in Western Australia_. Cada visitante se asigna aleatoriamente para ver una de estas variaciones:

| Página | Lo que va a ver | Cómo realizar las pruebas | Experiencia |
|------|-----------------|-------------|------------|
| [Hogar](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | Una de las tres variaciones de artículos destacados asignadas aleatoriamente en la sección &quot;Nuestros destacados&quot;: **&quot;Fuera de la cuadrícula: rutas épicas de camping por el oeste de Australia&quot;** o **&quot;Vagando por lo salvaje: aventuras de camping en el oeste de Australia&quot;** (o una tercera variación), cada una con imágenes y mensajes únicos para probar cuál resuena mejor | Visite la página de inicio en diferentes navegadores, utilice el modo incógnito/privado o borre las cookies para ver diferentes variaciones | ![Variaciones de prueba A/B](./assets/live-demo/ab-test-variations.png){width="200" zoomable="yes"} |

**Más información:** ¿Estás listo para implementar las pruebas A/B en tu propio sitio AEM? Empiece con el [Tutorial de experimentación (prueba A/B)](./use-cases/experimentation.md) para conocer el proceso de configuración completo.


## Próximos pasos

¿Está listo para implementar la personalización en su propio sitio de AEM? Empiece con [Información general de Personalization](./overview.md) para conocer el proceso de configuración completo.


