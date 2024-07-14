---
title: Personalization con el Compositor de experiencias visuales de Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer experiencias personalizadas con el Compositor de experiencias visuales (VEC) de Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 112
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---

# Personalization con el Compositor de experiencias visuales

En este capítulo, exploraremos la creación de experiencias con **Compositor de experiencias visuales** arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web desde Target.

## Información general del escenario

La página de inicio del sitio WKND muestra las actividades locales o lo mejor que se puede hacer por una ciudad en forma de diseños de tarjeta. Como especialista en marketing, se le ha asignado la tarea de modificar la página de inicio mediante la reorganización de los diseños de tarjeta para ver cómo afecta a la participación del usuario y cómo impulsa la conversión.

### Usuarios implicados

Para este ejercicio, deben participar los siguientes usuarios y, para realizar algunas tareas, puede necesitar acceso administrativo.

* **Productor de contenido/Editor de contenido** (Adobe Experience Manager)
* **Especialista en marketing** (Adobe Target / Equipo de optimización)

### Página de inicio del sitio WKND

AEM ![Escenario De Destino De La 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Requisitos previos

* AEM ****
   * AEM [instancia de publicación de la publicación de la](./implementation.md#getting-aem) que se ejecuta en 4503
   * [AEM Integración con Adobe Target mediante etiquetas de](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acceso a Adobe Experience Cloud de sus organizaciones - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con [Adobe Target](https://experiencecloud.adobe.com)

## Actividades del experto en marketing

1. El especialista en marketing crea una actividad de destinatario A/B dentro de Adobe Target.
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
   7. **La experiencia A** proporciona la página principal de WKND predeterminada y vamos a editar el diseño de contenido para la **experiencia B**.
      ![Experiencia B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Haga clic en uno de los contenedores de diseño de tarjeta (*Mejores asadores*) y seleccione la opción **Reorganizar**.
      ![Selección de contenedor](assets/personalization-use-case-3/container-selection.png)
   9. Haga clic en el contenedor que desee reorganizar y arrástrelo y suéltelo en la ubicación deseada. Vamos a reorganizar el contenedor *Best Roasters* de la primera fila de la primera columna a la primera fila de la tercera columna. Ahora el contenedor *Best Roasters* está junto al contenedor de *Exposiciones de Fotografía*.
      ![Intercambio de contenedor](assets/personalization-use-case-3/container-swap.png)
      **Después del intercambio**
      ![Contenedor intercambiado](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Del mismo modo, reorganice las posiciones de los otros contenedores de tarjetas.
      ![Contenedor intercambiado](assets/personalization-use-case-3/after-swap-all.png)
   11. También vamos a añadir un texto de encabezado debajo del componente de carrusel y encima del diseño de la tarjeta.
   12. Haga clic en el contenedor de carrusel y seleccione la opción **Insertar después > HTML** para agregar un HTML.
      ![Agregar texto](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Agregar texto](assets/personalization-use-case-3/after-changes.png)
   13. Haz clic en **Siguiente** para continuar con tu actividad.
   14. Seleccione el **Método de asignación de tráfico** como manual y asigne el 100% del tráfico a la **Experiencia B**.
      ![Tráfico de experiencia B](assets/personalization-use-case-2/traffic.png)
   15. Haga clic en **Siguiente**.
   16. Proporcione **métricas de objetivo** para su actividad y guarde y cierre la prueba A/B.
      ![Métrica de objetivo de prueba A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Proporcione un nombre (**WKND Home Page Refresh**) para su actividad y guarde los cambios.
   18. En la pantalla Detalles de la actividad, asegúrate de **Activar** tu actividad.
      ![Activar actividad](assets/personalization-use-case-3/save-activity.png)
   19. Vaya a la página principal de WKND (http://localhost:4503/content/wknd/en.html) y observe los cambios que hemos agregado a la actividad de la prueba A/B de actualización de la página principal de WKND.
      ![Página principal de WKND actualizada](assets/personalization-use-case-3/activity-result.png)
   20. Abra la consola del explorador e inspeccione la pestaña red para buscar la respuesta de destino para la actividad de la prueba A/B de actualización de la página principal de WKND.
      ![Actividad de red](assets/personalization-use-case-3/activity-result.png)

## Resumen

En este capítulo, un experto en marketing ha podido crear una experiencia con el Compositor de experiencias visuales arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web sin cambiar ningún código para ejecutar una prueba.
