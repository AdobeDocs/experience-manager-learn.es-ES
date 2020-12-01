---
title: Personalización mediante fragmentos de experiencia AEM y Adobe Target
seo-title: Personalización mediante fragmentos de experiencias de Adobe Experience Manager (AEM) y Adobe Target
description: Un tutorial completo que muestra cómo crear y ofrecer una experiencia personalizada con fragmentos de experiencia de Adobe Experience Manager y Adobe Target.
seo-description: Un tutorial completo que muestra cómo crear y ofrecer una experiencia personalizada con fragmentos de experiencia de Adobe Experience Manager y Adobe Target.
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '1729'
ht-degree: 1%

---


# Personalización mediante fragmentos de experiencia AEM y Adobe Target

Con la capacidad de exportar fragmentos de experiencia AEM a Adobe Target como ofertas HTML, puede combinar la facilidad de uso y la potencia de AEM con las potentes funciones de Inteligencia automatizada (AI) y Aprendizaje automático (ML) en Destinatario para probar y personalizar experiencias a escala.

AEM reúne todo su contenido y recursos en una ubicación central para impulsar su estrategia de personalización. AEM le permite crear fácilmente contenido para equipos de escritorio, tabletas y dispositivos móviles en una ubicación sin necesidad de escribir código. No es necesario crear páginas para cada dispositivo: AEM ajusta automáticamente cada experiencia con el contenido.

Destinatario le permite ofrecer experiencias personalizadas a escala basada en una combinación de enfoques de aprendizaje automático basados en reglas y dirigidos por AI que incorporan variables de comportamiento, contextuales y sin conexión.  Con Destinatario, puede configurar y ejecutar fácilmente actividades A/B y multivariadas (MVT) para determinar las mejores ofertas, contenido y experiencias.

Los fragmentos de experiencia representan un gran paso adelante para vincular a los creadores de contenido con los especialistas en marketing que dirigen los resultados comerciales mediante Destinatario.

## Información general del escenario

El sitio WKND planea anunciar un **Desafío SkateFest** en toda América a través de su sitio web y desea que los usuarios del sitio se registren para la audición realizada en cada estado. Como especialista en mercadotecnia, se le ha asignado la tarea de ejecutar una campaña en la página de inicio del sitio WKND, con mensajes de pancartas relevantes para la ubicación de los usuarios y un vínculo a la página de detalles del evento. Exploremos la página de inicio del sitio WKND y aprendamos a crear y ofrecer una experiencia personalizada para un usuario en función de su ubicación actual.

### Usuarios involucrados

Para este ejercicio, es necesario que participen los siguientes usuarios y que para realizar algunas tareas necesite acceso administrativo.

* **Content Producer / Content Editor**  (Adobe Experience Manager)
* **Especialista en mercadotecnia**  (Adobe Target / Equipo de optimización)

### Requisitos previos

