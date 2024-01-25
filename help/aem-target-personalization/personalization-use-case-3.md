---
title: Personalización mediante el Compositor de experiencias visuales de Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer experiencias personalizadas con el Compositor de experiencias visuales (VEC) de Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 165
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 1%

---

# Personalización mediante el Compositor de experiencias visuales

En este capítulo, exploraremos la creación de experiencias con **Compositor de experiencias visuales** al arrastrar y soltar, intercambiar y modificar el diseño y el contenido de una página web desde Target.

## Información general del escenario

La página de inicio del sitio WKND muestra las actividades locales o lo mejor que se puede hacer por una ciudad en forma de diseños de tarjeta. Como especialista en marketing, se le ha asignado la tarea de modificar la página de inicio mediante la reorganización de los diseños de tarjeta para ver cómo afecta a la participación del usuario y cómo impulsa la conversión.

### Usuarios implicados

Para este ejercicio, deben participar los siguientes usuarios y, para realizar algunas tareas, puede necesitar acceso administrativo.

* **Productor de contenido/Editor de contenido** (Adobe Experience Manager)
* **Especialista en marketing** (Adobe Target / Equipo de optimización)

### Página de inicio del sitio WKND

![AEM Escenario 1 de Target de](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Requisitos previos

* **AEM**
   * [AEM Instancia de publicación de](./implementation.md#getting-aem) ejecución en 4503
   * [AEM Integrado con Adobe Target mediante Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acceso a Adobe Experience Cloud de sus organizaciones: `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con [Adobe Target](https://experiencecloud.adobe.com)

## Actividades del experto en marketing

1. El especialista en marketing crea una actividad de destinatario A/B dentro de Adobe Target.
   1. En la ventana de Adobe Target, vaya a **Actividades** pestaña.
   2. Clic **Crear actividad** y seleccione el tipo de actividad como **Prueba A/B**
      ![Adobe Target - Crear actividad](assets/personalization-use-case-2/create-ab-activity.png)
   3. Seleccione el **Web** y seleccione la opción **Compositor de experiencias visuales**.
   4. Introduzca el **URL de actividad** y haga clic en **Siguiente** para abrir el Compositor de experiencias visuales.
      ![Adobe Target - Crear actividad](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para **Compositor de experiencias visuales** para cargar, active **Permitir carga de scripts no seguros** en el explorador y vuelva a cargar la página.
      ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe la página de inicio del sitio WKND abierta en el editor del Compositor de experiencias visuales.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Experiencia A** proporciona la página principal de WKND predeterminada y vamos a editar el diseño de contenido para **Experiencia B**.
      ![Experiencia B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Haga clic en uno de los contenedores de diseño de tarjeta (*Mejores asadores*) y seleccione **Reorganizar** opción.
      ![Selección de contenedor](assets/personalization-use-case-3/container-selection.png)
   9. Haga clic en el contenedor que desee reorganizar y arrástrelo y suéltelo en la ubicación deseada. Vamos a reorganizar la *Mejores asadores* contenedor desde la primera fila, primera columna, hasta la primera fila, tercera columna. Ahora, la *Mejores asadores* el contenedor está junto a *Exposiciones fotográficas* contenedor.
      ![Intercambio de contenedor](assets/personalization-use-case-3/container-swap.png)
      **Después del intercambio**
      ![Contenedor intercambiado](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Del mismo modo, reorganice las posiciones de los otros contenedores de tarjetas.
      ![Contenedor intercambiado](assets/personalization-use-case-3/after-swap-all.png)
   11. También vamos a añadir un texto de encabezado debajo del componente de carrusel y encima del diseño de la tarjeta.
   12. Haga clic en el contenedor de carrusel y seleccione **Insertar después > HTML** opción para agregar un HTML.
      ![Añadir texto](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Añadir texto](assets/personalization-use-case-3/after-changes.png)
   13. Clic **Siguiente** para continuar con su actividad.
   14. Seleccione el **Método de asignación de tráfico** como manual y asignar el tráfico del 100 % a **Experiencia B**.
      ![Tráfico de experiencia B](assets/personalization-use-case-2/traffic.png)
   15. Haga clic en **Siguiente**.
   16. Proporcionar **Métricas de objetivo** para su actividad y guarde y cierre la prueba A/B.
      ![Métrica de objetivo de prueba A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Proporcione un nombre (**Actualizar página de inicio de WKND**) para su actividad y guarde los cambios.
   18. En la pantalla Detalles de la actividad, asegúrese de lo siguiente **Activar** su actividad de.
      ![Activar actividad](assets/personalization-use-case-3/save-activity.png)
   19. Vaya a la página principal de WKND (http://localhost:4503/content/wknd/en.html) y observe los cambios que hemos agregado a la actividad de la prueba A/B de actualización de la página principal de WKND.
      ![Página de inicio de WKND actualizada](assets/personalization-use-case-3/activity-result.png)
   20. Abra la consola del explorador e inspeccione la pestaña red para buscar la respuesta de destino para la actividad de la prueba A/B de actualización de la página principal de WKND.
      ![Actividad de red](assets/personalization-use-case-3/activity-result.png)

## Resumen

En este capítulo, un experto en marketing ha podido crear una experiencia con el Compositor de experiencias visuales arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web sin cambiar ningún código para ejecutar una prueba.
