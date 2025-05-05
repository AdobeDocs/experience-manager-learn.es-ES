---
title: Seguimiento de componentes en los que se hizo clic con Adobe Analytics
description: Utilice la capa de datos del cliente de Adobe impulsada por evento para rastrear clics de componentes específicos en un sitio de Adobe Experience Manager. Aprenda a utilizar las reglas de etiquetas para detectar estos eventos y enviar datos a un grupo de informes de Adobe Analytics mediante una señalización de seguimiento de vínculos.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-6296
thumbnail: KT-6296.jpg
badgeIntegration: label="Integración" type="positive"
doc-type: Tutorial
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
duration: 394
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1750'
ht-degree: 1%

---

# Seguimiento de componentes en los que se hizo clic con Adobe Analytics

Utilice la capa de datos del cliente de Adobe [impulsada por evento con componentes principales de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es) para rastrear clics de componentes específicos en un sitio de Adobe Experience Manager. Obtenga información sobre cómo utilizar las reglas en la propiedad tag para detectar eventos de clic, filtrar por componente y enviar los datos a un Adobe Analytics con una señalización de seguimiento de vínculos.

## Lo que va a generar {#what-build}

El equipo de marketing de WKND está interesado en saber qué botones de `Call to Action (CTA)` funcionan mejor en la página de inicio. En este tutorial, vamos a agregar una regla a la propiedad de etiqueta que escucha los eventos `cmp:click` de los componentes **Teaser** y **Button**. A continuación, envíe el ID de componente y un nuevo evento a Adobe Analytics junto con la señalización de seguimiento de vínculos.

