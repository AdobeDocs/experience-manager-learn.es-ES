---
title: Personalización mediante el Compositor de experiencias visuales
description: Obtenga información sobre cómo crear una actividad de Adobe Target mediante el Compositor de experiencias visuales.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 1%

---

# Personalización mediante el Compositor de experiencias visuales {#personalization-vec}

Obtenga información sobre cómo crear una actividad de Target de prueba A/B mediante el Compositor de experiencias visuales (VEC).

## Requisitos previos

Para utilizar VEC en un sitio web AEM, se debe completar la siguiente configuración:

1. [Agregar Adobe Target al sitio web AEM](./add-target-launch-extension.md)
1. [Déclencheur de una llamada de Adobe Target desde Launch](./load-and-fire-target.md)

## Información general del escenario

La página de inicio del sitio WKND muestra actividades locales o lo mejor para hacer en una ciudad en forma de tarjetas informativas. Como especialista en marketing, se le ha asignado la tarea de modificar la página principal, realizando cambios de texto en el teaser de secciones de aventura y comprender cómo mejora la conversión.

## Pasos para crear una prueba A/B con el Compositor de experiencias visuales (VEC)

1. Inicie sesión en [Adobe Experience Cloud](https://experience.adobe.com/), toque __Target__, vaya a la __Actividades__ ficha

   + Si no ve __Target__ en el panel del Experience Cloud, asegúrese de que la organización de Adobe correcta esté seleccionada en el conmutador de organización en la parte superior derecha y de que el usuario haya obtenido acceso a Target en [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Haga clic en **Crear actividad** y, a continuación, elija **Prueba A/B** actividad

   ![Actividad A/B](assets/ab-target-activity.png)

1. Seleccione el **Compositor de experiencias visuales** , proporcione la dirección URL de actividad y haga clic en **Siguiente**

   ![URL de actividad](assets/ab-test-url.png)

1. El Compositor de experiencias visuales muestra dos fichas en el lado izquierdo después de crear una nueva actividad: *Experiencia A* y *Experiencia B*. Seleccione una experiencia de la lista. Puede añadir nuevas experiencias a la lista, utilizando la variable **Añadir experiencia** botón.

   ![Experiencia A](assets/experience.png)

1. Seleccione una imagen o texto en la página para comenzar a realizar modificaciones o utilice el editor de código para elegir y HTML el elemento .

   ![Elemento](assets/select-element.png)

1. Cambiar el texto de *Camping en Australia Occidental* a *Aventuras de Australia*. Una lista de los cambios agregados a una experiencia se muestra en Modificaciones. Puede hacer clic y editar el elemento modificado para ver su selector CSS y el nuevo contenido que se le ha agregado.

   ![Aventuras](assets/adventures.png)

1. Cambiar nombre *Experiencia A* a *Aventura*
1. Del mismo modo, actualice el texto en *Experiencia B* from *Camping en Australia Occidental* a *Explorar el salvaje australiano*.

   ![Explorar](assets/explore.png)

1. Haga clic en **Siguiente** para pasar a Segmentación y mantengamos una asignación de tráfico manual del 50 al 50 entre las dos experiencias.

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

+ [Adobe Experience Cloud Debugger: Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger: Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
