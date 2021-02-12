---
title: Seguimiento del componente en el que se hizo clic con Adobe Analytics
description: Utilice la capa de datos del cliente de Adobe impulsada por eventos para rastrear los clics de componentes específicos en un sitio de Adobe Experience Manager. Obtenga información sobre cómo utilizar las reglas en Experience Platform Launch para escuchar estos eventos y enviar datos a un Adobe Analytics con una señalización de seguimiento de vínculos.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 64c167ec1d625fdd8be1bc56f7f5e59460b8fed3
workflow-type: tm+mt
source-wordcount: '1831'
ht-degree: 1%

---


# Seguimiento del componente en el que se hizo clic con Adobe Analytics

Utilice la capa de datos del cliente de Adobe [evento con componentes principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/data-layer/overview.html) para rastrear los clics de componentes específicos en un sitio de Adobe Experience Manager. Obtenga información sobre cómo utilizar las reglas en Experience Platform Launch para detectar eventos de clics, filtrar por componente y enviar los datos a un Adobe Analytics con una señalización de seguimiento de vínculos.

## Qué va a generar

El equipo de mercadotecnia de WKND desea saber qué botones de llamada a acción (CTA) tienen el mejor rendimiento en la página de inicio. En este tutorial, agregaremos una nueva regla en Experience Platform Launch que escuche eventos `cmp:click` de los componentes **Teaser** y **Button** y envíe el ID del componente y un nuevo evento a Adobe Analytics junto con la señalización de seguimiento de vínculos.

