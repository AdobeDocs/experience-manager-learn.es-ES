---
title: Personalización mediante el Compositor de experiencias visuales de Adobe Target
seo-title: Personalización mediante el Compositor de experiencias visuales (VEC) de Adobe Target
description: Un tutorial de extremo a extremo que muestra cómo crear y ofrecer experiencias personalizadas mediante el Compositor de experiencias visuales (VEC) de Adobe Target.
seo-description: Un tutorial de extremo a extremo que muestra cómo crear y ofrecer experiencias personalizadas mediante el Compositor de experiencias visuales (VEC) de Adobe Target.
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 2%

---


# Personalización mediante el Compositor de experiencias visuales

En este capítulo, exploraremos la creación de experiencias mediante el uso del **Compositor de experiencias visuales** arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web desde dentro de Destinatario.

## Información general del escenario

La página de inicio del sitio WKND muestra actividades locales o lo mejor para hacer en una ciudad en forma de diseños de tarjetas. Como especialista en mercadotecnia, se le ha asignado la tarea de modificar la página de inicio, reorganizando los diseños de tarjeta para ver cómo afecta a la participación del usuario y genera la conversión.

### Usuarios involucrados

Para este ejercicio, es necesario que participen los siguientes usuarios y que para realizar algunas tareas necesite acceso administrativo.

* **Content Producer/Editor**  de contenido (Adobe Experience Manager)
* **Especialista en mercadotecnia**  (Adobe Target / Equipo de optimización)

### Página de inicio del sitio WKND

![Escenario de Destinatario AEM 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Requisitos previos

* **AEM**
   * [AEM publicar ](./implementation.md#getting-aem) la instalación en 4503
   * [AEM integrado con Adobe Target mediante Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud suministrado con [Adobe Target](https://experiencecloud.adobe.com)

## Actividades de los especialistas en marketing

1. El especialista en marketing crea una actividad de destinatario A/B en Adobe Target.
   1. Desde la ventana de Adobe Target, vaya a la ficha **Actividades**.
   2. Haga clic en el botón **Crear Actividad** y seleccione el tipo de actividad como **Prueba A/B**

      ![Adobe Target - Crear Actividad](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleccione el canal **Web** y elija el **Compositor de experiencias visuales**.
   4. Introduzca la **URL de Actividad** y haga clic en **Siguiente** para abrir el Compositor de experiencias visuales.
      ![Adobe Target - Crear Actividad](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para que **Compositor de experiencias visuales** se cargue, habilite **Permitir la carga de secuencias de comandos no seguras** en el explorador y vuelva a cargar la página.
      ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe que la página de inicio del sitio WKND se abre en el editor del Compositor de experiencias visuales.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Experience** Manager proporciona la Página de inicio WKND predeterminada y vamos a editar el diseño de contenido para  **Experience B**.
      ![Experiencia B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Haga clic en uno de los contenedores de diseño de tarjetas (*Mejores rascadores*) y seleccione la opción **Reorganizar**.
      ![Selección de contenedor](assets/personalization-use-case-3/container-selection.png)
   9. Haga clic en el contenedor que desee reorganizar y arrástrelo y colóquelo en la ubicación deseada. Reorganicemos el contenedor *Mejores rascadores* de la primera fila de la primera columna a la primera fila de la tercera columna. Ahora, el contenedor *Mejores asadores* estará junto al contenedor *Exposiciones fotográficas*.
      ![Intercambio de contenedores](assets/personalization-use-case-3/container-swap.png)

      **Después del intercambio**
      ![Contenedor intercambiado](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Del mismo modo, reorganice las posiciones para los demás contenedores de tarjeta.
      ![Contenedor intercambiado](assets/personalization-use-case-3/after-swap-all.png)
   11. Añadamos también un texto de encabezado debajo del componente carrusel y encima del diseño de la tarjeta.
   12. Haga clic en el contenedor de carrusel y seleccione la opción **Insertar después > HTML** para agregar HTML.
      ![Añadir texto](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Añadir texto](assets/personalization-use-case-3/after-changes.png)
   13. Haga clic en **Siguiente** para continuar con la actividad.
   14. Seleccione el **Método de asignación de tráfico** como manual y asigne el 100% de tráfico a **Experiencia B**.
      ![Tráfico de la experiencia B](assets/personalization-use-case-2/traffic.png)
   15. Haga clic en **Siguiente**. 
   16. Proporcione **Métricas de objetivo** para su Actividad y guarde y cierre la prueba A/B.
      ![Métrica de objetivo de prueba A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Proporcione un nombre (**Actualización de Página de inicio WKND**) para la Actividad y guarde los cambios.
   18. En la pantalla de detalles de la Actividad, asegúrese de **Activar** su actividad.
      ![Activar Actividad](assets/personalization-use-case-3/save-activity.png)
   19. Vaya a la Página de inicio WKND (http://localhost:4503/content/wknd/en.html) y observe los cambios que hemos agregado a la actividad de prueba A/B de actualización de Página de inicio WKND.
      ![Actualización de Página de inicio WKND](assets/personalization-use-case-3/activity-result.png)
   20. Abra la consola del navegador e inspeccione la ficha de red para buscar la respuesta de destinatario para la actividad de prueba A/B de actualización de Página de inicio WKND.
      ![Actividad de red](assets/personalization-use-case-3/activity-result.png)

## Resumen

En este capítulo, un especialista en marketing pudo crear una experiencia con el Compositor de experiencias visuales arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web sin cambiar ningún código para ejecutar una prueba.
