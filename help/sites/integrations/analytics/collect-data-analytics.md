---
title: Recopilación de datos de página con Adobe Analytics
description: Utilice la capa de datos del cliente de Adobe impulsada por eventos para recopilar datos sobre la actividad del usuario en un sitio web creado con Adobe Experience Manager. Aprenda a utilizar las reglas en Experience Platform Launch para detectar estos eventos y enviar datos a un grupo de informes de Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2371'
ht-degree: 3%

---

# Recopilación de datos de página con Adobe Analytics

Aprenda a utilizar las funciones integradas del [Adobe de la capa de datos del cliente con AEM componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es) para recopilar datos sobre una página en Adobe Experience Manager Sites. [Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) y [Extensión de Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) se utilizan para crear reglas para enviar datos de página a Adobe Analytics.

## Qué va a generar

![Seguimiento de datos de página](assets/collect-data-analytics/analytics-page-data-tracking.png)

En este tutorial, debe almacenar en déclencheur una regla de Launch basada en un evento de la capa de datos del cliente de Adobe, agregar condiciones para cuándo se debe activar la regla y enviar la variable **Nombre de la página** y **Plantilla de página** de una página AEM a Adobe Analytics.

### Objetivos {#objective}

1. Cree una regla basada en eventos en Launch en función de los cambios realizados en la capa de datos
1. Asignación de propiedades de capa de datos de página a elementos de datos en Launch
1. Recopilar datos de página y enviarlos a Adobe Analytics con la señalización de vista de página

## Requisitos previos

Se requieren las siguientes opciones:

