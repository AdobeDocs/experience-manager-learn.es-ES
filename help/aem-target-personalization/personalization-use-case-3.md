---
title: Personalización mediante el Compositor de experiencias visuales de Adobe Target
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: Un tutorial completo que muestra cómo crear y ofrecer experiencias personalizadas con el Compositor de experiencias visuales (VEC) de Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 2%

---


# Personalización mediante el Compositor de experiencias visuales

En este capítulo, exploraremos la creación de experiencias con el **Compositor de experiencias visuales** arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web desde Target.

## Información general del escenario

La página de inicio del sitio WKND muestra las actividades locales o lo mejor para hacer en una ciudad en forma de diseños de tarjeta. Como especialista en marketing, se le ha asignado la tarea de modificar la página principal, reorganizando los diseños de tarjeta para ver cómo afecta a la participación del usuario y la conversión.

### Usuarios implicados

Para este ejercicio, es necesario involucrar a los siguientes usuarios y para realizar algunas tareas es posible que necesite acceso administrativo.

* **Productor de contenido/Editor de contenido**  (Adobe Experience Manager)
* **Especialista en marketing**  (Adobe Target/Equipo de optimización)

### Página principal del sitio WKND

![Escenario de AEM objetivo 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Requisitos previos

* **AEM**
   * [AEM publicar la ](./implementation.md#getting-aem) instancia el 4503
   * [AEM integrado con Adobe Target mediante Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con [Adobe Target](https://experiencecloud.adobe.com)

## Actividades de marketing

1. El especialista en marketing crea una actividad de destino A/B dentro de Adobe Target.
   1. En la ventana de Adobe Target, vaya a la pestaña **Actividades**.
   2. Haga clic en el botón **Crear actividad** y seleccione el tipo de actividad como **Prueba A/B**

      ![Adobe Target: Crear actividad](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleccione el canal **Web** y elija el **Compositor de experiencias visuales**.
   4. Introduzca la **URL de actividad** y haga clic en **Siguiente** para abrir el Compositor de experiencias visuales.
      ![Adobe Target: Crear actividad](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para que **Compositor de experiencias visuales** se cargue, habilite **Permitir la carga de scripts no seguros** en el explorador y vuelva a cargar la página.
      ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe que se abre la página de inicio del sitio WKND en el editor del Compositor de experiencias visuales.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Experience** Manager proporciona la página principal WKND predeterminada y vamos a editar el diseño de contenido para la  **Experiencia B**.
      ![Experiencia B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Haga clic en uno de los contenedores de diseño de la tarjeta (*Mejores presentadores*) y seleccione la opción **Reorganizar**.
      ![Selección de contenedor](assets/personalization-use-case-3/container-selection.png)
   9. Haga clic en el contenedor que desee reorganizar y arrástrelo hasta la ubicación deseada. Reorganicemos el contenedor *Best Roasters* de la primera fila de la primera columna a la primera fila de la tercera columna. Ahora el contenedor *Mejores pilas* estará junto al contenedor de *Exposiciones fotográficas*.
      ![Intercambio de contenedores](assets/personalization-use-case-3/container-swap.png)

      **Después de intercambiar**
      ![Contenedor intercambiado](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Del mismo modo, reorganice las posiciones para los demás contenedores de tarjetas.
      ![Contenedor intercambiado](assets/personalization-use-case-3/after-swap-all.png)
   11. Añadamos también un texto de encabezado debajo del componente carrusel y encima del diseño de la tarjeta.
   12. Haga clic en el contenedor de carrusel y seleccione la opción **Insertar después > HTML** para añadir HTML.
      ![Añadir texto](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Añadir texto](assets/personalization-use-case-3/after-changes.png)
   13. Haga clic en **Siguiente** para continuar con su actividad.
   14. Seleccione el **Método de asignación de tráfico** como manual y asigne el 100% de tráfico a **Experiencia B**.
      ![Tráfico de la experiencia B](assets/personalization-use-case-2/traffic.png)
   15. Haga clic en **Siguiente**. 
   16. Proporcione **Métricas de objetivo** para su actividad y guarde y cierre la prueba A/B.
      ![Métrica de objetivo de prueba A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Proporcione un nombre (**WKND Home Page Refresh**) para su actividad y guarde los cambios.
   18. En la pantalla Detalles de la actividad, asegúrese de **Activar** su actividad.
      ![Activar actividad](assets/personalization-use-case-3/save-activity.png)
   19. Vaya a la página principal de WKND (http://localhost:4503/content/wknd/en.html) y verá los cambios que hemos añadido a la actividad Prueba A/B de actualización de la página principal de WKND.
      ![Página principal de WKND actualizada](assets/personalization-use-case-3/activity-result.png)
   20. Abra la consola del explorador e inspeccione la pestaña red para buscar la respuesta de destino para la actividad Prueba A/B de actualización de la página principal de WKND.
      ![Actividad de red](assets/personalization-use-case-3/activity-result.png)

## Resumen

En este capítulo, un especialista en marketing pudo crear una experiencia con el Compositor de experiencias visuales arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web sin cambiar ningún código para ejecutar una prueba.
