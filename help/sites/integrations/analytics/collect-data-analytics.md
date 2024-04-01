---
title: Integración de AEM Sites con Adobe Analytics con la extensión de etiquetas de Adobe Analytics
description: Integre AEM Sites con Adobe Analytics mediante la capa de datos del cliente de Adobe impulsada por evento para recopilar datos sobre la actividad del usuario en un sitio web creado con Adobe Experience Manager. Aprenda a utilizar las reglas de etiquetas para detectar estos eventos y enviar datos a un grupo de informes de Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="Integración" type="positive"
doc-type: Tutorial
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
duration: 668
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '2262'
ht-degree: 1%

---

# Integración de AEM Sites y Adobe Analytics

Obtenga información sobre cómo integrar AEM Sites y Adobe Analytics con la extensión de etiquetas de Adobe Analytics, mediante las funciones integradas de la [Capa de datos del cliente de Adobe AEM con componentes principales de](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es) para recopilar datos sobre una página en Adobe Experience Manager Sites. [Etiquetas en el Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) y el [Extensión de Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) se utilizan para crear reglas para enviar datos de página a Adobe Analytics.

## Lo que va a generar {#what-build}

![Seguimiento de datos de página](assets/collect-data-analytics/analytics-page-data-tracking.png)

En este tutorial, va a almacenar en déclencheur una regla de etiquetas basada en un evento de la capa de datos del cliente de Adobe. Además, agregue condiciones para cuándo se debe activar la regla y, a continuación, envíe el **Nombre de página** y **Plantilla de página** AEM valores de una página de a Adobe Analytics.

### Objetivos {#objective}

1. Cree una regla impulsada por evento en la propiedad de etiqueta que capture los cambios de la capa de datos
1. Asignar propiedades de capa de datos de página a elementos de datos en la propiedad de etiqueta
1. Recopilación y envío de datos de página a Adobe Analytics mediante la señalización de vista de página

## Requisitos previos

Se requiere lo siguiente:

* **Propiedad de etiqueta** en Experience Platform
* **Adobe Analytics** ID del grupo de informes de prueba/desarrollo y servidor de seguimiento. Consulte la siguiente documentación para [creación de un grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) extensión del explorador. Capturas de pantalla de este tutorial capturadas desde el navegador Chrome.
* AEM (Opcional) Sitio de con [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Este tutorial utiliza el entorno público [WKND](https://wknd.site/us/es.html) sitio, pero puede utilizar su propio sitio.

>[!NOTE]
>
> AEM ¿Necesita ayuda con la integración de la propiedad de etiquetas y el sitio de? [Ver esta serie de vídeos](../experience-platform/data-collection/tags/overview.md).

## Cambiar el entorno de etiquetas para el sitio WKND

El [WKND](https://wknd.site/us/es.html) es un sitio público creado sobre la base de [un proyecto de código abierto](https://github.com/adobe/aem-guides-wknd) diseñado como referencia y [tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es) AEM para una implementación de la.

AEM En lugar de configurar un entorno de e instalar el código base de WKND, puede utilizar el depurador de Experience Platform para lo siguiente **cambiar** el live [Sitio WKND](https://wknd.site/us/es.html) hasta *su* propiedad de etiqueta. AEM Sin embargo, puede utilizar su propio sitio de la si ya tiene el [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

1. Inicie sesión en el Experience Platform y [crear una propiedad Tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (si aún no lo ha hecho).
1. Asegúrese de que haya una etiqueta JavaScript inicial [se ha creado la biblioteca](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) y se promocionan a la etiqueta [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=es).
1. Copie el código incrustado de JavaScript del entorno de etiquetas en el que se ha publicado la biblioteca.

   ![Copiar código incrustado de propiedad de etiqueta](assets/collect-data-analytics/launch-environment-copy.png)

1. En el explorador, abra una pestaña nueva y navegue hasta [Sitio WKND](https://wknd.site/us/es.html)
1. Abra la extensión del explorador de Experience Platform Debugger.

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Vaya a **Etiquetas de Experience Platform** > **Configuración** y en **Códigos incrustados insertados** reemplazar el código incrustado existente con *su* código incrustado copiado del paso 3.

   ![Reemplazar código incrustado](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Activar **Registro de consola** y **Bloquear** Use el depurador en la pestaña WKND.

   ![Registro de consola](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Comprobar la capa de datos del cliente de Adobe en el sitio WKND

El [Proyecto de referencia de WKND](https://github.com/adobe/aem-guides-wknd) AEM se crea con los componentes principales de la y tiene el [Capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) de forma predeterminada. A continuación, compruebe que la capa de datos del cliente de Adobe esté habilitada.

1. Vaya a [Sitio WKND](https://wknd.site/us/es.html).
1. Abra las herramientas para desarrolladores del explorador y vaya al **Consola**. Ejecute el siguiente comando:

   ```js
   adobeDataLayer.getState();
   ```

   El código anterior devuelve el estado actual de la capa de datos del cliente de Adobe.

   ![Estado de capa de datos de Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Expanda la respuesta e inspeccione el `page` entrada. Debería ver un esquema de datos como el siguiente:

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

   Para enviar datos de página a Adobe Analytics, usemos las propiedades estándar como `dc:title`, `xdm:language`, y `xdm:template` de la capa de datos.

   Para obtener más información, consulte [Esquema de página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) de los Esquemas de datos de componentes principales.

   >[!NOTE]
   >
   > Si no ve la etiqueta `adobeDataLayer` ¿Objeto JavaScript? Asegúrese de que la variable [Se ha habilitado la capa de datos del cliente de Adobe](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) en el sitio.

## Crear una regla de carga de página

La capa de datos del cliente de Adobe es un **impulsado por eventos** capa de datos. AEM Cuando se carga la capa de datos de la página de, se genera un déclencheur de `cmp:show` evento. Cree una regla que se active cuando la variable `cmp:show` se activa desde la capa de datos de página.

1. Vaya a Experience Platform AEM y luego a la propiedad de etiquetas integrada con el sitio de.
1. Vaya a **Reglas** en la interfaz de usuario de la propiedad de etiquetas y haga clic en **Crear nueva regla**.

   ![Crear regla](assets/collect-data-analytics/analytics-create-rule.png)

1. Asignar un nombre a la regla **Página cargada**.
1. En el **Eventos** , haga clic en **Añadir** para abrir **Configuración de eventos** asistente.
1. Para **Tipo de evento** , seleccione **Código personalizado**.

   ![Asigne un nombre a la regla y añada el evento de código personalizado](assets/collect-data-analytics/custom-code-event.png)

1. Clic **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.log("cmp:show event: " + evt.eventInfo.path);
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

   El fragmento de código anterior agrega un detector de eventos de [inserción de una función](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) en la capa de datos. Cuándo `cmp:show` se activa el `pageShownEventHandler` se llama a la función. En esta función, se añaden algunas comprobaciones de coherencia y una nueva `event` se construye con la última [estado de la capa de datos](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para el componente que activó el evento.

   Finalmente, la `trigger(event)` se llama a la función. El `trigger()` función es un nombre reservado en la propiedad etiqueta y **déclencheur** la regla. El `event` El objeto se pasa como parámetro que, a su vez, se expone con otro nombre reservado en la propiedad de etiqueta. Los elementos de datos de la propiedad de etiqueta ahora pueden hacer referencia a varias propiedades mediante fragmentos de código como `event.component['someKey']`.

1. Guarde los cambios.
1. Siguiente debajo de **Acciones** click **Añadir** para abrir **Configuración de acción** asistente.
1. Para **Tipo de acción** , elija **Código personalizado**.

   ![Tipo de acción de código personalizado](assets/collect-data-analytics/action-custom-code.png)

1. Clic **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   El `event` se pasa desde el `trigger()` método invocado en el evento personalizado. Aquí, el `component` es la página actual derivada de la capa de datos `getState` en el evento personalizado.

1. Guarde los cambios y ejecute un [generar](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) en la propiedad tag para promocionar el código a [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=es) AEM se utiliza en el sitio de la.

   >[!NOTE]
   >
   > Puede resultar útil utilizar la variable [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) para cambiar el código incrustado a **Desarrollo** entorno.

1. AEM Vaya al sitio de la y abra las herramientas para desarrolladores para ver la consola. Actualice la página y debería ver que se han registrado los mensajes de la consola:

![Mensajes de consola cargados de página](assets/collect-data-analytics/page-show-event-console.png)

## Crear elementos de datos

A continuación, cree varios elementos de datos para capturar valores diferentes de la capa de datos del cliente de Adobe. Como se ha visto en el ejercicio anterior, es posible acceder a las propiedades de la capa de datos directamente a través del código personalizado. La ventaja de utilizar elementos de datos es que se pueden reutilizar en las reglas de etiquetas.

Los elementos de datos se asignan a `@type`, `dc:title`, y `xdm:template` propiedades.

### Tipo de medio de componente

1. Vaya a Experience Platform AEM y luego a la propiedad de etiquetas integrada con el sitio de.
1. Vaya a **Elementos de datos** y haga clic en **Crear nuevo elemento de datos**.
1. Para el **Nombre** , introduzca la variable **Tipo de medio de componente**.
1. Para el **Tipo de elemento de datos** , seleccione **Código personalizado**.

   ![Tipo de medio de componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Clic **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Guarde los cambios.

   >[!NOTE]
   >
   > Recuerde que la variable `event` está disponible y su ámbito se basa en el evento que activó el **Regla** en la propiedad de etiqueta. El valor de un elemento de datos no se establece hasta que se define el elemento de datos *referenciado* dentro de una regla. Por lo tanto, es seguro utilizar este elemento de datos dentro de una regla como la **Página cargada** regla creada en el paso anterior *pero* no sería seguro utilizarlo en otros contextos.

### Nombre de página

1. Clic **Añadir elemento de datos** botón
1. Para el **Nombre** , introduzca **Nombre de página**.
1. Para el **Tipo de elemento de datos** , seleccione **Código personalizado**.
1. Clic **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Guarde los cambios.

### Plantilla de la página

1. Haga clic en **Añadir elemento de datos** botón
1. Para el **Nombre** , introduzca **Plantilla de página**.
1. Para el **Tipo de elemento de datos** , seleccione **Código personalizado**.
1. Clic **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Guarde los cambios.

1. Ahora debe tener tres elementos de datos como parte de la regla:

   ![Elementos de datos en la regla](assets/collect-data-analytics/data-elements-page-rule.png)

## Añadir la extensión de Analytics

A continuación, añada la extensión de Analytics a la propiedad de etiquetas para enviar datos a un grupo de informes.

1. Vaya a Experience Platform AEM y luego a la propiedad de etiquetas integrada con el sitio de.
1. Ir a **Extensiones** > **Catálogo**
1. Busque el **Adobe Analytics** y haga clic en **Instalar**

   ![Extensión de Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. En **Administración de biblioteca** > **Grupos de informes**, introduzca los id de los grupos de informes que desee utilizar con cada entorno de etiquetas.

   ![Introduzca los ID del grupo de informes](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Es aceptable utilizar un grupo de informes para todos los entornos en este tutorial, pero en la vida real le recomendamos utilizar grupos de informes separados, como se muestra en la siguiente imagen

   >[!TIP]
   >
   >Se recomienda utilizar la variable *Opción Administrar la biblioteca por mí* como la configuración de Administración de biblioteca, ya que facilita en gran medida mantener la variable `AppMeasurement.js` biblioteca actualizada.

1. Marque la casilla para habilitar **Usar Activity Map**.

   ![Habilitar el Activity Map de usuario](assets/track-clicked-component/analytic-track-click.png)

1. En **General** > **Servidor de seguimiento**, introduzca su servidor de seguimiento, por ejemplo, `tmd.sc.omtrdc.net`. Introduzca el servidor de seguimiento SSL si su sitio es compatible con. `https://`

   ![Introduzca los servidores de seguimiento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Clic **Guardar** para guardar los cambios.

## Añadir una condición a la regla Page Loaded

A continuación, actualice el **Página cargada** regla para utilizar **Tipo de medio de componente** elemento de datos para garantizar que la regla solo se active cuando el `cmp:show` evento es para **Página**. Otros componentes pueden activar el `cmp:show` , por ejemplo, el componente Carrusel lo activa cuando cambian las diapositivas. Por lo tanto, es importante agregar una condición para esta regla.

1. En la interfaz de usuario de la propiedad Etiqueta, vaya a **Página cargada** regla creada anteriormente.
1. En **Condiciones** click **Añadir** para abrir **Configuración de condición** asistente.
1. Para **Tipo de condición** , seleccione **Value Comparison** opción.
1. Establezca el primer valor del campo de formulario en `%Component Resource Type%`. Puede utilizar el icono de elemento de datos ![icono de elemento de datos](assets/collect-data-analytics/cylinder-icon.png) para seleccionar **Tipo de medio de componente** elemento de datos. Deje el comparador configurado como. `Equals`.
1. Establezca el segundo valor en `wknd/components/page`.

   ![Configuración de condición para regla de página cargada](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Es posible añadir esta condición dentro de la función de código personalizado que escucha el `cmp:show` evento creado anteriormente en el tutorial. Sin embargo, agregarla dentro de la interfaz de usuario da más visibilidad a los usuarios adicionales que pueden necesitar realizar cambios en la regla. Además, podemos utilizar nuestro elemento de datos.

1. Guarde los cambios.

## Establecer variables de Analytics y señalización de vista de página déclencheur

Actualmente la variable **Página cargada** La regla simplemente genera una instrucción de consola. A continuación, utilice los elementos de datos y la extensión de Analytics para establecer variables de Analytics como un **acción** en el **Página cargada** regla. También establecemos una acción adicional para almacenar en déclencheur la variable **Señalización de vista de página** y enviar los datos recopilados a Adobe Analytics.

1. En la regla Page Loaded, **quitar** el **Core - Custom Code** acción (instrucciones de la consola):

   ![Eliminar acción de código personalizado](assets/collect-data-analytics/remove-console-statements.png)

1. En la subsección Acciones, haga clic en **Añadir** para añadir una nueva acción.

1. Configure las variables **Extensión** escriba a **Adobe Analytics** y configure el **Tipo de acción** hasta  **Establecer variables**

   ![Establecer extensión de la acción en variables de conjunto de Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. En el panel principal, seleccione un disponible **eVar** y se establece como el valor del elemento de datos **Plantilla de página**. Uso del icono Elementos de datos ![Icono de elementos de datos](assets/collect-data-analytics/cylinder-icon.png) para seleccionar **Plantilla de página** Elemento.

   ![Establecer como plantilla de página de eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Desplazarse hacia abajo, debajo de **Configuración adicional** set **Nombre de página** al elemento de datos **Nombre de página**:

   ![Nombre de página Entorno Conjunto de variables](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Guarde los cambios.

1. A continuación, añada una acción adicional a la derecha de **Adobe Analytics: Establecer variables** pulsando el botón **plus** icono:

   ![Añadir una acción adicional de regla de etiqueta](assets/collect-data-analytics/add-additional-launch-action.png)

1. Configure las variables **Extensión** escriba a **Adobe Analytics** y configure el **Tipo de acción** hasta  **Send Beacon**. Dado que esta acción se considera una vista de página, deje el seguimiento predeterminado establecido en **`s.t()`**.

   ![Acción Enviar señalización de Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Guarde los cambios. El **Página cargada** La regla debe tener la configuración siguiente:

   ![Configuración final de regla de etiqueta](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Escuche el `cmp:show` evento.
   * **2.** Compruebe que el evento se activó mediante una página.
   * **3.** Establecer variables de Analytics para **Nombre de página** y **Plantilla de página**
   * **4.** Envío de la señalización de vista de página de Analytics

1. Guarde todos los cambios y cree su biblioteca de etiquetas, promocionando al entorno adecuado.

## Validar la señalización de vista de página y la llamada de Analytics.

Ahora que la variable **Página cargada** La regla envía la señalización de Analytics. Debería poder ver las variables de seguimiento de Analytics mediante Experience Platform Debugger.

1. Abra el [Sitio WKND](https://wknd.site/us/es.html) en el explorador.
1. Haga clic en el icono Debugger ![Icono de Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) para abrir Experience Platform Debugger.
1. Asegúrese de que Debugger asigne la propiedad de etiqueta a *su* Entorno de desarrollo, tal como se describió anteriormente y **Registro de consola** está marcada.
1. Abra el menú Analytics y compruebe que el grupo de informes está configurado en *su* grupo de informes. También debe rellenarse el Nombre de página:

   ![Debugger de pestaña Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Desplazarse hacia abajo y expandir **Solicitudes de red**. Debería ser capaz de encontrar el **evar** configure para el **Plantilla de página**:

   ![Evar y conjunto de nombres de página](assets/collect-data-analytics/evar-page-name-set.png)

1. Vuelva al explorador y abra la consola de desarrollador. Haga clic en **Carrusel** en la parte superior de la página.

   ![Clic en la página de carrusel](assets/collect-data-analytics/click-carousel-page.png)

1. Observe en la consola del explorador la instrucción de consola:

   ![Condición no cumplida](assets/collect-data-analytics/condition-not-met.png)

   Esto se debe a que el carrusel hace un déclencheur a `cmp:show` evento *pero* debido a nuestra comprobación de la **Tipo de medio de componente**, no se activa ningún evento.

   >[!NOTE]
   >
   > Si no ve ningún registro de consola, asegúrese de que **Registro de consola** está marcado en **Etiquetas de Experience Platform** en Experience Platform Debugger.

1. Navegue hasta una página de artículo como [Australia Occidental](https://wknd.site/us/en/magazine/western-australia.html). Observe que Nombre de página y Tipo de plantilla cambian.

## Enhorabuena.

Acaba de utilizar la capa de datos del cliente de Adobe impulsada por evento y las etiquetas en Experience Platform AEM para recopilar datos de página de un sitio de y enviarlos a Adobe Analytics.

### Pasos siguientes

Consulte el siguiente tutorial para aprender a utilizar la capa de datos del cliente de Adobe impulsada por evento para lo siguiente [seguimiento de clics de componentes específicos en un sitio de Adobe Experience Manager](track-clicked-component.md).
