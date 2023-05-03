---
title: Recopilación de datos de página con Adobe Analytics
description: Utilice la capa de datos del cliente de Adobe impulsada por eventos para recopilar datos sobre la actividad del usuario en un sitio web creado con Adobe Experience Manager. Aprenda a utilizar las reglas de etiquetas para detectar estos eventos y enviar datos a un grupo de informes de Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '2447'
ht-degree: 3%

---

# Recopilación de datos de página con Adobe Analytics

>[!NOTE]
>
>Adobe Experience Platform Launch se ha convertido en un conjunto de tecnologías de recopilación de datos en Adobe Experience Platform. Como resultado, se han implementado varios cambios terminológicos en la documentación del producto. Consulte lo siguiente [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) para una referencia consolidada de los cambios terminológicos.


Aprenda a utilizar las funciones integradas de la variable [Adobe de la capa de datos del cliente con AEM componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es) para recopilar datos sobre una página en Adobe Experience Manager Sites. [Etiquetas en el Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) y [Extensión de Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) se utilizan para crear reglas para enviar datos de página a Adobe Analytics.

## Qué va a generar {#what-build}

![Seguimiento de datos de página](assets/collect-data-analytics/analytics-page-data-tracking.png)

En este tutorial, se va a almacenar en déclencheur una regla de etiqueta basada en un evento de la capa de datos del cliente de Adobe. Además, agregue condiciones para cuándo se debe activar la regla y, a continuación, envíe la variable **Nombre de la página** y **Plantilla de página** valores de una página AEM a Adobe Analytics.

### Objetivos {#objective}

1. Cree una regla impulsada por eventos en la propiedad tag que capture los cambios de la capa de datos
1. Asignación de propiedades de capa de datos de página a elementos de datos en la propiedad de etiqueta
1. Recopilar y enviar datos de página a Adobe Analytics mediante la señalización de vista de página

## Requisitos previos

Se requieren las siguientes opciones:

