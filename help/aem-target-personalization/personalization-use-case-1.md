---
title: Personalization AEM con fragmentos de experiencias de y Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer experiencias personalizadas con Fragmentos de experiencias de Adobe Experience Manager y Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
duration: 1088
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1663'
ht-degree: 0%

---

# Personalization AEM con fragmentos de experiencias de y Adobe Target

AEM Con la capacidad de exportar fragmentos de experiencias de la red de distribución de datos en Adobe Target como ofertas de HTML AEM, puede combinar la facilidad de uso y la potencia de los fragmentos de experiencias con las potentes capacidades de inteligencia automatizada (AI) y aprendizaje automático (ML) de Target para probar y personalizar experiencias a escala.

AEM El contenido y los recursos se reúnen en una ubicación central para impulsar la estrategia de personalización. AEM le permite crear fácilmente contenido para equipos de escritorio, tabletas y dispositivos móviles en una sola ubicación y sin tener que escribir código. AEM No es necesario crear páginas para cada dispositivo: ajusta automáticamente todas las experiencias con el uso de su contenido.

Target permite ofrecer experiencias personalizadas a escala en función de una combinación de enfoques de aprendizaje automático basados en reglas y controlados por IA que incorporan variables de comportamiento, contextuales y sin conexión.  Target le permite configurar y ejecutar fácilmente actividades A/B y multivariable (MVT) para determinar las mejores ofertas, contenidos y experiencias.

Los fragmentos de experiencia representan un enorme paso adelante en el vínculo entre los creadores de contenido y los especialistas en marketing que dirigen mediante Target los resultados empresariales.

## Información general del escenario

El sitio WKND planea anunciar un **Desafío SkateFest** en todo Estados Unidos a través de su sitio web y quisiera que los usuarios de su sitio se inscriban para la audición realizada en cada estado. Como experto en marketing, se le ha asignado la tarea de ejecutar una campaña en la página de inicio del sitio WKND, con titulares y mensajes relevantes para la ubicación de los usuarios y un vínculo a la página de detalles del evento. Exploremos la página de inicio del sitio WKND y aprendamos a crear y ofrecer una experiencia personalizada para un usuario en función de su ubicación actual.

### Usuarios implicados

Para este ejercicio, deben participar los siguientes usuarios y, para realizar algunas tareas, puede necesitar acceso administrativo.

* **Productor de contenido/Editor de contenido** (Adobe Experience Manager)
* **Especialista en marketing** (Adobe Target / Equipo de optimización)

### Requisitos previos