![Qué generará los clics de seguimiento](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objetivos {#objective}

1. Cree una regla controlada por eventos en Launch según el evento `cmp:click`.
1. Filtre los diferentes eventos por tipo de recurso de componente.
1. Configure la identificación del componente en la que se hizo clic y envíe evento Adobe Analytics con la señalización de seguimiento de vínculos.

## Requisitos previos

Este tutorial es una continuación de [Recopilación de datos de página con Adobe Analytics](./collect-data-analytics.md) y supone que:

* Una **Propiedad de inicio** con la [extensión de Adobe Analytics](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) habilitada
* **Adobe** Analytics, ID del grupo de informes y servidor de seguimiento. Consulte la siguiente documentación para [crear un nuevo grupo de informes](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) Debuggerextensión del explorador configurada con la propiedad Launch cargada en  [https://wknd.site/us/en.](https://wknd.site/us/en.html) htmlor un sitio AEM con la capa de datos de Adobe activada.

## Inspect el Esquema Botón y Teaser

Antes de crear reglas en Launch, es útil revisar el esquema [del Botón y del teaser](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) e inspeccionarlos en la implementación de la capa de datos.

1. Vaya a [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Abra las herramientas de desarrollador del explorador y vaya a la **Consola**. Ejecute el siguiente comando:

   ```js
   adobeDataLayer.getState();
   ```

   Esto devuelve el estado actual de la capa de datos del cliente de Adobe.

   ![Estado de la capa de datos mediante la consola del navegador](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Expanda la respuesta y busque las entradas con el prefijo `button-` y `teaser-xyz-cta`. Debería ver un esquema de datos como el siguiente:

   Esquema de botón:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Esquema de teaser:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Se basan en el Esquema [Componente/Elemento de Contenedor](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item). La regla que crearemos en Launch utilizará este esquema.

## Crear una regla de llamada a acción en la que se hizo clic

La capa de datos del cliente de Adobe es una capa de datos controlada por **evento**. Cuando se hace clic en cualquier componente principal, se envía un evento `cmp:click` a través de la capa de datos. A continuación, cree una regla para escuchar el evento `cmp:click`.

1. Vaya al Experience Platform Launch y a la propiedad Web integrada con el sitio AEM.
1. Vaya a la sección **Reglas** en la interfaz de usuario de Launch y, a continuación, haga clic en **Añadir regla**.
1. Asigne un nombre a la regla **Llamada a acción: clic**.
1. Haga clic en **Eventos** > **Añadir** para abrir el asistente para **Configuración de Evento**.
1. En **Tipo de evento** seleccione **Código personalizado**.

   ![Asigne un nombre a la regla en la que se hizo clic y agregue el evento de código personalizado](assets/track-clicked-component/custom-code-event.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   El fragmento de código anterior agregará un detector de eventos [insertando una función](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) en la capa de datos. Cuando se activa el evento `cmp:click`, se llama a la función `componentClickedHandler`. En esta función se agregan algunas comprobaciones de integridad y se construye un nuevo objeto `event` con el estado más reciente [de la capa de datos](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para el componente que activó el evento.

   Después de llamar a `trigger(event)`. `trigger()` es un nombre reservado en Launch y &quot;déclencheur&quot; de la regla Launch. Pasamos el objeto `event` como parámetro que, a su vez, será expuesto por otro nombre reservado en Launch denominado `event`. Los elementos de datos de Launch ahora pueden hacer referencia a varias propiedades como, por ejemplo: `event.component['someKey']`.

1. Guarde los cambios.
1. A continuación, en **Acciones** haga clic en **Añadir** para abrir el asistente para **Configuración de acción**.
1. En **Tipo de acción** elija **Código personalizado**.

   ![Tipo de acción de código personalizado](assets/track-clicked-component/action-custom-code.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   El objeto `event` se pasa desde el método `trigger()` al que se llama en el evento personalizado. `component` es el estado actual del componente derivado de la capa de datos  `getState` que activó el clic.

1. Guarde los cambios y ejecute una [compilación](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) en Launch para promocionar el código al [entorno](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) utilizado en el sitio AEM.

   >[!NOTE]
   >
   > Puede resultar muy útil utilizar el [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) para cambiar el código incrustado a un entorno **Development**.

1. Vaya al [sitio WKND](https://wknd.site/us/en.html) y abra las herramientas de desarrollo para vista de la consola. Seleccione **Mantener registro**.

1. Haga clic en uno de los botones **Teaser** o **Botón** CTA para navegar a otra página.

   ![Botón de llamada a acción para hacer clic](assets/track-clicked-component/cta-button-to-click.png)

1. Observe en la consola del desarrollador que se ha activado la regla **Llamada a acción: clic**:

   ![Botón CTA en el que se hizo clic](assets/track-clicked-component/cta-button-clicked-log.png)

## Crear elementos de datos

A continuación, cree un elemento de datos para capturar el ID del componente y el título en los que se hizo clic. Recuerde que en el ejercicio anterior el resultado de `event.path` era algo similar a `component.button-b6562c963d` y el valor de `event.component['dc:title']` era algo así como &quot;Viajes de Vista&quot;.

### ID del componente

1. Vaya al Experience Platform Launch y a la propiedad Web integrada con el sitio AEM.
1. Vaya a la sección **Elementos de datos** y haga clic en **Añadir nuevo elemento de datos**.
1. Para **Nombre** escriba **Id. de componente**.
1. Para **Tipo de elemento de datos** seleccione **Código personalizado**.

   ![Formulario de elemento de datos de ID de componente](assets/track-clicked-component/component-id-data-element.png)

1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Guarde los cambios.

   >[!NOTE]
   >
   > Recuerde que el objeto `event` está disponible y tiene un ámbito basado en el evento que activó la **Regla** en Launch. El valor de un elemento de datos no se establece hasta que el elemento de datos se *hace referencia a* dentro de una regla. Por lo tanto, es seguro utilizar este elemento de datos dentro de una regla como la regla **Llamada a acción: clic** creada en el ejercicio anterior *pero* no sería seguro usar en otros contextos.

### Título del componente

1. Vaya a la sección **Elementos de datos** y haga clic en **Añadir nuevo elemento de datos**.
1. Para **Nombre** ingrese **Título del componente**.
1. Para **Tipo de elemento de datos** seleccione **Código personalizado**.
1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Guarde los cambios.

## Añadir una condición a la regla de llamada a acción en la que se hizo clic

A continuación, actualice la regla **Llamada a acción: clic** para asegurarse de que la regla solo se active cuando el evento `cmp:click` se active para un **teaser** o un **botón**. Dado que el CTA del teaser se considera un objeto independiente en la capa de datos, es importante comprobar el elemento principal para verificar que proviene de un teaser.

1. En la interfaz de usuario de Launch, vaya a la regla **Llamada a acción: clic** creada anteriormente.
1. En **Condiciones** haga clic en **Añadir** para abrir el asistente para **Configuración de condición**.
1. Para **Tipo de condición** seleccione **Código personalizado**.

   ![Código personalizado de condición de llamada a acción](assets/track-clicked-component/custom-code-condition.png)

1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   El código anterior primero comprueba si el tipo de recurso era de un **Botón** y luego comprueba si el tipo de recurso era de un CTA dentro de un **teaser**.

1. Guarde los cambios.

## Establecer variables de Analytics y señalización de vínculo de seguimiento de déclencheur

Actualmente, la regla **Llamada a acción: clic** simplemente genera una sentencia de consola. A continuación, utilice los elementos de datos y la extensión de Analytics para establecer las variables de Analytics como una **acción**. También estableceremos una acción adicional para el déclencheur del **vínculo de seguimiento** y enviaremos los datos recopilados a Adobe Analytics.

1. En la **CTA Clic** regla **eliminar** la acción **Core - Custom Code** (las sentencias de la consola):

   ![Eliminar acción de código personalizado](assets/track-clicked-component/remove-console-statements.png)

1. En Acciones, haga clic en **Añadir** para agregar una nueva acción.
1. Configure el tipo **Extension** en **Adobe Analytics** y establezca el **Tipo de acción** en **Establecer variables**.

1. Configure los siguientes valores para **eVars**, **Props** y **Eventos**:

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![Definir propiedades y eventos de eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Aquí `%Component ID%` se utiliza porque garantizará un identificador único para la llamada a acción en la que se hizo clic. Un posible inconveniente del uso de `%Component ID%` es que el informe de Analytics contendrá valores como `button-2e6d32893a`. El uso de `%Component Title%` daría un nombre más humano pero el valor podría no ser único.

1. A continuación, agregue una acción adicional a la derecha del icono **Adobe Analytics - Establecer variables** tocando el icono **más**:

   ![Añadir una acción de inicio adicional](assets/track-clicked-component/add-additional-launch-action.png)

1. Establezca el tipo **Extension** en **Adobe Analytics** y establezca el **Tipo de acción** en **Send Beacon**.
1. En **Seguimiento** establezca el botón de radio en **`s.tl()`**.
1. Para **Tipo de vínculo** elija **Vínculo personalizado** y para **Nombre del vínculo** establezca el valor en: **`%Component Title%: CTA Clicked`**:

   ![Configuración de la señalización Enviar vínculo](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Esto combinará la variable dinámica del elemento de datos **Título del componente** y la cadena estática **Llamada a acción: clic**.

1. Guarde los cambios. La regla **Llamada a acción: se hizo clic** ahora debe tener la siguiente configuración:

   ![Configuración de lanzamiento final](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Escucha el  `cmp:click` evento.
   * **2.** Compruebe que el evento se activó con un  **** teaser  **de botones**.
   * **3.** Configure las variables de Analytics para que rastreen el  **ID de** componente como un  **eVar**, una  **prop** y un  **evento**.
   * **4.** Envíe la señalización de vínculo de seguimiento de Analytics (y  **** no la considere una vista de página).

1. Guarde todos los cambios y cree su biblioteca de Launch, promocionándola al Entorno adecuado.

## Validar la señalización de vínculo de seguimiento y la llamada de Analytics

Ahora que la regla **Llamada a acción: clic** envía la señalización de Analytics, debería poder ver las variables de seguimiento de Analytics mediante el depurador de Experience Platform.

1. Abra el [sitio WKND](https://wknd.site/us/en.html) en su explorador.
1. Haga clic en el icono del depurador ![Depurador de la plataforma de experiencias](assets/track-clicked-component/experience-cloud-debugger.png) para abrir el depurador de Experience Platform.
1. Asegúrese de que el depurador está asignando la propiedad Launch a *el entorno de desarrollo*, como se ha descrito anteriormente, y **Registro de consola** está marcado.
1. Abra el menú Análisis y compruebe que el grupo de informes esté establecido en *su grupo de informes*.

   ![Depurador de fichas de Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. En el explorador, haga clic en uno de los botones **Teaser** o **Botón** CTA para navegar a otra página.

   ![Botón de llamada a acción para hacer clic](assets/track-clicked-component/cta-button-to-click.png)

1. Vuelva al depurador de Experience Platform y desplácese hacia abajo y expanda **Solicitudes de red** > *Su grupo de informes*. Debería poder encontrar el conjunto **eVar**, **prop** y **evento**.

   ![Eventos, eVar y prop de Analytics rastreados al hacer clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Vuelva al explorador y abra la consola de desarrollador. Vaya al pie de página del sitio y haga clic en uno de los vínculos de navegación:

   ![Haga clic en Vínculo de navegación en el pie de página](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observe en la consola del explorador el mensaje *&quot;Código personalizado&quot; de la regla &quot;Llamada a acción: clic&quot; no se cumplió*.

   Esto se debe a que el componente Navegación no déclencheur un evento `cmp:click` *pero* debido a nuestra comprobación del tipo de recurso, no se realiza ninguna acción.

   >[!NOTE]
   >
   > Si no ve ningún registro de consola, asegúrese de que **Registro de consola** está marcado en **Iniciar** en el depurador de Experience Platform.

## Felicitaciones!

Se acaba de utilizar la capa de datos y el Experience Platform Launch del cliente de Adobe, basados en evento, para rastrear los clics de componentes específicos en un sitio de Adobe Experience Manager.