* **Tag, propiedad** en Experience Platform
* **Adobe Analytics** ID del grupo de informes de prueba/desarrollo y servidor de seguimiento. Consulte la siguiente documentación para [creación de un grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) extensión del explorador. Capturas de pantalla de este tutorial tomadas del navegador Chrome.
* (Opcional) AEM sitio con la variable [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Este tutorial utiliza la cara pública [WKND](https://wknd.site/us/en.html) pero puede usar su propio sitio.

>[!NOTE]
>
> ¿Necesita ayuda para integrar la propiedad de etiquetas y AEM sitio? [Ver esta serie de vídeo](../experience-platform/data-collection/tags/overview.md).

## Cambiar el entorno de etiquetas para el sitio WKND

La variable [WKND](http://wknd.site/us/en.html) es un sitio público creado en función de [un proyecto de código abierto](https://github.com/adobe/aem-guides-wknd) diseñado como referencia y [tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es) para una implementación AEM.

En lugar de configurar un entorno AEM e instalar la base de código WKND, puede utilizar el depurador de Experience Platform para **switch** live [Sitio WKND](http://wknd.site/us/en.html) a *your* tag . Sin embargo, puede usar su propio sitio AEM si ya tiene el [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

1. Inicie sesión en el Experience Platform y [crear una propiedad Tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (si aún no lo ha hecho).
1. Asegúrese de que una etiqueta inicial de JavaScript [se ha creado la biblioteca](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) y promocionados a la etiqueta [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=es).
1. Copie el código incrustado de JavaScript del entorno de etiquetas en el que se ha publicado su biblioteca.

   ![Copiar código incrustado de propiedad de etiqueta](assets/collect-data-analytics/launch-environment-copy.png)

1. En el explorador, abra una nueva pestaña y vaya a [Sitio WKND](http://wknd.site/us/en.html)
1. Abra la extensión del explorador de Experience Platform Debugger .

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Vaya a **Etiquetas de Experience Platform** > **Configuración** y **Códigos incrustados insertados** reemplazar el código incrustado existente por *your* incrustar código copiado del paso 3.

   ![Reemplazar código incrustado](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Habilitar **Registro de consola** y **Bloqueo** Debugger en la ficha WKND.

   ![Registro de consola](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Comprobar la capa de datos del cliente de Adobe en el sitio WKND

La variable [Proyecto de referencia WKND](https://github.com/adobe/aem-guides-wknd) está construido con AEM componentes principales y tiene el [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) de forma predeterminada. A continuación, compruebe que la capa de datos del cliente de Adobe esté habilitada.

1. Vaya a [Sitio WKND](http://wknd.site/us/en.html).
1. Abra las herramientas para desarrolladores del explorador y vaya a la **Consola**. Ejecute el siguiente comando:

   ```js
   adobeDataLayer.getState();
   ```

   El código anterior devuelve el estado actual de la capa de datos del cliente de Adobe.

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

   Para enviar datos de página a Adobe Analytics, usemos las propiedades estándar como `dc:title`, `xdm:language`y `xdm:template` de la capa de datos.

   Para obtener más información, consulte [Esquema de página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) de los esquemas de datos de los componentes principales.

   >[!NOTE]
   >
   > Si no ve la variable `adobeDataLayer` ¿Objeto JavaScript? Asegúrese de que la variable [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) del sitio.

## Crear una regla de página cargada

La capa de datos del cliente de Adobe es una **evento** capa de datos controlada. Cuando se carga la capa de datos de la página AEM, se déclencheur un `cmp:show` evento. Cree una regla que se active cuando `cmp:show` se activa desde la capa de datos de la página.

1. Vaya al Experience Platform y a la propiedad tag integrada con el sitio AEM.
1. Vaya a la **Reglas** en la interfaz de usuario de Tag Property y, a continuación, haga clic en **Crear nueva regla**.

   ![Crear regla](assets/collect-data-analytics/analytics-create-rule.png)

1. Asigne un nombre a la regla **Página cargada**.
1. En el **Eventos** subsección, haga clic en **Agregar** para abrir el **Configuración de eventos** asistente.
1. Para **Tipo de evento** campo, seleccione **Código personalizado**.

   ![Asigne un nombre a la regla y añada el evento de Custom Code](assets/collect-data-analytics/custom-code-event.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
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

   El fragmento de código anterior añade un detector de eventos por [inserción de una función](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) en la capa de datos. When `cmp:show` se activa el evento `pageShownEventHandler` llamada a . En esta función, se añaden algunas comprobaciones de coherencia y se añade una nueva `event` se construye con el último [estado de la capa de datos](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para el componente que activó el evento.

   Por último, la variable `trigger(event)` llamada a . La variable `trigger()` es un nombre reservado en la propiedad tag y **déclencheur** la regla. La variable `event` se pasa como un parámetro que, a su vez, está expuesto por otro nombre reservado en la propiedad tag . Los elementos de datos de la propiedad tag ahora pueden hacer referencia a varias propiedades mediante fragmentos de código como `event.component['someKey']`.

1. Guarde los cambios.
1. Siguiente en **Acciones** click **Agregar** para abrir el **Configuración de la acción** asistente.
1. Para **Tipo de acción** , elija **Código personalizado**.

   ![Tipo de acción de código personalizado](assets/collect-data-analytics/action-custom-code.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   La variable `event` se pasa desde el `trigger()` método llamado en el evento personalizado. Aquí puede consultar la `component` es la página actual derivada de la capa de datos `getState` en el evento personalizado.

1. Guarde los cambios y ejecute un [versión](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) en la propiedad tag para promocionar el código a la variable [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=es) en su sitio AEM.

   >[!NOTE]
   >
   > Puede resultar útil usar la variable [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) para cambiar el código incrustado a un **Desarrollo** entorno.

1. Vaya a su sitio AEM y abra las herramientas para desarrolladores para ver la consola. Actualice la página y verá que se han registrado los mensajes de la consola:

![Mensajes de la consola Carga de página](assets/collect-data-analytics/page-show-event-console.png)

## Crear elementos de datos

A continuación, cree varios elementos de datos para capturar valores diferentes de la capa de datos del cliente de Adobe. Como se ha visto en el ejercicio anterior, es posible acceder a las propiedades de la capa de datos directamente a través del código personalizado. La ventaja de utilizar elementos de datos es que se pueden reutilizar en todas las reglas de etiquetas.

Los elementos de datos se asignan al `@type`, `dc:title`y `xdm:template` propiedades.

### Tipo de recurso de componente

1. Vaya al Experience Platform y a la propiedad tag integrada con el sitio AEM.
1. Vaya a la **Elementos de datos** y haga clic en **Crear nuevo elemento de datos**.
1. Para la variable **Nombre** , introduzca la variable **Tipo de recurso de componente**.
1. Para la variable **Tipo de elemento de datos** campo, seleccione **Código personalizado**.

   ![Tipo de recurso de componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Guarde los cambios.

   >[!NOTE]
   >
   > Recuerde que la variable `event` El objeto está disponible y tiene ámbitos basados en el evento que activó la variable **Regla** en la propiedad tag . El valor de un elemento de datos no se establece hasta que el elemento de datos *referenciado* dentro de una regla. Por lo tanto, es seguro utilizar este elemento de datos dentro de una regla como la **Página cargada** regla creada en el paso anterior *but* no sería seguro usar en otros contextos.

### Nombre de página

1. Haga clic en **Añadir elemento de datos** botón
1. Para la variable **Nombre** , introduzca **Nombre de la página**.
1. Para la variable **Tipo de elemento de datos** campo, seleccione **Código personalizado**.
1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Guarde los cambios.

### Plantilla de la página

1. Haga clic en el **Añadir elemento de datos** botón
1. Para la variable **Nombre** , introduzca **Plantilla de página**.
1. Para la variable **Tipo de elemento de datos** campo, seleccione **Código personalizado**.
1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Guarde los cambios.

1. Ahora debe tener tres elementos de datos como parte de la regla:

   ![Elementos de datos en la regla](assets/collect-data-analytics/data-elements-page-rule.png)

## Añadir la extensión de Analytics

A continuación, agregue la extensión de Analytics a la propiedad de etiqueta para enviar datos a un grupo de informes.

1. Vaya al Experience Platform y a la propiedad tag integrada con el sitio AEM.
1. Vaya a **Extensiones** > **Catálogo**
1. Busque la variable **Adobe Analytics** extensión y haga clic en **Instalar**

   ![Extensión de Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. En **Administración de biblioteca** > **Grupos de informes**, introduzca los ID de los grupos de informes que desee utilizar con cada entorno de etiquetas.

   ![Especifique los ID de los grupos de informes](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Está bien utilizar un grupo de informes para todos los entornos en este tutorial, pero en la vida real le recomendamos utilizar grupos de informes independientes, como se muestra en la imagen siguiente

   >[!TIP]
   >
   >Se recomienda usar la variable *Opción Administrar la biblioteca por mí* ya que la configuración de Administración de biblioteca facilita mucho mantener la variable `AppMeasurement.js` biblioteca actualizada.

1. Marque la casilla para habilitar **Usar Activity Map**.

   ![Habilitar Activity Map de uso](assets/track-clicked-component/analytic-track-click.png)

1. En **General** > **Servidor de seguimiento**, introduzca su servidor de seguimiento, por ejemplo, `tmd.sc.omtrdc.net`. Introduzca el servidor de seguimiento SSL si su sitio es compatible con `https://`

   ![Especifique los servidores de seguimiento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Haga clic en **Guardar** para guardar los cambios.

## Añadir una condición a la regla Page Loaded

A continuación, actualice la variable **Página cargada** para usar la variable **Tipo de recurso de componente** elemento de datos para garantizar que la regla solo se active cuando la variable `cmp:show` es para la variable **Página**. Otros componentes pueden activar el `cmp:show` por ejemplo, el componente Carrusel lo activa cuando cambian las diapositivas. Por lo tanto, es importante añadir una condición para esta regla.

1. En la interfaz de usuario de la propiedad de etiqueta, vaya a la **Página cargada** regla creada anteriormente.
1. En **Condiciones** click **Agregar** para abrir el **Configuración de condición** asistente.
1. Para **Tipo de condición** campo, seleccione **Value Comparison** .
1. Defina el primer valor del campo de formulario en `%Component Resource Type%`. Puede utilizar el icono del elemento de datos ![icono de elemento de datos](assets/collect-data-analytics/cylinder-icon.png) para seleccionar el **Tipo de recurso de componente** elemento de datos. Deje el comparador configurado en `Equals`.
1. Establezca el segundo valor en `wknd/components/page`.

   ![Configuración de la condición para la regla de carga de página](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Es posible añadir esta condición dentro de la función de código personalizado que escucha la variable `cmp:show` creado anteriormente en el tutorial. Sin embargo, agregarla dentro de la interfaz de usuario da más visibilidad a los usuarios adicionales que pueden necesitar realizar cambios en la regla. Además, podemos usar nuestro elemento de datos!

1. Guarde los cambios.

## Establecer variables de Analytics y señalización de vista de página de déclencheur

Actualmente, la variable **Página cargada** la regla simplemente genera una sentencia de consola. A continuación, utilice los elementos de datos y la extensión de Analytics para establecer las variables de Analytics como un **acción** en el **Página cargada** regla. También configuramos una acción adicional para almacenar en déclencheur la variable **Señalización de vista de página** y enviar los datos recopilados a Adobe Analytics.

1. En la regla Page Loaded , **remove** el **Core - Custom Code** acción (las instrucciones de consola):

   ![Eliminar acción de código personalizado](assets/collect-data-analytics/remove-console-statements.png)

1. En la subsección Acciones , haga clic en **Agregar** para agregar una nueva acción.

1. Configure las variables **Extensión** escriba a **Adobe Analytics** y establezca la variable **Tipo de acción** a  **Establecer variables**

   ![Configurar extensión de acción en variables definidas de Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. En el panel principal, seleccione una **eVar** y se establecen como el valor del elemento de datos **Plantilla de página**. Uso del icono de elementos de datos ![Icono de Data Elements](assets/collect-data-analytics/cylinder-icon.png) para seleccionar el **Plantilla de página** elemento.

   ![Establecer como plantilla de página eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Desplácese hacia abajo, debajo **Configuración adicional** set **Nombre de la página** al elemento de datos **Nombre de la página**:

   ![Nombre de página Configuración de variable de entorno](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Guarde los cambios.

1. A continuación, añada una acción adicional a la derecha del **Adobe Analytics: Establecer variables** tocando el botón **plus** icono:

   ![Añadir una acción adicional de regla de etiqueta](assets/collect-data-analytics/add-additional-launch-action.png)

1. Configure las variables **Extensión** escriba a **Adobe Analytics** y establezca la variable **Tipo de acción** a  **Send Beacon**. Dado que esta acción se considera una vista de página, deje el seguimiento predeterminado establecido en **`s.t()`**.

   ![Acción Send Beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Guarde los cambios. La variable **Página cargada** ahora debe tener la siguiente configuración:

   ![Configuración final de regla de etiqueta](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Escuche el `cmp:show` evento.
   * **2.** Compruebe que el evento se activó mediante una página.
   * **3.** Configure las variables de Analytics para **Nombre de la página** y **Plantilla de página**
   * **4.** Envío de la señalización de vista de página de Analytics

1. Guarde todos los cambios y cree la biblioteca de etiquetas, promocionándola al entorno adecuado.

## Validar la señalización de vista de página y la llamada de Analytics

Ahora que la variable **Página cargada** envía la señalización de Analytics, debería poder ver las variables de seguimiento de Analytics mediante Experience Platform Debugger.

1. Abra el [Sitio WKND](https://wknd.site/us/en.html) en su navegador.
1. Haga clic en el icono de Debugger ![Icono de Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) para abrir Experience Platform Debugger.
1. Asegúrese de que Debugger asigne la propiedad tag a *your* Entorno de desarrollo, tal como se describió anteriormente y **Registro de consola** está activada.
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
   > Si no ve ningún registro de consola, asegúrese de que **Registro de consola** está marcado en **Etiquetas de Experience Platform** en Experience Platform Debugger.

1. Vaya a una página de artículos como [Australia Occidental](https://wknd.site/us/en/magazine/western-australia.html). Observe que Nombre de página y Tipo de plantilla cambian.

## Enhorabuena.

Solo ha utilizado la capa de datos del cliente de Adobe impulsada por eventos y las etiquetas en Experience Platform para recopilar datos de página de datos de un sitio AEM y enviarlos a Adobe Analytics.

### Pasos siguientes

Consulte el siguiente tutorial para aprender a utilizar la capa de datos del cliente de Adobe impulsada por eventos para [rastrear clics de componentes específicos en un sitio de Adobe Experience Manager](track-clicked-component.md).