* **AEM**
   * [AEM creación y publicación de ](./implementation.md#getting-aem) instancias en localhost 4502 y 4503 respectivamente.
* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Target](https://experiencecloud.adobe.com)

### Página de inicio del sitio WKND

![Escenario de Destinatario AEM 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. Marketer inicia el análisis de campaña de WKND SkateFest con AEM editor de contenido y detalla los requisitos.
   * ***Requisito***: Promocione la campaña de WKND SkateFest en la página de inicio del sitio WKND con contenido personalizado para visitantes de cada estado de Estados Unidos. Añada un nuevo bloque de contenido debajo del carrusel de Página de inicio que contenga una imagen de fondo, texto y un botón.
      * **Imagen** de fondo: La imagen debe ser relevante para el estado desde el cual el usuario visita la página del sitio WKND.
      * **Texto**: &quot;Regístrese para los Audition&quot;
      * **Botón**: &quot;Detalles de Evento&quot; que apuntan a la página de WKND SkateFest
      * **Página** de WKND SkateFest: una nueva página con detalles de evento, que incluye el lugar de la audiencia, la fecha y la hora.
1. En función de los requisitos, AEM editor de contenido crea un fragmento de experiencia para el bloque de contenido y lo exporta a Adobe Target como Oferta. Para ofrecer contenido personalizado para todos los estados de Estados Unidos, el autor del contenido puede crear una variación maestra de fragmento de experiencia y, a continuación, crear otras 50 variaciones, una para cada estado. El contenido de cada variación de estado con imágenes y texto relevantes se puede editar manualmente. Al crear un fragmento de experiencia, los editores de contenido pueden acceder rápidamente a todos los recursos disponibles en AEM Assets mediante la opción Buscador de recursos. Cuando se exporta un fragmento de experiencia a Adobe Target, todas sus variaciones también se insertan en Adobe Target como Ofertas.

1. Después de exportar Fragmento de experiencia de AEM a Adobe Target como Ofertas, los especialistas en marketing pueden crear Actividades en Destinatario mediante estas Ofertas. Según la campaña SkateFest del sitio WKND, los especialistas en marketing deben crear y ofrecer una experiencia personalizada a los visitantes del sitio WKND de cada estado. Para crear una actividad de segmentación de experiencias, el especialista en marketing debe identificar las audiencias. Para nuestra campaña WKND SkateFest, necesitamos crear 50 audiencias independientes, basadas en su ubicación desde la cual están visitando el sitio web de WKND.
   * [Las ](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) audiencias definen el destinatario de la actividad y se utilizan en cualquier lugar donde esté disponible la segmentación. Las audiencias de destinatario son un conjunto definido de criterios de visitante. Las ofertas se pueden dirigir a audiencias (o segmentos) específicas. Solo los visitantes que pertenecen a esa audiencia ven la experiencia que va dirigida a ellos.  Por ejemplo, puede enviar una oferta a una audiencia formada por visitantes que utilizan un navegador concreto o una ubicación geográfica específica.
   * Una [Oferta](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) es el contenido que se muestra en las páginas web durante campañas o actividades. Al probar las páginas web, se mide el éxito de cada experiencia con diferentes ofertas en las ubicaciones. Una Oferta puede contener diferentes tipos de contenido, incluidos:
      * Imagen
      * Texto
      * **HTML**
         * *Se utilizarán Ofertas HTML para la Actividad de este escenario*
      * Vínculo
      * Botón

## Actividades del Editor de contenido

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Publique el fragmento de experiencias antes de exportarlo a Adobe Target.

## Actividades de los especialistas en marketing

### Crear una Audiencia con Geo-Targeting {#marketer-audience}

1. Navegue a sus organizaciones [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experience ecloud.adobe.com)
1. Inicie sesión con su Adobe ID y asegúrese de que se encuentra en la organización correcta.
1. En el conmutador de soluciones, haga clic en **Destinatario** y, a continuación, **inicie** Adobe Target.

   ![Experience Cloud: Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Vaya a la ficha **Ofertas** y busque ofertas &quot;WKND&quot;. Debería poder ver la lista de las variaciones de fragmentos de experiencia, exportadas desde AEM como Ofertas HTML. Cada Oferta corresponde a un estado. Por ejemplo: *WKND SkateFest California* es la oferta que se sirve en un visitante del sitio WKND de California.

   ![Experience Cloud: Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. En la navegación principal, haga clic en **Audiencias**.

   Un especialista en mercadotecnia necesita crear 50 audiencias independientes para los visitantes del sitio WKND provenientes de cada estado en los Estados Unidos de América.

1. Para crear una audiencia, haga clic en el botón **Crear Audiencia** y proporcione un nombre para la audiencia.

   **Formato Nombre de audiencia: WKND-\&lt;>state *\>***

   ![Experience Cloud: Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Haga clic en **Añadir regla > Geografía**.
1. Haga clic en **Seleccionar** y, a continuación, seleccione una de las siguientes opciones:
   * País
   * **Estado** *(Seleccionar estado para la Campaña SkateFest del sitio WKND)*
   * Ciudad
   * Código postal
   * Latitud
   * Longitud
   * DMA
   * Operador de telefonía móvil

   **Geografía** : utilice audiencias para los usuarios de destinatario según su ubicación geográfica, incluido su país, estado/provincia, ciudad, código postal, DMA o operador de telefonía móvil. Los parámetros de geolocalización permiten el destinatario de actividades y experiencias en función de la geografía de los visitantes. Estos datos se envían con cada solicitud de Destinatario y se basan en la dirección IP del visitante. Seleccione estos parámetros como cualquier valor de objetivo.

   >[!NOTE]
   >La dirección IP de un visitante se pasa con una solicitud de mbox, una vez por visita (sesión), para resolver los parámetros de objetivo geográfico para ese visitante.

1. Seleccione el operador como **coincide con**, proporcione un valor apropiado (Por ejemplo: California) y **Guarde** los cambios. En nuestro caso, proporcione el nombre del estado.

   ![Adobe Target: regla geográfica](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >Puede tener varias reglas asignadas a una audiencia.

1. Repita los pasos 6 a 9 para crear audiencias para los demás estados.

   ![Adobe Target: Audiencias WKND](assets/personalization-use-case-1/adobe-target-audiences-50.png)

En este punto, hemos creado con éxito audiencias para todos los visitantes del sitio WKND en diferentes estados de los Estados Unidos de América y también tenemos la oferta HTML correspondiente para cada estado. Así que ahora creemos una actividad de Segmentación de experiencias para el destinatario de la audiencia con una oferta correspondiente para la Página de inicio del sitio WKND.

### Crear una Actividad con Geo-Targeting

1. Desde la ventana de Adobe Target, vaya a la ficha **Actividades**.
1. Haga clic en **Crear Actividad** y seleccione el tipo de actividad **Segmentación de experiencias**.
1. Seleccione el canal **Web** y elija el **Compositor de experiencias visuales**.
1. Introduzca la **URL de Actividad** y haga clic en **Siguiente** para abrir el Compositor de experiencias visuales.

   URL de publicación de Página de inicio del sitio WKND: http://localhost:4503/content/wknd/en.html

   ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/target-activity.png)

1. Para que **Compositor de experiencias visuales** se cargue, habilite **Permitir la carga de secuencias de comandos no seguras** en el explorador y vuelva a cargar la página.

   ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Observe que la página de inicio del sitio WKND se abre en el editor del Compositor de experiencias visuales.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Para agregar una audiencia a su VEC, haga clic en **Añadir Segmentación de experiencias** en Audiencias y seleccione la audiencia WKND-California y haga clic en **Siguiente**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Haga clic en la página del sitio WKND dentro de VEC, seleccione el elemento HTML para agregar la oferta para la audiencia WKND-California, elija la opción **Reemplazar con** y luego seleccione la **Oferta HTML**.

   ![Actividad de segmentación de experiencias](assets/personalization-use-case-1/vec-selecting-div.png)

1. Seleccione la oferta HTML **WKND SkateFest California** para la audiencia **WKND-California** en la oferta seleccione IU y haga clic en **Listo**.
1. Ahora debería poder ver la Oferta HTML **WKND SkateFest California** agregada a la página del sitio WKND para la audiencia WKND-California.
1. Repita los pasos del 7 al 10 para agregar Segmentación de experiencias a los demás estados y elija la Oferta HTML correspondiente.
1. Haga clic en **Siguiente** para continuar y podrá ver una asignación de Audiencias a experiencias.
1. Haga clic en **Siguiente** para moverse a Objetivos y configuración.
1. Elija el origen del sistema de informes e identifique un objetivo principal para su actividad. Para nuestro escenario, seleccionemos el origen de Sistema de informes como **Adobe Target**, midiendo la actividad como **Conversión**, la acción como si se viera una página y la dirección URL que apunta a la página Detalles del SkateFest de WKND.

   ![Objetivo y objetivo: Destinatario](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >También puede elegir Adobe Analytics como fuente de sistema de informes.

1. Pase el ratón sobre el nombre de la actividad actual y puede cambiarlo a **WKND SkateFest - USA** y luego **Guardar y cerrar** los cambios.
1. En la pantalla de detalles de la Actividad, asegúrese de **Activar** su actividad.

   ![Activar Actividad](assets/personalization-use-case-1/activate-activity.png)

1. Su Campaña de WKND SkateFest ahora está activa para todos los visitantes del sitio WKND.
1. Vaya a la [Página de inicio del sitio WKND](http://localhost:4503/content/wknd/en.html) y debería poder ver la Oferta de WKND SkateFest en función de su ubicación geográfica (*estado: California*).

   ![Control de calidad de la actividad](assets/personalization-use-case-1/wknd-california.png)

### Control de calidad de Actividad de destinatario

1. En la ficha **Detalles de Actividad > Información general**, haga clic en el botón **Control de calidad de la Actividad** y podrá obtener el vínculo de control de calidad directo a todas sus experiencias.

   ![Control de calidad de la actividad](assets/personalization-use-case-1/activity-qa.png)

1. Vaya a la [Página de inicio del sitio WKND](http://localhost:4503/content/wknd/en.html) y debería poder ver la Oferta WKND SkateFest en función de su ubicación geográfica (estado).
1. Vea el siguiente vídeo para comprender cómo se entrega una oferta a la página, cómo personalizar los tokens de respuesta y cómo realizar una comprobación de calidad.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Resumen

En este capítulo, un editor de contenido pudo crear todo el contenido para admitir la campaña WKND SkateFest en Adobe Experience Manager y exportarla a Adobe Target como Ofertas HTML para crear Segmentación de experiencias, en función de la ubicación geográfica de los usuarios.