* **Experience Platform Launch** Propiedad
* **Adobe Analytics** ID del grupo de informes de prueba/desarrollo y servidor de seguimiento. Consulte la siguiente documentación para [crear un nuevo grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) extensión del explorador. Capturas de pantalla de este tutorial tomadas del navegador Chrome.
* (Opcional) AEM sitio con la variable [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Este tutorial utilizará el sitio de cara al público [https://wknd.site/us/en.html](https://wknd.site/us/en.html) pero puede utilizar su propio sitio.

>[!NOTE]
>
> ¿Necesita ayuda para integrar Launch y su sitio AEM? [Ver esta serie de vídeo](../experience-platform-launch/overview.md).

## Cambiar entornos de Launch para el sitio WKND

[https://wknd.site](https://wknd.site) es un sitio público creado en función de [un proyecto de código abierto](https://github.com/adobe/aem-guides-wknd) diseñado como referencia y [tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es) para implementaciones de AEM.

En lugar de configurar un entorno AEM e instalar la base de código WKND, puede utilizar el depurador de Experience Platform para **switch** live [https://wknd.site/](https://wknd.site/) a *your* Propiedad Launch. Por supuesto, puede usar su propio sitio AEM si ya tiene la variable [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Inicie sesión en el Experience Platform Launch y [crear una propiedad de Launch](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (si aún no lo ha hecho).
1. Asegúrese de que haya un lanzamiento inicial [Se ha creado la biblioteca](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) y promocionados a un lanzamiento [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. Copie el código de incrustación de Launch del entorno en el que se ha publicado la biblioteca.

   ![Copiar código incrustado de Launch](assets/collect-data-analytics/launch-environment-copy.png)

1. En el explorador, abra una pestaña nueva y vaya a [https://wknd.site/](https://wknd.site/)
1. Abra la extensión del explorador de Experience Platform Debugger .

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Vaya a **Launch** > **Configuración** y **Códigos incrustados insertados** reemplace el código de incrustación de Launch existente por *your* incrustar código copiado del paso 3.

   ![Reemplazar código incrustado](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Habilitar **Registro de consola** y **Bloqueo** Debugger en la ficha WKND.

   ![Registro de consola](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Comprobar la capa de datos del cliente de Adobe en el sitio WKND

La variable [Proyecto de referencia WKND](https://github.com/adobe/aem-guides-wknd) está construido con AEM componentes principales y tiene el [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) de forma predeterminada. A continuación, compruebe que la capa de datos del cliente de Adobe esté habilitada.

1. Vaya a [https://wknd.site](https://wknd.site).
1. Abra las herramientas para desarrolladores del explorador y vaya a la **Consola**. Ejecute el siguiente comando:

   ```js
   adobeDataLayer.getState();
   ```

   Esto devuelve el estado actual de la capa de datos del cliente de Adobe.

   ![Estado de la capa de datos del Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Expanda la respuesta e inspeccione la variable `page` entrada. Debería ver un esquema de datos como el siguiente:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Utilizaremos propiedades estándar derivadas de la variable [Esquema de página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page),  `dc:title`, `xdm:language` y `xdm:template` de la capa de datos para enviar datos de página a Adobe Analytics.

   >[!NOTE]
   >
   > No vea el `adobeDataLayer` objeto javascript? Asegúrese de que la variable [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) del sitio.

## Crear una regla de página cargada

La capa de datos del cliente de Adobe es una **evento** capa de datos controlada. Cuando el AEM **Página** la capa de datos se carga, se déclencheur un evento `cmp:show`. Cree una regla que se active según la variable `cmp:show` evento.

1. Vaya al Experience Platform Launch y a la propiedad web integrada con el sitio AEM.
1. Vaya a la **Reglas** en la interfaz de usuario de Launch y, a continuación, haga clic en **Crear nueva regla**.

   ![Crear regla](assets/collect-data-analytics/analytics-create-rule.png)

1. Asigne un nombre a la regla **Página cargada**.
1. Haga clic en **Eventos** **Agregar** para abrir el **Configuración de eventos** asistente.
1. En **Tipo de evento** select **Código personalizado**.

   ![Asigne un nombre a la regla y añada el evento de Custom Code](assets/collect-data-analytics/custom-code-event.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Launch Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   El fragmento de código anterior agregará un detector de eventos por [inserción de una función](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) en la capa de datos. Cuando la variable `cmp:show` se activa el evento `pageShownEventHandler` llamada a . En esta función se añaden algunas comprobaciones de coherencia y se añade una nueva `event` se construye con el último [estado de la capa de datos](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para el componente que activó el evento.

   Después de eso `trigger(event)` se llama. `trigger()` es un nombre reservado en Launch y &quot;déclencheur&quot; de la regla de Launch. Pasamos el `event` objeto como parámetro que, a su vez, está expuesto por otro nombre reservado en Launch denominado `event`. Los elementos de datos de Launch ahora pueden hacer referencia a varias propiedades como esta: `event.component['someKey']`.

1. Guarde los cambios.
1. Siguiente en **Acciones** click **Agregar** para abrir el **Configuración de la acción** asistente.
1. En **Tipo de acción** elija **Código personalizado**.

   ![Tipo de acción de código personalizado](assets/collect-data-analytics/action-custom-code.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   La variable `event` se pasa desde el `trigger()` método llamado en el evento personalizado. `component` es la página actual derivada de la capa de datos `getState` en el evento personalizado . Recuerde desde la versión anterior de [Esquema de página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) expuestos por la capa de datos para ver las distintas claves expuestas fuera de la caja.

1. Guarde los cambios y ejecute un [versión](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) en Launch para promocionar el código a la variable [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) en su sitio AEM.

   >[!NOTE]
   >
   > Puede ser muy útil usar la variable [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) para cambiar el código incrustado a un **Desarrollo** entorno.

1. Vaya a su sitio AEM y abra las herramientas para desarrolladores para ver la consola. Actualice la página y verá que se han registrado los mensajes de la consola:

   ![Mensajes de la consola Carga de página](assets/collect-data-analytics/page-show-event-console.png)

## Crear elementos de datos

A continuación, cree varios elementos de datos para capturar valores diferentes de la capa de datos del cliente de Adobe. Como se ha visto en el ejercicio anterior, hemos visto que es posible acceder a las propiedades de la capa de datos directamente a través del código personalizado. La ventaja de utilizar elementos de datos es que se pueden reutilizar en todas las reglas de Launch.

Recuerde desde la versión anterior de [Esquema de página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) expuesto por la capa de datos:

Los elementos de datos se asignan al `@type`, `dc:title`y `xdm:template` propiedades.

### Tipo de recurso de componente

1. Vaya al Experience Platform Launch y a la propiedad web integrada con el sitio AEM.
1. Vaya a la **Elementos de datos** y haga clic en **Crear nuevo elemento de datos**.
1. Para **Nombre** enter **Tipo de recurso de componente**.
1. Para **Tipo de elemento de datos** select **Código personalizado**.

   ![Tipo de recurso de componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Guarde los cambios.

   >[!NOTE]
   >
   > Recuerde que la variable `event` El objeto está disponible y tiene ámbitos basados en el evento que activó la variable **Regla** en Launch. El valor de un elemento de datos no se establece hasta que el elemento de datos *referenciado* dentro de una regla. Por lo tanto, es seguro utilizar este elemento de datos dentro de una regla como la **Página cargada** regla creada en el paso anterior *but* no sería seguro usar en otros contextos.

### Nombre de página

1. Haga clic en **Añadir elemento de datos**.
1. Para **Nombre** enter **Nombre de la página**.
1. Para **Tipo de elemento de datos** select **Código personalizado**.
1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Guarde los cambios.

### Plantilla de la página

1. Haga clic en **Añadir elemento de datos**.
1. Para **Nombre** enter **Plantilla de página**.
1. Para **Tipo de elemento de datos** select **Código personalizado**.
1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Guarde los cambios.

1. Ahora debe tener tres elementos de datos como parte de la regla:

   ![Elementos de datos en la regla](assets/collect-data-analytics/data-elements-page-rule.png)

## Añadir la extensión de Analytics

A continuación, agregue la extensión de Analytics a la propiedad de Launch. ¡Necesitamos enviar estos datos a algún lugar!

1. Vaya al Experience Platform Launch y a la propiedad web integrada con el sitio AEM.
1. Vaya a **Extensiones** > **Catálogo**
1. Busque la variable **Adobe Analytics** extensión y haga clic en **Instalar**

   ![Extensión de Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. En **Administración de biblioteca** > **Grupos de informes**, introduzca los ID de los grupos de informes que desee utilizar con cada entorno de Launch.

   ![Especifique los ID de los grupos de informes](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Está bien utilizar un grupo de informes para todos los entornos en este tutorial, pero en la vida real le recomendamos utilizar grupos de informes independientes, como se muestra en la imagen siguiente

   >[!TIP]
   >
   >Se recomienda usar la variable *Opción Administrar la biblioteca por mí* ya que la configuración de Administración de biblioteca facilita mucho mantener la variable `AppMeasurement.js` biblioteca actualizada.

1. Marque la casilla para habilitar **Usar Activity Map**.

   ![Habilitar Activity Map de uso](assets/track-clicked-component/analytic-track-click.png)

1. En **General** > **Servidor de seguimiento**, introduzca su servidor de seguimiento, por ejemplo: `tmd.sc.omtrdc.net`. Introduzca el servidor de seguimiento SSL si su sitio es compatible con `https://`

   ![Especifique los servidores de seguimiento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Haga clic en **Guardar** para guardar los cambios.

## Añadir una condición a la regla Page Loaded

A continuación, actualice la variable **Página cargada** para usar la variable **Tipo de recurso de componente** elemento de datos para garantizar que la regla solo se active cuando la variable `cmp:show` es para la variable **Página**. Otros componentes pueden activar el `cmp:show` , por ejemplo, el componente Carrusel se activará cuando cambien las diapositivas. Por lo tanto, es importante añadir una condición para esta regla.

1. En la interfaz de usuario de Launch, vaya a la **Página cargada** regla creada anteriormente.
1. En **Condiciones** click **Agregar** para abrir el **Configuración de condición** asistente.
1. Para **Tipo de condición** select **Value Comparison**.
1. Defina el primer valor del campo de formulario en `%Component Resource Type%`. Puede utilizar el icono del elemento de datos ![icono de elemento de datos](assets/collect-data-analytics/cylinder-icon.png) para seleccionar el **Tipo de recurso de componente** elemento de datos. Deje el comparador configurado en `Equals`.
1. Establezca el segundo valor en `wknd/components/page`.

   ![Configuración de la condición para la regla de carga de página](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Es posible añadir esta condición dentro de la función de código personalizado que escucha la variable `cmp:show` creado anteriormente en el tutorial. Sin embargo, agregarla dentro de la interfaz de usuario da más visibilidad a los usuarios adicionales que pueden necesitar realizar cambios en la regla. Además, podemos usar nuestro elemento de datos!

1. Guarde los cambios.

## Establecer variables de Analytics y señalización de vista de página de déclencheur

Actualmente, la variable **Página cargada** la regla simplemente genera una sentencia de consola. A continuación, utilice los elementos de datos y la extensión de Analytics para establecer las variables de Analytics como un **acción** en el **Página cargada** regla. También estableceremos una acción adicional para el déclencheur del **Señalización de vista de página** y enviar los datos recopilados a Adobe Analytics.

1. En el **Página cargada** regla **remove** el **Core - Custom Code** acción (las instrucciones de consola):

   ![Eliminar acción de código personalizado](assets/collect-data-analytics/remove-console-statements.png)

1. En Acciones (Actions), haga clic en **Agregar** para agregar una nueva acción.
1. Configure las variables **Extensión** escriba a **Adobe Analytics** y establezca la variable **Tipo de acción** a  **Establecer variables**

   ![Configurar extensión de acción en variables definidas de Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. En el panel principal, seleccione una **eVar** y se establecen como el valor del elemento de datos **Plantilla de página**. Uso del icono de elementos de datos ![Icono de Data Elements](assets/collect-data-analytics/cylinder-icon.png) para seleccionar el **Plantilla de página** elemento.

   ![Establecer como plantilla de página eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Desplácese hacia abajo, debajo **Configuración adicional** set **Nombre de la página** al elemento de datos **Nombre de la página**:

   ![Nombre de página Configuración de variable de entorno](assets/collect-data-analytics/page-name-env-variable-set.png)

   Guarde los cambios.

1. A continuación, añada una acción adicional a la derecha del **Adobe Analytics: Establecer variables** tocando el botón **plus** icono:

   ![Añadir una acción de Launch adicional](assets/collect-data-analytics/add-additional-launch-action.png)

1. Configure las variables **Extensión** escriba a **Adobe Analytics** y establezca la variable **Tipo de acción** a  **Send Beacon**. Dado que esto se considera una vista de página, deje el seguimiento predeterminado establecido en **`s.t()`**.

   ![Acción Send Beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Guarde los cambios. La variable **Página cargada** ahora debe tener la siguiente configuración:

   ![Configuración de lanzamiento final](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Escuche el `cmp:show` evento.
   * **2.** Compruebe que el evento se activó mediante una página.
   * **3.** Configure las variables de Analytics para **Nombre de la página** y **Plantilla de página**
   * **4.** Envío de la señalización de vista de página de Analytics
1. Guarde todos los cambios y cree la biblioteca de Launch, promociéndola al entorno adecuado.

## Validar la señalización de vista de página y la llamada de Analytics

Ahora que la variable **Página cargada** envía la señalización de Analytics, debería poder ver las variables de seguimiento de Analytics mediante Experience Platform Debugger.

1. Abra el [Sitio WKND](https://wknd.site/us/en.html) en su navegador.
1. Haga clic en el icono de Debugger ![Icono de Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) para abrir Experience Platform Debugger.
1. Asegúrese de que Debugger asigne la propiedad de Launch a *your* Entorno de desarrollo, tal como se describió anteriormente y **Registro de consola** está activada.
1. Abra el menú Analytics y compruebe que el grupo de informes está configurado en *your* grupo de informes. El Nombre de página también debe rellenarse:

   ![Depurador de pestañas de Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Desplácese hacia abajo y expanda **Solicitudes de red**. Debería poder encontrar la variable **evar** configurado para la variable **Plantilla de página**:

   ![Evar y conjunto de nombres de página](assets/collect-data-analytics/evar-page-name-set.png)

1. Vuelva al explorador y abra la consola del desarrollador. Haga clic en **Carrusel** en la parte superior de la página.

   ![Pulsar en la página de carrusel](assets/collect-data-analytics/click-carousel-page.png)

1. Observe en la consola del explorador la declaración de la consola:

   ![Condición no cumplida](assets/collect-data-analytics/condition-not-met.png)

   Esto se debe a que el carrusel hace el déclencheur a `cmp:show` evento *but* debido a nuestra comprobación de las **Tipo de recurso de componente**, no se activa ningún evento.

   >[!NOTE]
   >
   > Si no ve ningún registro de consola, asegúrese de que **Registro de consola** está marcado en **Launch** en Experience Platform Debugger.

1. Vaya a una página de artículos como [Australia Occidental](https://wknd.site/us/en/magazine/western-australia.html). Observe que Nombre de página y Tipo de plantilla cambian.

## Felicitaciones!

Solo ha utilizado la capa de datos del cliente de Adobe impulsada por eventos y el Experience Platform Launch para recopilar datos de página de datos de un sitio AEM y enviarlos a Adobe Analytics.

### Siguientes pasos

Consulte el siguiente tutorial para aprender a utilizar la capa de datos del cliente de Adobe impulsada por eventos para [rastrear clics de componentes específicos en un sitio de Adobe Experience Manager](track-clicked-component.md).
