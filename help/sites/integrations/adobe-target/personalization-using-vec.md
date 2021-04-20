---
title: Personalización mediante el Compositor de experiencias visuales
description: Obtenga información sobre cómo crear una actividad de Adobe Target mediante el Compositor de experiencias visuales.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 1%

---


# Personalización mediante el Compositor de experiencias visuales {#personalization-vec}

Obtenga información sobre cómo crear una actividad de Target de prueba A/B mediante el Compositor de experiencias visuales (VEC).

## Requisitos previos

Para utilizar VEC en un sitio web de AEM, se debe completar la siguiente configuración:

1. [Añadir Adobe Target al sitio web de AEM](./add-target-launch-extension.md)
1. [Activación de una llamada de Adobe Target desde Launch](./load-and-fire-target.md)

## Información general del escenario

La página de inicio del sitio WKND muestra actividades locales o lo mejor para hacer en una ciudad en forma de tarjetas informativas. Como especialista en marketing, se le ha asignado la tarea de modificar la página principal, realizando cambios de texto en el teaser de secciones de aventura y comprender cómo mejora la conversión.

## Pasos para crear una prueba A/B con el Compositor de experiencias visuales (VEC)

1. Inicie sesión en [Adobe Experience Cloud](https://experience.adobe.com/), pulse __Target__, vaya a la pestaña __Actividades__

   + Si no ve __Target__ en el panel de Experience Cloud, asegúrese de que la organización correcta de Adobe esté seleccionada en el conmutador de organización en la esquina superior derecha y de que el usuario haya obtenido acceso a Target en [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Haga clic en el botón **Crear actividad** y, a continuación, elija **Prueba A/B** actividad

   ![Actividad A/B](assets/ab-target-activity.png)

1. Seleccione la opción **Compositor de experiencias visuales**, proporcione la URL de actividad y haga clic en **Siguiente**

   ![URL de actividad](assets/ab-test-url.png)

1. El Compositor de experiencias visuales muestra dos fichas en el lado izquierdo después de crear una nueva actividad: *Experiencia A* y *Experiencia B*. Seleccione una experiencia de la lista. Puede añadir nuevas experiencias a la lista mediante el botón **Añadir experiencia**.

   ![Experiencia A](assets/experience.png)

1. Seleccione una imagen o texto en la página para comenzar a realizar modificaciones o utilice el editor de código para elegir un elemento HTML.

   ![Elemento](assets/select-element.png)

1. Cambie el texto de *Camping in Western Australia* a *Adventures of Australia*. Se mostrará una lista de los cambios agregados a una experiencia en Modificaciones. Puede hacer clic y editar el elemento modificado para ver su selector CSS y el nuevo contenido que se le ha agregado.

   ![Aventuras](assets/adventures.png)

1. Cambiar el nombre de *Experiencia A* a *Aventura*
1. Del mismo modo, actualice el texto de *Experiencia B* de *Camping in Western Australia* a *Explorar el salvaje australiano*.

   ![Explorar](assets/explore.png)

1. Haga clic en **Siguiente** para pasar a Segmentación y conserve una asignación de tráfico manual de 50-50 entre las dos experiencias.

   ![Direccionamiento](assets/targeting.png)

1. En Objetivos y configuración, elija la fuente de informes como Adobe Target y seleccione la métrica Objetivo como Conversión con una acción de vista de página.

   ![Objetivos](assets/goals.png)

1. Proporcione un nombre a la actividad y haga clic en Guardar.
1. Active la actividad guardada para activar los cambios.

   ![Objetivos](assets/activate.png)

1. Abra la página del sitio (URL de actividad del paso 3) en una nueva pestaña y debería poder ver cualquiera de las experiencias (aventura o exploración) desde nuestra actividad de prueba A/B.

   ![Objetivos](assets/publish.png)

## Resumen

En este capítulo, un especialista en marketing pudo crear una experiencia con el Compositor de experiencias visuales arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web sin cambiar ningún código para ejecutar una prueba.

## Compatibilidad con vínculos

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger: Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
