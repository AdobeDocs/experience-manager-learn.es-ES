---
title: Personalización de la experiencia de página web completa
description: AEM Obtenga información sobre cómo crear una actividad de Target para redirigir las páginas de su sitio web de a nuevas páginas mediante Adobe Target.
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 89
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Personalización de la experiencia de página web completa {#personalization-fpe}

AEM Obtenga información sobre cómo crear una actividad para redireccionar las páginas del sitio alojadas en una nueva página mediante el uso de Adobe Target.

## Requisitos previos

AEM Para personalizar las páginas completas de un sitio web de, se debe completar la siguiente configuración:

1. [Añadir Adobe Target AEM al sitio web de la](./add-target-launch-extension.md)
1. [Déclencheur de una llamada de Adobe Target desde etiquetas](./load-and-fire-target.md)

## Información general del escenario

El sitio WKND ha rediseñado su página de inicio y le gustaría redirigir a los visitantes de su página principal actual a la nueva página principal. Al mismo tiempo, comprenda también cómo la página de inicio rediseñada ayuda a mejorar la participación del usuario y los ingresos. Como experto en marketing, se le ha asignado la tarea de crear una actividad para redirigir a los visitantes a la nueva página principal. Exploremos la página de inicio del sitio WKND y aprendamos a crear una actividad con Adobe Target.

## Pasos para crear una prueba A/B con el Compositor de experiencias visuales (VEC)

1. Inicie sesión en Adobe Target y vaya a la pestaña Actividades
1. Clic **Crear actividad** y luego elija **Prueba A/B** actividad

   ![Actividad A/B](assets/ab-target-activity.png)

1. Seleccione el **Compositor de experiencias visuales** , proporcione la URL de la actividad y haga clic en **Siguiente**

   ![URL de actividad](assets/ab-test-url.png)

1. El Compositor de experiencias visuales muestra dos pestañas a la izquierda después de crear una actividad: *Experiencia A* y *Experiencia B*. Seleccione una experiencia de la lista. Puede añadir nuevas experiencias a la lista utilizando el **Añadir experiencia** botón.

   ![Opciones de experiencia](assets/experience-options.png)

1. Vea las opciones disponibles para la Experiencia A y, a continuación, seleccione la **Redirigir a URL** y proporcione una dirección URL para la nueva página principal del sitio WKND.

   ![URL de redireccionamiento](assets/redirect-url.png)

1. Cambiar nombre *Experiencia A* hasta *Nueva página de inicio de WKND* y *Experiencia B* hasta *Página principal de WKND*

   ![Aventuras](assets/new-experiences.png)

1. Clic **Siguiente** para pasar a Segmentación y mantener una asignación de tráfico manual de 50-50 entre las dos experiencias.

   ![Segmentación](assets/targeting.png)

1. Para Objetivos y configuración, elija la Fuente de informes como Adobe Target y seleccione la métrica Objetivo como Conversión con una acción de vista de página.

   ![Metas](assets/goals.png)

1. Asigne un nombre a la actividad y haga clic en Guardar.
1. Active la actividad guardada para aplicar los cambios.

   ![Metas](assets/activate.png)

1. Abra la página del sitio (URL de actividad del paso 3) en una nueva pestaña y podrá ver cualquiera de las experiencias (página principal de WKND o nueva página principal de WKND) desde la actividad de prueba A/B. `us/en.html` redirige a `us/home.html`.

   ![Metas](assets/redirect-test.png)

## Resumen

AEM Como especialista en marketing, ha podido crear una actividad para redirigir las páginas del sitio alojadas en la página a una nueva página mediante Adobe Target.

## Vínculos de soporte

* [Adobe Experience Cloud Debugger: Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