* AEM **&#x200B;**
   * AEM [instancia de autor y publicación de](./implementation.md#getting-aem) que se ejecuta en localhost 4502 y 4503 respectivamente.
* **Experience Cloud**
   * Acceso a Adobe Experience Cloud de sus organizaciones - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Target](https://experiencecloud.adobe.com)

### Página de inicio del sitio WKND

AEM ![Escenario De Destino De La 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. AEM El especialista en marketing inicia la conversación de la campaña WKND SkateFest con el Editor de contenido de la y detalla los requisitos.
   * ***Requisito***: promocione la campaña WKND SkateFest en la página de inicio del sitio WKND con contenido personalizado para visitantes de cada estado de Estados Unidos. Agregue un nuevo bloque de contenido debajo del carrusel de la página principal que contenga una imagen de fondo, un texto y un botón.
      * **Imagen de fondo**: la imagen debe ser relevante para el estado desde el que el usuario está visitando la página del sitio WKND.
      * **Texto**: &quot;Regístrese para los Audition&quot;
      * **Botón**: &quot;Detalles del evento&quot; que señala a la página del WKND SkateFest
      * **Página del WKND SkateFest**: una nueva página con detalles del evento, incluyendo el lugar de audición, la fecha y la hora.
1. AEM En función de los requisitos, el Editor de contenido crea un fragmento de experiencia para el bloque de contenido y lo exporta a Adobe Target como una oferta. Para ofrecer contenido personalizado para todos los estados de Estados Unidos, el autor del contenido puede crear una variación principal del Fragmento de experiencia y luego crear otras 50 variaciones, una para cada estado. El contenido de cada variación de estado con imágenes y texto relevantes se puede editar manualmente. Al crear un fragmento de experiencia, los editores de contenido pueden acceder rápidamente a todos los recursos disponibles en AEM Assets mediante la opción Buscador de recursos. Cuando se exporta un fragmento de experiencia a Adobe Target, todas sus variaciones también se insertan en Adobe Target como Ofertas.

1. AEM Después de exportar el fragmento de experiencia de la a Adobe Target como ofertas, los especialistas en marketing pueden crear actividades en Target utilizando estas ofertas. En función de la campaña SkateFest del sitio WKND, el experto en marketing debe crear y ofrecer una experiencia personalizada a los visitantes del sitio WKND de cada estado. Para crear una actividad de segmentación de experiencias, el experto en marketing debe identificar las audiencias. Para nuestra campaña WKND SkateFest, necesitamos crear 50 audiencias independientes, basadas en su ubicación desde la que visitan el sitio web de WKND.
   * [Audiencias](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=es#section_3F32DA46BDF947878DD79DBB97040D01) definen el público objetivo de su actividad y se utilizan en cualquier lugar donde el direccionamiento esté disponible. Las audiencias de destino son un conjunto definido de criterios de visitante. Las ofertas se pueden dirigir a audiencias específicas (o segmentos). Solo los visitantes que pertenecen a esa audiencia ven la experiencia segmentada para ellos.  Por ejemplo, puede enviar una oferta a una audiencia compuesta por visitantes que utilizan un explorador concreto o desde una ubicación geográfica específica.
   * Una [oferta](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=es#section_973D4CC4CEB44711BBB9A21BF74B89E9) es el contenido que se muestra en las páginas web durante las campañas o actividades. Al probar las páginas web, se mide el éxito de cada experiencia con diferentes ofertas en las ubicaciones. Una oferta puede contener diferentes tipos de contenido, entre ellos:
      * Imagen
      * Texto
      * **HTML**
         * *Se utilizan ofertas de HTML para la actividad de este escenario*
      * Vínculo
      * Botón

## Actividades del editor de contenido

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Publish el fragmento de experiencia antes de exportarlo a Adobe Target.

## Actividades del experto en marketing

### Crear una audiencia con segmentación geográfica {#marketer-audience}

1. Vaya a sus organizaciones [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. Inicie sesión con su Adobe ID y asegúrese de que se encuentra en la organización correcta.
1. En el conmutador de soluciones, haga clic en **Target** y, a continuación, en **launch** Adobe Target.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Vaya a la pestaña **Ofertas** y busque ofertas &quot;WKND&quot;. AEM Debería poder ver la lista de variaciones de Fragmentos de experiencias, exportadas desde como Ofertas de HTML. Cada oferta corresponde a un estado. Por ejemplo, *WKND SkateFest California* es la oferta que se sirve a un visitante del sitio WKND de California.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. En la navegación principal, haga clic en **Audiencias**.

   Un especialista en marketing debe crear 50 audiencias independientes para los visitantes del sitio WKND que provienen de cada estado de los Estados Unidos de América.

1. Para crear una audiencia, haga clic en el botón **Crear audiencia** y proporcione un nombre para la audiencia.

   **Formato de nombre de audiencia: WKND-\&lt;*estado*\>**

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Haga clic en **Agregar regla > Geografía**.
1. Haga clic en **Seleccionar** y, a continuación, seleccione una de las siguientes opciones:
   * País
   * **Estado** *(Seleccionar estado para la campaña WKND Site SkateFest)*
   * Ciudad
   * Código postal
   * Latitud
   * Longitud
   * DMA
   * Operador de telefonía móvil

   **Geografía**: use audiencias para segmentar usuarios según su ubicación geográfica, incluidos país, estado/provincia, ciudad, código postal, DMA y operador de telefonía móvil. Los parámetros de geolocalización le permiten segmentar las actividades y experiencias en función de la ubicación geográfica de los visitantes. Estos datos se envían con cada solicitud de Target y se basan en la dirección IP del visitante. Seleccione estos parámetros igual que cualquier otro valor de segmentación.

   >[!NOTE]
   >La dirección IP de un visitante se pasa con una solicitud de mbox, una vez por visita (sesión), para resolver los parámetros de targeting geográfico de ese visitante.

1. Seleccione el operador como **coincide**, proporcione un valor apropiado (por ejemplo, California) y **guarde** sus cambios. En nuestro caso, proporcione el nombre del estado.

   ![Regla geográfica de Adobe Target](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >Puede tener varias reglas asignadas a una audiencia.

1. Repita los pasos del 6 al 9 para crear audiencias para los demás estados.

   ![Audiencias de Adobe Target- WKND](assets/personalization-use-case-1/adobe-target-audiences-50.png)

En este punto, hemos creado audiencias con éxito para todos los visitantes del sitio WKND en diferentes estados de los Estados Unidos de América y también tenemos la oferta de HTML correspondiente para cada estado. Ahora vamos a crear una actividad de segmentación de experiencias para dirigirse a la audiencia con una oferta correspondiente para la página de inicio del sitio WKND.

### Crear una actividad con segmentación geográfica

1. En la ventana de Adobe Target, ve a la pestaña **Actividades**.
1. Haga clic en **Crear actividad** y seleccione el tipo de actividad **Segmentación de experiencias**.
1. Seleccione el canal **Web** y elija **Compositor de experiencias visuales**.
1. Escriba la **URL de actividad** y haga clic en **Siguiente** para abrir el Compositor de experiencias visuales.

   URL de Publish de la página de inicio del sitio WKND: http://localhost:4503/content/wknd/en.html

   ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/target-activity.png)

1. Para que **Compositor de experiencias visuales** se cargue, habilita **Permitir la carga de scripts no seguros** en el explorador y vuelve a cargar la página.

   ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Observe la página de inicio del sitio WKND abierta en el editor del Compositor de experiencias visuales.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Para agregar una audiencia a su VEC, haga clic en **Agregar segmentación de experiencias** en Audiencias, seleccione la audiencia de WKND-California y haga clic en **Siguiente**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Haga clic en la página del sitio WKND del VEC, seleccione el elemento HTML al que desee agregar la oferta para la audiencia de WKND-California, elija **Reemplazar con** y, a continuación, seleccione la **oferta HTML**.

   ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/vec-selecting-div.png)

1. Seleccione la oferta del HTML **WKND SkateFest California** para la audiencia **WKND-California** de la oferta, seleccione la interfaz de usuario y haga clic en **Listo**.
1. Ahora debería poder ver la oferta de HTML **WKND SkateFest California** añadida a la página de su sitio WKND para la audiencia de WKND-California.
1. Repita los pasos 7-10 para agregar Segmentación de experiencias para los demás estados y elija la Oferta de HTML correspondiente.
1. Haga clic en **Siguiente** para continuar y verá una asignación de audiencias a experiencias.
1. Haz clic en **Siguiente** para ir a Objetivos y configuración.
1. Elija su fuente de informes e identifique el objetivo principal de su actividad. Para nuestro escenario, vamos a seleccionar el Source de informes como **Adobe Target**, medir la actividad como **Conversión**, actuar como si viera una página y URL que apunte a la página Detalles del WKND SkateFest.

   ![Objetivo y direccionamiento: destino](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >También puede elegir Adobe Analytics como fuente de informes.

1. Pase el ratón sobre el nombre de la actividad actual y renómbrela a **WKND SkateFest - USA**, y a continuación **Guardar y cerrar** sus cambios.
1. En la pantalla Detalles de la actividad, asegúrate de **Activar** tu actividad.

   ![Activar actividad](assets/personalization-use-case-1/activate-activity.png)

1. La campaña del WKND SkateFest ahora está activa para todos los visitantes del sitio WKND.
1. Vaya a la [página de inicio del sitio WKND](http://localhost:4503/content/wknd/en.html) y debería poder ver la oferta de WKND SkateFest en función de su ubicación geográfica (*estado: California*).

   ![Control de calidad de la actividad](assets/personalization-use-case-1/wknd-california.png)

### Control de calidad de actividad de Target

1. En la ficha **Detalles de actividad > Información general**, haga clic en el botón **Control de calidad de la actividad** y obtendrá el vínculo de control de calidad directo a todas sus experiencias.

   ![Control de calidad de la actividad](assets/personalization-use-case-1/activity-qa.png)

1. Vaya a la [página de inicio del sitio WKND](http://localhost:4503/content/wknd/en.html) y debería poder ver la oferta de WKND SkateFest en función de su ubicación geográfica (estado).
1. Vea el siguiente vídeo para comprender cómo se envía una oferta a su página, cómo personalizar los tokens de respuesta y para realizar una comprobación de calidad.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Resumen

En este capítulo, un editor de contenido pudo crear todo el contenido para admitir la campaña WKND SkateFest en Adobe Experience Manager y exportarla a Adobe Target como Ofertas de HTML para crear una segmentación de experiencias basada en la geolocalización de los usuarios.
