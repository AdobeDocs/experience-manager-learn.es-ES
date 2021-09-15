---
title: Personalización de la experiencia de página web completa
description: Aprenda a crear una actividad de Target para redirigir las páginas del sitio web AEM a nuevas páginas mediante Adobe Target.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---

# Personalización de la experiencia de página web completa {#personalization-fpe}

Aprenda a crear una actividad para redirigir las páginas del sitio alojadas en AEM a una nueva página mediante Adobe Target.

## Requisitos previos

Para personalizar las páginas completas de un sitio web AEM, se debe completar la siguiente configuración:

1. [Agregar Adobe Target al sitio web AEM](./add-target-launch-extension.md)
1. [Déclencheur de una llamada de Adobe Target desde Launch](./load-and-fire-target.md)

## Información general del escenario

El sitio WKND rediseñó su página principal y le gustaría redirigir a los visitantes de su página principal actual a la nueva página principal. Al mismo tiempo, comprenda también cómo la página de inicio rediseñada ayuda a mejorar la participación del usuario y los ingresos. Como especialista en marketing, se le ha asignado la tarea de crear una actividad para redirigir a los visitantes a la nueva página principal. Exploremos la página de inicio del sitio WKND y aprendamos a crear una actividad con Adobe Target.

## Pasos para crear una prueba A/B con el Compositor de experiencias visuales (VEC)

1. Inicie sesión en Adobe Target y vaya a la pestaña Actividades
1. Haga clic en el botón **Crear actividad** y, a continuación, elija **Prueba A/B** actividad

   ![Actividad A/B](assets/ab-target-activity.png)

1. Seleccione la opción **Compositor de experiencias visuales**, proporcione la URL de actividad y haga clic en **Siguiente**

   ![URL de actividad](assets/ab-test-url.png)

1. El Compositor de experiencias visuales muestra dos fichas en el lado izquierdo después de crear una nueva actividad: *Experiencia A* y *Experiencia B*. Seleccione una experiencia de la lista. Puede añadir nuevas experiencias a la lista mediante el botón **Añadir experiencia**.

   ![Opciones de experiencia](assets/experience-options.png)

1. Vea las opciones disponibles para la Experiencia A y, a continuación, seleccione la opción **Redireccionar a URL** y proporcione una URL para la nueva página de inicio del sitio WKND.

   ![Dirección URL de redireccionamiento](assets/redirect-url.png)

1. Cambie el nombre de *Experiencia A* a *Nueva página principal de WKND* y *Experiencia B* a *Página principal de WKND*

   ![Aventuras](assets/new-experiences.png)

1. Haga clic en **Siguiente** para pasar a Segmentación y mantener una asignación de tráfico manual de 50-50 entre las dos experiencias.

   ![Direccionamiento](assets/targeting.png)

1. En Objetivos y configuración, elija la fuente de informes como Adobe Target y seleccione la métrica Objetivo como Conversión con una acción de vista de página.

   ![Objetivos](assets/goals.png)

1. Proporcione un nombre a la actividad y haga clic en Guardar.
1. Active la actividad guardada para activar los cambios.

   ![Objetivos](assets/activate.png)

1. Abra la página del sitio (URL de actividad del paso 3) en una nueva pestaña y debería poder ver cualquiera de las experiencias (página principal de WKND o página principal de New WKND) desde nuestra actividad de prueba A/B. `us/en.html` redirige a  `us/home.html`.

   ![Objetivos](assets/redirect-test.png)

## Resumen

Como especialista en marketing, pudo crear una actividad para redirigir las páginas del sitio alojadas en AEM a una nueva página mediante Adobe Target.

## Compatibilidad con vínculos

* [Adobe Experience Cloud Debugger: Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger: Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
