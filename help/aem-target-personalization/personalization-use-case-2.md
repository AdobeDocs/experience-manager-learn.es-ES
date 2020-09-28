---
title: Personalización mediante Adobe Target
seo-title: Personalización mediante Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer una experiencia personalizada con Adobe Target.
seo-description: Un tutorial completo que muestra cómo crear y ofrecer una experiencia personalizada con Adobe Target.
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 2%

---


# Personalización de experiencias de página web completa con Adobe Target

En el capítulo anterior, hemos aprendido a crear una actividad basada en la ubicación geográfica dentro de Adobe Target mediante el uso de contenido creado como fragmentos de experiencia y exportado desde AEM como Ofertas HTML.

En este capítulo analizaremos la creación de actividades para redirigir las páginas del sitio que se alojan en AEM a una nueva página mediante Adobe Target.

## Información general del escenario

El sitio WKND rediseñó su página de inicio y quisiera redirigir sus visitantes de página de inicio actuales a la nueva página de inicio. Al mismo tiempo, comprenda también cómo la página de inicio rediseñada ayuda a mejorar la participación y los ingresos de los usuarios. Como especialista en marketing, se le ha asignado la tarea para crear una actividad que redirija los visitantes a la nueva página de inicio. Exploremos la página de inicio del sitio WKND y aprendamos a crear una actividad con Adobe Target.

### Usuarios involucrados

Para este ejercicio, es necesario que participen los siguientes usuarios y que para realizar algunas tareas necesite acceso administrativo.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Especialista en mercadotecnia** (Adobe Target / Equipo de optimización)

### Página de inicio del sitio WKND

![Escenario de Destinatario AEM 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Requisitos previos

* **AEM**
   * [AEM la instancia](./implementation.md#getting-aem) de creación y publicación se ejecuta en localhost 4502 y 4503 respectivamente.
   * [AEM integrado con Adobe Target mediante Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Target](https://experiencecloud.adobe.com)

## Actividades del Editor de contenido

1. El especialista en marketing inicia el análisis de rediseño de la Página de inicio WKND con AEM editor de contenido y detalla los requisitos.
   * ***Requisito*** : Rediseñe la Página de inicio del sitio WKND con el diseño basado en tarjetas.
2. En función de los requisitos, AEM editor de contenido crea una nueva página de inicio del sitio WKND con un diseño basado en tarjetas y publica la nueva página de inicio.

## Actividades de los especialistas en marketing

1. Marketer crea una actividad de destinatario A/B con la oferta de redireccionamiento como experiencia y el tráfico del sitio web del 100 % asignado a la nueva página de inicio con el objetivo de éxito y las métricas agregadas.
   1. Desde la ventana de Adobe Target, vaya a la ficha **Actividades** .
   2. Haga clic en el botón **Crear Actividad** y seleccione el tipo de actividad como Prueba **A/B**

      ![Adobe Target - Crear Actividad](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleccione el canal **Web** y elija el Compositor de experiencias **visuales**.
   4. Introduzca la dirección URL **de la** Actividad y haga clic en **Siguiente** para abrir el Compositor de experiencias visuales.
      ![Adobe Target - Crear Actividad](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para que el Compositor **de experiencias** visuales se cargue, habilite **Permitir cargar secuencias de comandos** no seguras en el explorador y vuelva a cargar la página.
      ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe que la página de inicio del sitio WKND se abre en el editor del Compositor de experiencias visuales.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Pase el ratón sobre la **experiencia B** y seleccione vista de otras opciones.
      ![Experiencia B](assets/personalization-use-case-2/redirect-url.png)
   8. Seleccione la opción **Redireccionar a URL** e introduzca la URL de la nueva Página de inicio WKND. (http://localhost:4503/content/wknd/en1.html)
      ![Experiencia B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Guarde** los cambios y continúe con los siguientes pasos de Creación de Actividades.
   10. Seleccione el método **de asignación de** tráfico como manual y asigne un 100 % de tráfico a la **experiencia B**.
      ![Tráfico de la experiencia B](assets/personalization-use-case-2/traffic.png)
   11. Haga clic en **Siguiente**. 
   12. Proporcione métricas **de** objetivo para su Actividad y guarde y cierre la prueba A/B.
      ![Métrica de objetivo de prueba A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Proporcione un nombre (**WKND Página de inicio Redesign**) para la Actividad y guarde los cambios.
   14. En la pantalla de detalles de la Actividad, asegúrese de **activar** la actividad.
      ![Activar Actividad](assets/personalization-use-case-2/ab-activate.png)
   15. Vaya a la Página de inicio WKND (http://localhost:4503/content/wknd/en.html) y será redirigido a la Página de inicio del sitio WKND rediseñada (http://localhost:4503/content/wknd/en1.html).
      ![Página de inicio WKND rediseñada](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Resumen

En este capítulo, un especialista en mercadotecnia pudo crear una actividad para redireccionar las páginas del sitio alojadas en AEM a una nueva página mediante Adobe Target.