![Lo que generará rastreará clics](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objetivos {#objective}

1. Cree una regla controlada por evento en la propiedad de etiqueta que capture el evento `cmp:click`.
1. Filtre los diferentes eventos por tipo de recurso de componente.
1. Establezca el ID de componente y envíe un evento a Adobe Analytics con la señalización de seguimiento de vínculos.

## Requisitos previos

Este tutorial es una continuación de [Recopilar datos de página con Adobe Analytics](./collect-data-analytics.md) y supone que tiene lo siguiente:

* Una **propiedad de etiquetas** con la [extensión de Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) habilitada
* **Adobe Analytics**: ID del grupo de informes de prueba/desarrollo y servidor de seguimiento. Consulte la siguiente documentación para [crear un grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* Extensión de explorador [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) configurada con su propiedad de etiquetas cargada en el [sitio WKND](https://wknd.site/us/es.html) o un sitio AEM con la capa de datos de Adobe habilitada.

## Inspeccionar el esquema de Botón y Teaser

Antes de crear reglas en la propiedad de etiqueta, es útil revisar el esquema [para Button y Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) e inspeccionarlos en la implementación de la capa de datos.

1. Navegar a [página principal de WKND](https://wknd.site/us/es.html)
1. Abra las herramientas para desarrolladores del explorador y vaya a la **Consola**. Ejecute el siguiente comando:

   ```js
   adobeDataLayer.getState();
   ```

   El código anterior devuelve el estado actual de la capa de datos del cliente de Adobe.

   ![Estado de capa de datos a través de la consola del explorador](assets/track-clicked-component/adobe-data-layer-state-browser.png)

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

   Los detalles de los datos anteriores se basan en el [Esquema de componente/elemento del contenedor](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). La nueva regla de etiquetas utiliza este esquema.

## Crear una regla en la que se hizo clic CTA

La capa de datos del cliente de Adobe es una capa de datos impulsada por **event**. Cada vez que se hace clic en un componente principal, se envía un evento `cmp:click` a través de la capa de datos. Para escuchar el evento `cmp:click`, vamos a crear una regla

1. Vaya a Experience Platform y a la propiedad de etiquetas integrada con el sitio de AEM.
1. Vaya a la sección **Reglas** en la interfaz de usuario de la propiedad de etiquetas y haga clic en **Agregar regla**.
1. Asigne un nombre a la regla **CTA donde se hizo clic**.
1. Haga clic en **Eventos** > **Agregar** para abrir el asistente de **Configuración de eventos**.
1. Para el campo **Tipo de evento**, seleccione **Código personalizado**.

   ![Asigne un nombre a la regla en la que CTA hizo clic y agregue el evento de código personalizado](assets/track-clicked-component/custom-code-event.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
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

   El fragmento de código anterior agrega un detector de eventos al [insertar una función](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) en la capa de datos. Cada vez que se activa el evento `cmp:click`, se llama a la función `componentClickedHandler`. En esta función, se agregan algunas comprobaciones y se construye un nuevo objeto `event` con el último [estado de la capa de datos](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) del componente que activó el evento.

   Finalmente se llama a la función `trigger(event)`. La función `trigger()` es un nombre reservado en la propiedad de etiqueta y **déclencheur** la regla. El objeto `event` se pasa como parámetro, que a su vez se expone con otro nombre reservado en la propiedad de etiqueta. Los elementos de datos de la propiedad de etiqueta ahora pueden hacer referencia a varias propiedades mediante fragmentos de código como `event.component['someKey']`.

1. Guarde los cambios.
1. A continuación, en **Acciones**, haga clic en **Agregar** para abrir el asistente de **Configuración de la acción**.
1. Para el campo **Tipo de acción**, elija **Código personalizado**.

   ![Tipo de acción de código personalizado](assets/track-clicked-component/action-custom-code.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   El objeto `event` se ha pasado desde el método `trigger()` llamado en el evento personalizado. El objeto `component` es el estado actual del componente derivado del método de la capa de datos `getState()` y es el elemento que activó el clic.

1. Guarde los cambios y ejecute una [compilación](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) en la propiedad de etiquetas para promocionar el código al [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=es) utilizado en su sitio de AEM.

   >[!NOTE]
   >
   > Puede resultar útil usar [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) para cambiar el código incrustado a un entorno **Development**.

1. Vaya al [sitio WKND](https://wknd.site/us/es.html) y abra las herramientas para desarrolladores para ver la consola. Seleccione también la casilla de verificación **Conservar registro**.

1. Haga clic en uno de los botones de CTA **Teaser** o **Button** para desplazarse a otra página.

   ![Botón de CTA donde hacer clic](assets/track-clicked-component/cta-button-to-click.png)

1. Observe en la consola del desarrollador que la regla **CTA Clicked** se ha activado:

   ![Se Hizo Clic En El Botón CTA](assets/track-clicked-component/cta-button-clicked-log.png)

## Crear elementos de datos

A continuación, cree elementos de datos para capturar el ID y el título del componente en el que se hizo clic. Recuerde que en el ejercicio anterior el resultado de `event.path` era similar a `component.button-b6562c963d` y el valor de `event.component['dc:title']` era similar a &quot;Ver viajes&quot;.

### ID de componente

1. Vaya a Experience Platform y a la propiedad de etiquetas integrada con el sitio de AEM.
1. Vaya a la sección **Elementos de datos** y haga clic en **Agregar nuevo elemento de datos**.
1. Para el campo **Nombre**, escriba **ID de componente**.
1. Para el campo **Tipo de elemento de datos**, seleccione **Código personalizado**.

   ![Formulario de elemento de datos de ID de componente](assets/track-clicked-component/component-id-data-element.png)

1. Haga clic en el botón **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. Guarde los cambios.

   >[!NOTE]
   >
   > Recuerde que el objeto `event` está disponible y con ámbito en función del evento que activó la **regla** en la propiedad de etiqueta. No se establece el valor de un elemento de datos hasta que se haga referencia al elemento de datos *1&rbrace; en una regla.* Por lo tanto, es seguro usar este elemento de datos dentro de una regla como la regla **Page Loaded** creada en el paso anterior *pero* no sería seguro usarla en otros contextos.


### Título del componente

1. Vaya a la sección **Elementos de datos** y haga clic en **Agregar nuevo elemento de datos**.
1. Para el campo **Nombre**, escriba **Título del componente**.
1. Para el campo **Tipo de elemento de datos**, seleccione **Código personalizado**.
1. Haga clic en el botón **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Guarde los cambios.

## Añadir una condición a la regla en la que se hizo clic en CTA

A continuación, actualice la regla **CTA Clicked** para asegurarse de que la regla solo se active cuando el evento `cmp:click` se active para un **Teaser** o un **Botón**. Dado que CTA del teaser se considera un objeto independiente en la capa de datos, es importante comprobar que el elemento principal para comprobar que proviene de un teaser.

1. En la interfaz de usuario de la propiedad Etiqueta, navegue hasta la regla **CTA en la que se hizo clic** creada anteriormente.
1. En **Condiciones**, haga clic en **Agregar** para abrir el asistente de **Configuración de condición**.
1. Para el campo **Tipo de condición**, seleccione **Código personalizado**.

   ![Código personalizado de condición en la que se hizo clic en CTA](assets/track-clicked-component/custom-code-condition.png)

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

   El código anterior primero comprueba si el tipo de recurso era de un **Button** o si el tipo de recurso era de un CTA dentro de un **Teaser**.

1. Guarde los cambios.

## Establecer variables de Analytics y señalización de seguimiento de vínculos de déclencheur

Actualmente, la regla **CTA Clicked** simplemente genera una instrucción de consola. A continuación, use los elementos de datos y la extensión de Analytics para establecer las variables de Analytics como una **acción**. Vamos a definir una acción adicional para almacenar en déclencheur el **vínculo de seguimiento** y enviar los datos recopilados a Adobe Analytics.

1. En la regla **CTA Clicked**, **quite** la acción **Core - Custom Code** (las instrucciones de la consola):

   ![Quitar acción de código personalizado](assets/track-clicked-component/remove-console-statements.png)

1. En Acciones, haga clic en **Agregar** para crear una acción.
1. Establezca el tipo de **Extension** en **Adobe Analytics** y establezca **Action Type** en **Set Variables**.

1. Establezca los siguientes valores para **eVars**, **Props** y **Events**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Establecer prop y eventos de eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Aquí se usa `%Component ID%`, ya que garantiza un identificador único para la CTA en la que se hizo clic. Un posible inconveniente de usar `%Component ID%` es que el informe de Analytics contiene valores como `button-2e6d32893a`. Si se usa `%Component Title%`, se daría un nombre más descriptivo pero el valor podría no ser único.

1. A continuación, agregue una acción adicional a la derecha de **Adobe Analytics - Set Variables** tocando el icono **plus**:

   ![Agregar una acción adicional a la regla de etiquetas](assets/track-clicked-component/add-additional-launch-action.png)

1. Establezca el tipo de **Extension** en **Adobe Analytics** y establezca **Action Type** en **Send Beacon**.
1. En **Seguimiento** establezca el botón de opción en **`s.tl()`**.
1. Para el campo **Tipo de vínculo**, elija **Vínculo personalizado** y para **Nombre de vínculo** establezca el valor en: **`%Component Title%: CTA Clicked`**:

   ![Configuración de la señalización de vínculo de envío](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   La configuración anterior combina la variable dinámica del elemento de datos **Component Title** y la cadena estática **CTA Clicked**.

1. Guarde los cambios. La regla **CTA en la que se hizo clic** debe tener la configuración siguiente:

   ![Configuración final de la regla de etiquetas](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Escuche el evento `cmp:click`.
   * **2.** Compruebe que el evento se activó mediante un **botón** o un **teaser**.
   * **3.** configuró variables de Analytics para rastrear el **ID de componente** como **eVar**, **prop** y un **evento**.
   * **4.** envía la señalización de seguimiento de vínculos de Analytics (y **no** la trata como una vista de página).

1. Guarde todos los cambios y cree su biblioteca de etiquetas, promocionando al entorno adecuado.

## Validación de la señalización de seguimiento de vínculos y la llamada de Analytics

Ahora que la regla **CTA ha hecho clic** envía la señalización de Analytics, debería poder ver las variables de seguimiento de Analytics mediante Experience Platform Debugger.

1. Abra el [sitio WKND](https://wknd.site/us/es.html) en su explorador.
1. Haga clic en el icono de Debugger ![Experience Platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) para abrir Experience Platform Debugger.
1. Asegúrese de que Debugger asigne la propiedad de etiqueta a *su entorno de desarrollo*, tal como se describió anteriormente, y de que se compruebe **Registro de consola**.
1. Abra el menú Analytics y verifique que el grupo de informes esté establecido en *su* grupo de informes.

   ![Debugger de ficha de Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. En el explorador, haga clic en uno de los botones de CTA **Teaser** o **Button** para desplazarse a otra página.

   ![Botón de CTA donde hacer clic](assets/track-clicked-component/cta-button-to-click.png)

1. Vuelva a Experience Platform Debugger y desplácese hacia abajo para expandir **Solicitudes de red** > *Su grupo de informes*. Debería poder encontrar el conjunto **eVar**, **prop** y **event**.

   ![Eventos, evar y prop de Analytics rastreados al hacer clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Vuelva al explorador y abra la consola de desarrollador. Navegue hasta el pie de página del sitio y haga clic en uno de los vínculos de navegación:

   ![Haga clic en Vínculo de navegación en el pie de página](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observe que en la consola del explorador no se cumplió el mensaje *&quot;Código personalizado&quot; para la regla &quot;CTA en el que se hizo clic&quot;*.

   El mensaje anterior se debe a que el componente de navegación no almacena en déclencheur un evento `cmp:click` *pero* debido a [Condición de la regla](#add-a-condition-to-the-cta-clicked-rule) que comprueba el tipo de recurso para la que no se realiza ninguna acción.

   >[!NOTE]
   >
   > Si no ve ningún registro de consola, asegúrese de que **Registro de consola** esté marcado en **Etiquetas de Experience Platform** en Experience Platform Debugger.

## Enhorabuena.

Acaba de utilizar la capa de datos del cliente de Adobe impulsada por evento y la etiqueta de Experience Platform para rastrear los clics de componentes específicos en un sitio de AEM.
