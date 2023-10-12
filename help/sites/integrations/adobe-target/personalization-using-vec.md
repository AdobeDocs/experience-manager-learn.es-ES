---
title: Personalización mediante el Compositor de experiencias visuales
description: Obtenga información sobre cómo crear una actividad de Adobe Target mediante el Compositor de experiencias visuales.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 2%

---

# Personalización mediante el Compositor de experiencias visuales {#personalization-vec}

Obtenga información sobre cómo crear una actividad de destinatario de prueba A/B mediante el Compositor de experiencias visuales (VEC).

## Requisitos previos

AEM Para utilizar el VEC en un sitio web de, se debe completar la siguiente configuración:

1. [Añadir Adobe Target AEM al sitio web de la](./add-target-launch-extension.md)
1. [Déclencheur de una llamada de Adobe Target desde Launch](./load-and-fire-target.md)

## Información general del escenario

La página de inicio del sitio WKND muestra las actividades locales o las mejores cosas que hacer en una ciudad en forma de tarjetas informativas. Como especialista en marketing, se le ha asignado la tarea de modificar la página de inicio realizando cambios de texto en el teaser de la sección de aventura y comprendiendo cómo mejora la conversión.

## Pasos para crear una prueba A/B con el Compositor de experiencias visuales (VEC)

1. Inicie sesión en [Adobe Experience Cloud](https://experience.adobe.com/), pulse en __Target__, vaya al __Actividades__ pestaña

   + Si no ve __Target__ en el panel del Experience Cloud, asegúrese de que está seleccionada la organización de Adobe correcta en el conmutador de organización en la parte superior derecha y de que se ha concedido acceso al usuario a Target en [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Clic **Crear actividad** y luego elija **Prueba A/B** actividad

   ![Actividad A/B](assets/ab-target-activity.png)

1. Seleccione el **Compositor de experiencias visuales** , proporcione la URL de la actividad y haga clic en **Siguiente**

   ![URL de actividad](assets/ab-test-url.png)

1. El Compositor de experiencias visuales muestra dos pestañas a la izquierda después de crear una actividad: *Experiencia A* y *Experiencia B*. Seleccione una experiencia de la lista. Puede añadir nuevas experiencias a la lista utilizando el **Añadir experiencia** botón.

   ![Experiencia A](assets/experience.png)

1. Seleccione una imagen o un texto de la página para empezar a realizar modificaciones o utilice el editor de código para elegir un elemento de HTML.

   ![Elemento](assets/select-element.png)

1. Cambiar el texto de *Camping en Australia Occidental* hasta *Aventuras de Australia*. Una lista de los cambios añadidos a una experiencia se muestra en Modificaciones. Puede hacer clic en el elemento modificado y editarlo para ver su selector de CSS y el contenido nuevo que se le ha agregado.

   ![Aventuras](assets/adventures.png)

1. Cambiar nombre *Experiencia A* hasta *Aventura*
1. Del mismo modo, actualice el texto en *Experiencia B* de *Camping en Australia Occidental* hasta *Explora el desierto australiano*.

   ![Explorar](assets/explore.png)

1. Clic **Siguiente** para pasar a Segmentación y mantengamos una asignación de tráfico manual de 50-50 entre las dos experiencias.

   ![Direccionamiento](assets/targeting.png)

1. Para Objetivos y configuración, elija la Fuente de informes como Adobe Target y seleccione la métrica Objetivo como Conversión con una acción de vista de página.

   ![Objetivos](assets/goals.png)

1. Asigne un nombre a la actividad y haga clic en Guardar.
1. Active la actividad guardada para aplicar los cambios.

   ![Objetivos](assets/activate.png)

1. Abra la página del sitio (URL de actividad del paso 3) en una nueva pestaña y podrá ver cualquiera de las experiencias (Aventura o Exploración) desde su actividad de prueba A/B.

   ![Objetivos](assets/publish.png)

## Resumen

En este capítulo, un experto en marketing ha podido crear una experiencia utilizando el Compositor de experiencias visuales arrastrando y soltando, intercambiando y modificando el diseño y el contenido de una página web sin cambiar ningún código para ejecutar una prueba.

## Vínculos de soporte

+ [Adobe Experience Cloud Debugger: Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger: Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
