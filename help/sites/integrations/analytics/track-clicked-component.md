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
source-git-commit: 096cdccdf1675480aa0a35d46ce7b62a3906dad1
workflow-type: tm+mt
source-wordcount: '1831'
ht-degree: 1%

---


# Seguimiento del componente en el que se hizo clic con Adobe Analytics

Utilice la capa de datos del cliente de [Adobe impulsada por evento con AEM componentes](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/data-layer/overview.html) principales para rastrear los clics de componentes específicos en un sitio de Adobe Experience Manager. Obtenga información sobre cómo utilizar las reglas en Experience Platform Launch para detectar eventos de clics, filtrar por componente y enviar los datos a un Adobe Analytics con una señalización de seguimiento de vínculos.

## Qué va a generar

El equipo de mercadotecnia de WKND desea saber qué botones de llamada a acción (CTA) tienen el mejor rendimiento en la página de inicio. En este tutorial, agregaremos una nueva regla en Experience Platform Launch que escuche `cmp:click` eventos de los componentes **Teaser** y **Button** y envíe el ID del componente y un nuevo evento a Adobe Analytics junto con la señalización de seguimiento de vínculos.

![Qué generará los clics de seguimiento](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objetivos {#objective}

1. Cree una regla basada en eventos en Launch según el `cmp:click` evento.
1. Filtre los diferentes eventos por tipo de recurso de componente.
1. Configure la identificación del componente en la que se hizo clic y envíe evento Adobe Analytics con la señalización de seguimiento de vínculos.

## Requisitos previos

Este tutorial es una continuación de [Recopilación de datos de página con Adobe Analytics](./collect-data-analytics.md) y supone que:

* Una propiedad **** Launch con la extensión [](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) Adobe Analytics activada
* **ID del grupo de informes de prueba/desarrollo de Adobe Analytics** y servidor de seguimiento. Consulte la siguiente documentación para [crear un nuevo grupo](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)de informes.
* [Extensión del navegador Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) configurada con la propiedad Launch cargada en [https://wknd.site/us/en.html](https://wknd.site/us/en.html) o un sitio AEM con la capa de datos de Adobe activada.

## Inspect el Esquema Botón y Teaser

Antes de crear reglas en Launch, es útil revisar el [esquema del botón y el teaser](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) e inspeccionarlos en la implementación de la capa de datos.

1. Vaya a [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Abra las herramientas de desarrollador del navegador y vaya a la **consola**. Ejecute el siguiente comando:

   ```js
   adobeDataLayer.getState();
   ```

   Esto devuelve el estado actual de la capa de datos del cliente de Adobe.

   ![Estado de la capa de datos mediante la consola del navegador](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Expanda la respuesta y busque las entradas con el prefijo `button-` y la `teaser-xyz-cta` entrada. Debería ver un esquema de datos como el siguiente:

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
       @type: "nt:unstructured"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Se basan en el Esquema [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)Componente/Elemento de Contenedor. La regla que crearemos en Launch utilizará este esquema.

## Crear una regla de llamada a acción en la que se hizo clic

La capa de datos del cliente de Adobe es una capa de datos controlada por **evento** . Cuando se hace clic en cualquier componente principal, se envía un `cmp:click` evento a través de la capa de datos. A continuación, cree una regla para escuchar el `cmp:click` evento.

1. Vaya al Experience Platform Launch y a la propiedad Web integrada con el sitio AEM.
1. Vaya a la sección **Reglas** en la interfaz de usuario de Launch y, a continuación, haga clic en **Añadir regla**.
1. Asigne un nombre a la regla **Llamada a acción**.
1. Haga clic en **Eventos** > **Añadir** para abrir el asistente de configuración **de** Evento.
1. En **Tipo de evento** , seleccione Código **** personalizado.

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

   El fragmento de código anterior agregará un detector de eventos [insertando una función](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) en la capa de datos. Cuando se activa el `cmp:click` evento, se llama a la `componentClickedHandler` función. En esta función se añaden algunas comprobaciones de integridad y se construye un nuevo `event` objeto con el [estado más reciente de la capa](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) de datos para el componente que activó el evento.

   Después de eso `trigger(event)` se llama. `trigger()` es un nombre reservado en Launch y &quot;activará&quot; la regla de inicio. Pasamos el `event` objeto como un parámetro que, a su vez, será expuesto por otro nombre reservado en Launch denominado `event`. Los elementos de datos de Launch ahora pueden hacer referencia a varias propiedades como, por ejemplo: `event.component['someKey']`.

1. Guarde los cambios.
1. A continuación, en **Acciones** , haga clic en **Añadir** para abrir el Asistente para configuración **de** acciones.
1. En Tipo **de** acción, elija Código **** personalizado.

   ![Tipo de acción de código personalizado](assets/track-clicked-component/action-custom-code.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   El `event` objeto se pasa desde el `trigger()` método al que se llama en el evento personalizado. `component` es el estado actual del componente derivado de la capa de datos `getState` que activó el clic.

1. Guarde los cambios y ejecute una [compilación](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) en Launch para promocionar el código al [entorno](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) utilizado en el sitio AEM.

   >[!NOTE]
   >
   > Puede resultar muy útil utilizar [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) para cambiar el código incrustado a un entorno de **desarrollo** .

1. Vaya al [sitio](https://wknd.site/us/en.html) WKND y abra las herramientas de desarrollador para la vista de la consola. Seleccione **Conservar registro**.

1. Haga clic en uno de los botones **Teaser** o **Botón** de llamada a acción para desplazarse a otra página.

   ![Botón de llamada a acción para hacer clic](assets/track-clicked-component/cta-button-to-click.png)

1. Observe en la consola del desarrollador que se ha activado la regla **Llamada a acción** :

   ![Botón CTA en el que se hizo clic](assets/track-clicked-component/cta-button-clicked-log.png)

## Crear elementos de datos

A continuación, cree un elemento de datos para capturar el ID del componente y el título en los que se hizo clic. Recuerden que en el ejercicio anterior el resultado de `event.path` era algo similar a `component.button-b6562c963d` y el valor de `event.component['dc:title']` era algo como &quot;Viajes de Vista&quot;.

### ID del componente

1. Vaya al Experience Platform Launch y a la propiedad Web integrada con el sitio AEM.
1. Vaya a la sección Elementos **de** datos y haga clic en **Añadir nuevo elemento** de datos.
1. Para **Nombre** , introduzca el ID **del** componente.
1. Para Tipo **de elemento** de datos, seleccione Código **** personalizado.

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
   > Recuerde que el `event` objeto está disponible y tiene un ámbito basado en el evento que activó la **regla** en Launch. El valor de un elemento de datos no se establece hasta que se *hace referencia* al elemento de datos dentro de una regla. Por lo tanto, es seguro utilizar este elemento de datos dentro de una regla como la regla de clics **en** llamada a acción creada en el ejercicio anterior *pero* no sería seguro usar en otros contextos.

### Título del componente

1. Vaya a la sección Elementos **de** datos y haga clic en **Añadir nuevo elemento** de datos.
1. En **Nombre** , introduzca Título **** del componente.
1. Para Tipo **de elemento** de datos, seleccione Código **** personalizado.
1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Guarde los cambios.

## Añadir una condición a la regla de llamada a acción en la que se hizo clic

A continuación, actualice la regla **Llamada a acción: Se hizo clic** para asegurarse de que la regla solo se activa cuando se activa el `cmp:click` evento para un **teaser** o un **botón**. Dado que el CTA del teaser se considera un objeto independiente en la capa de datos, es importante comprobar el elemento principal para verificar que proviene de un teaser.

1. En la interfaz de usuario de Launch, vaya a la regla **Página cargada** creada anteriormente.
1. En **Condiciones** , haga clic en **Añadir** para abrir el asistente de Configuración **de** condición.
1. Para Tipo **de** condición, seleccione Código **** personalizado.

   ![Código personalizado de condición de llamada a acción](assets/track-clicked-component/custom-code-condition.png)

1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       //Check for Button Type
       if(event.component['@type'] === 'wknd/components/button') {
           return true;
       } else if (event.component['@type'] == 'nt:unstructured') {
           // Check for CTA inside a Teaser
           var parentComponentId = event.component['parentId'];
           var parentComponent = window.adobeDataLayer.getState('component.' + parentComponentId);
   
           if(parentComponent['@type'] === 'wknd/components/teaser') {
               return true;
           }
       }
   }
   
   return false;
   ```

   El código anterior primero comprueba si el tipo de recurso era de un **Botón** y, a continuación, comprueba si el tipo de recurso era de un CTA dentro de un **teaser**.

1. Guarde los cambios.

## Establecer variables de Analytics y activar la señalización de vínculo de seguimiento

Actualmente, la regla **Llamada a acción: Clic** simplemente genera una sentencia de consola. A continuación, utilice los elementos de datos y la extensión de Analytics para definir las variables de Analytics como una **acción**. También estableceremos una acción adicional para activar el **vínculo** de seguimiento y enviar los datos recopilados a Adobe Analytics.

1. En la regla **Página cargada** , **elimine** la acción **Principal - Código** personalizado (las sentencias de la consola):

   ![Eliminar acción de código personalizado](assets/track-clicked-component/remove-console-statements.png)

1. En Acciones, haga clic en **Añadir** para agregar una nueva acción.
1. Defina el tipo de **extensión** en **Adobe Analytics** y defina el tipo **de** acción en **Establecer variables**.

1. Establezca los siguientes valores para **eVars**, **Props** y **Eventos**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Definir propiedades y eventos de eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Aquí `%Component ID%` se utiliza, ya que garantiza un identificador único para la llamada a acción en la que se hizo clic. Un posible inconveniente de usar `%Component ID%` es que el informe de Analytics contendrá valores como `button-2e6d32893a`. Usar `%Component Title%` daría un nombre más humano pero el valor podría no ser único.

1. A continuación, agregue una acción adicional a la derecha de las variables **de** Adobe Analytics - Establecer tocando el icono de **signo más** :

   ![Añadir una acción de inicio adicional](assets/track-clicked-component/add-additional-launch-action.png)

1. Establezca el tipo de **extensión** en **Adobe Analytics** y el tipo **de** acción en **Enviar señalización**.
1. En **Seguimiento** , establezca el botón de radio en **`s.tl()`**.
1. Para Tipo **de** vínculo, elija Vínculo **** personalizado y, para Nombre **del** vínculo, defina el valor en: **`%Component Title%: CTA Clicked`**::

   ![Configuración de la señalización Enviar vínculo](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Esto combinará la variable dinámica del elemento de datos Título **del** componente y la cadena estática **Llamada a acción: Clic**.

1. Guarde los cambios. La regla **Llamada a acción: Clic** ahora debe tener la siguiente configuración:

   ![Configuración de lanzamiento final](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Escucha el `cmp:click` evento.
   * **2.** Compruebe que el evento se activó con un **Botón** o un **teaser**.
   * **3.** Configure las variables de Analytics para que rastreen el ID **de** componente como un **eVar**, una **prop** y un **evento**.
   * **4.** Envíe la señalización de vínculo de seguimiento de Analytics (y **no la trate** como una vista de página).

1. Guarde todos los cambios y cree su biblioteca de Launch, promocionándola al Entorno adecuado.

## Validar la señalización de vínculo de seguimiento y la llamada de Analytics

Ahora que la regla de clics **en** llamada a acción envía la señalización de Analytics, debería poder ver las variables de seguimiento de Analytics mediante el depurador de Experience Platform.

1. Abra el [sitio](https://wknd.site/us/en.html) WKND en su explorador.
1. Haga clic en el icono del depurador ![Experience platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) para abrir el depurador de Experience Platform.
1. Asegúrese de que el depurador está asignando la propiedad Launch a *su* entorno de desarrollo, tal como se ha descrito anteriormente, y de que el Registro **de** consola está marcado.
1. Abra el menú Análisis y compruebe que el grupo de informes esté establecido en *su grupo* de informes.

   ![Depurador de fichas de Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. En el navegador, haga clic en uno de los botones **Teaser** o **Botón** de llamada a acción para navegar a otra página.

   ![Botón de llamada a acción para hacer clic](assets/track-clicked-component/cta-button-to-click.png)

1. Vuelva al depurador de Experience Platform y desplácese hacia abajo y expanda Solicitudes **de** red > *Su grupo* de informes. Debería poder encontrar el **eVar**, la **prop** y el conjunto de **eventos** .

   ![Eventos, eVar y prop de Analytics rastreados al hacer clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Vuelva al explorador y abra la consola de desarrollador. Vaya al pie de página del sitio y haga clic en uno de los vínculos de navegación:

   ![Haga clic en Vínculo de navegación en el pie de página](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observe en la consola del explorador el mensaje *&quot;Código personalizado&quot; de la regla &quot;Llamada a acción: clic&quot; no se cumplió*.

   Esto se debe a que el componente Navegación activa un `cmp:click` evento *pero* debido a nuestra comprobación del tipo de recurso no se realiza ninguna acción.

   >[!NOTE]
   >
   > Si no ve ningún registro de consola, asegúrese de que el Registro **de** consola está marcado en **Iniciar** en el depurador Experience Platform.

## Felicitaciones!

Se acaba de utilizar la capa de datos y el Experience Platform Launch del cliente de Adobe, basados en evento, para rastrear los clics de componentes específicos en un sitio de Adobe Experience Manager.