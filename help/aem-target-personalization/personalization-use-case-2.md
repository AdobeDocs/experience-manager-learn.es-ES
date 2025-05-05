---
title: Personalization con Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer experiencias personalizadas con Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 114
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 1%

---

# Personalization de experiencias de página web completa con Adobe Target

En el capítulo anterior, aprendimos a crear una actividad basada en la ubicación geográfica dentro de Adobe Target AEM utilizando contenido creado como Fragmentos de experiencias y exportado desde Ofertas de HTML de Adobe

AEM En este capítulo, exploraremos la creación de actividades para redirigir las páginas del sitio alojadas en la página a una nueva página mediante Adobe Target.

## Información general del escenario

El sitio WKND ha rediseñado su página de inicio y le gustaría redirigir a los visitantes de su página principal actual a la nueva página principal. Al mismo tiempo, comprenda también cómo la página de inicio rediseñada ayuda a mejorar la participación del usuario y los ingresos. Como experto en marketing, se le ha asignado la tarea de crear una actividad para redirigir a los visitantes a la nueva página principal. Exploremos la página de inicio del sitio WKND y aprendamos a crear una actividad con Adobe Target.

### Usuarios implicados

Para este ejercicio, deben participar los siguientes usuarios y, para realizar algunas tareas, puede necesitar acceso administrativo.

* **Productor de contenido/Editor de contenido** (Adobe Experience Manager)
* **Especialista en marketing** (Adobe Target / Equipo de optimización)

### Página de inicio del sitio WKND

AEM ![Escenario De Destino De La 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Requisitos previos

* AEM **&#x200B;**
   * AEM [instancia de autor y publicación de](./implementation.md#getting-aem) que se ejecuta en localhost 4502 y 4503 respectivamente.
   * [AEM Integración con Adobe Target mediante etiquetas de](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acceso a Adobe Experience Cloud de sus organizaciones - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Target](https://experiencecloud.adobe.com)

## Actividades del editor de contenido

1. AEM El especialista en marketing inicia la conversación de rediseño de la página principal de WKND con el editor de contenido de la y detalla los requisitos.
   * ***Requisito***: rediseñar la página de inicio del sitio WKND con diseño basado en tarjetas.
2. AEM En función de los requisitos, el Editor de contenido crea una nueva página de inicio del sitio WKND con un diseño basado en tarjetas y publica la nueva página principal.

## Actividades del experto en marketing

1. El especialista en marketing crea una actividad de destinatario A/B con la oferta de redireccionamiento como una experiencia y el tráfico del sitio web asignado a la nueva página de inicio con el objetivo de éxito y las métricas añadidas.
   1. En la ventana de Adobe Target, ve a la pestaña **Actividades**.
   2. Haga clic en el botón **Crear actividad** y seleccione el tipo de actividad como **Prueba A/B**

      ![Adobe Target - Crear actividad](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleccione el canal **Web** y elija **Compositor de experiencias visuales**.
   4. Escriba la **URL de actividad** y haga clic en **Siguiente** para abrir el Compositor de experiencias visuales.

      ![Adobe Target - Crear actividad](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para que **Compositor de experiencias visuales** se cargue, habilita **Permitir la carga de scripts no seguros** en el explorador y vuelve a cargar la página.

      ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe la página de inicio del sitio WKND abierta en el editor del Compositor de experiencias visuales.

      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Pase el ratón sobre la **experiencia B** y seleccione Ver otras opciones.

      ![Experiencia B](assets/personalization-use-case-2/redirect-url.png)
   8. Seleccione la opción **Redirigir a URL** e introduzca la URL a la nueva página principal de WKND. (http://localhost:4503/content/wknd/en1.html)

      ![Experiencia B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Guarde** sus cambios y continúe con los siguientes pasos de la Creación de actividades.
   10. Seleccione el **Método de asignación de tráfico** como manual y asigne el 100% del tráfico a la **Experiencia B**.

       ![Tráfico de experiencia B](assets/personalization-use-case-2/traffic.png)
   11. Haga clic en **Siguiente**.
   12. Proporcione **métricas de objetivo** para su actividad y guarde y cierre la prueba A/B.

       ![Métrica de objetivo de prueba A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Proporcione un nombre (**Rediseño de la página principal de WKND**) para su actividad y guarde los cambios.
   14. En la pantalla Detalles de la actividad, asegúrate de **Activar** tu actividad.

       ![Activar actividad](assets/personalization-use-case-2/ab-activate.png)
   15. Vaya a la página principal de WKND (http://localhost:4503/content/wknd/en.html) y se le redirigirá a la página principal del sitio WKND rediseñada (http://localhost:4503/content/wknd/en1.html).

      ![Se rediseñó la página principal de WKND](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Resumen

En este capítulo, un especialista en marketing ha podido crear una actividad para redirigir las páginas del sitio alojadas en una nueva página mediante Adobe Target. Se ha creado una actividad de marketing que se ha creado para crear una actividad de redireccionamiento de páginas de sitio que están alojadas en una nueva página mediante AEM.
