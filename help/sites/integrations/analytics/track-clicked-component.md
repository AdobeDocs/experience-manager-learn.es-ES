---
title: Seguimiento del componente en el que se hizo clic con Adobe Analytics
description: Utilice la capa de datos del cliente de Adobe impulsada por eventos para rastrear clics de componentes específicos en un sitio de Adobe Experience Manager. Aprenda a utilizar las reglas en Experience Platform Launch para detectar estos eventos y enviar datos a un Adobe Analytics con una señalización de seguimiento de vínculos.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1806'
ht-degree: 3%

---

# Seguimiento del componente en el que se hizo clic con Adobe Analytics

Uso impulsado por eventos [Adobe de la capa de datos del cliente con AEM componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es) para rastrear clics de componentes específicos en un sitio de Adobe Experience Manager. Obtenga información sobre cómo utilizar las reglas en Experience Platform Launch para detectar eventos de clic, filtrar por componente y enviar los datos a un Adobe Analytics con una señalización de seguimiento de vínculos.

## Qué va a generar

El equipo de marketing de WKND desea saber qué botones de Llamada a acción (CTA) tienen el mejor rendimiento en la página principal. En este tutorial, agregaremos una nueva regla en Experience Platform Launch que espere `cmp:click` eventos desde **Teaser** y **Botón** y envíe el ID de componente y un nuevo evento a Adobe Analytics junto con la señalización de seguimiento de vínculos.

![Qué generará para rastrear clics](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objetivos {#objective}

1. Cree una regla impulsada por eventos en Launch basada en la variable `cmp:click` evento.
1. Filtre los diferentes eventos por tipo de recurso de componente.
1. Establezca el id de componente en el que se hizo clic y envíe Adobe Analytics de evento con la señalización de seguimiento de vínculos.

## Requisitos previos

Este tutorial es una continuación de [Recopilación de datos de página con Adobe Analytics](./collect-data-analytics.md) y supone que:

* A **Propiedad Launch** con la variable [Extensión de Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) enabled
* **Adobe Analytics** ID del grupo de informes de prueba/desarrollo y servidor de seguimiento. Consulte la siguiente documentación para [crear un nuevo grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) extensión del explorador configurada con la propiedad de Launch cargada en [https://wknd.site/us/en.html](https://wknd.site/us/en.html) o un sitio AEM con la capa de datos de Adobe habilitada.

## Esquema Botón y teaser de Inspect

Antes de crear reglas en Launch, es útil revisar la [esquema para Button y Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) e inspecciónelos en la implementación de la capa de datos.

1. Vaya a [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Abra las herramientas para desarrolladores del explorador y vaya a la **Consola**. Ejecute el siguiente comando:

   ```js
   adobeDataLayer.getState();
   ```

   Esto devuelve el estado actual de la capa de datos del cliente de Adobe.

   ![Estado de la capa de datos a través de la consola del explorador](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Expanda la respuesta y busque las entradas con el prefijo `button-` y  `teaser-xyz-cta` entrada. Debería ver un esquema de datos como el siguiente:

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

   Se basan en la variable [Esquema de elemento de componente/contenedor](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). La regla que crearemos en Launch utilizará este esquema.

## Creación de una regla en la que se hizo clic en CTA

La capa de datos del cliente de Adobe es una **evento** capa de datos controlada. Cuando se hace clic en un componente principal, se llama a una función `cmp:click` se envía mediante la capa de datos. A continuación, cree una regla para escuchar la `cmp:click` evento.

1. Vaya al Experience Platform Launch y a la propiedad web integrada con el sitio AEM.
1. Vaya a la **Reglas** en la interfaz de usuario de Launch y, a continuación, haga clic en **Agregar regla**.
1. Asigne un nombre a la regla **Llamada a acción: clic**.
1. Haga clic en **Eventos** > **Agregar** para abrir el **Configuración de eventos** asistente.
1. En **Tipo de evento** select **Código personalizado**.

   ![Asigne un nombre a la regla en la que se hizo clic y añada el evento de código personalizado .](assets/track-clicked-component/custom-code-event.png)

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

   El fragmento de código anterior añade un detector de eventos por [inserción de una función](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) en la capa de datos. Cuando la variable `cmp:click` se activa el evento `componentClickedHandler` llamada a . En esta función se añaden algunas comprobaciones de coherencia y se añade una nueva `event` se construye con el último [estado de la capa de datos](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para el componente que activó el evento.

   Después de eso `trigger(event)` se llama. `trigger()` es un nombre reservado en Launch y &quot;déclencheur&quot; en la regla de Launch. Pasamos el `event` objeto como parámetro que, a su vez, está expuesto por otro nombre reservado en Launch denominado `event`. Los elementos de datos de Launch ahora pueden hacer referencia a varias propiedades como esta: `event.component['someKey']`.

1. Guarde los cambios.
1. Siguiente en **Acciones** click **Agregar** para abrir el **Configuración de la acción** asistente.
1. En **Tipo de acción** elija **Código personalizado**.

   ![Tipo de acción de código personalizado](assets/track-clicked-component/action-custom-code.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   La variable `event` se pasa desde el `trigger()` método llamado en el evento personalizado. `component` es el estado actual del componente derivado de la capa de datos `getState` que activaron el clic.

1. Guarde los cambios y ejecute un [versión](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) en Launch para promocionar el código a la variable [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) en su sitio AEM.

   >[!NOTE]
   >
   > Puede ser muy útil usar la variable [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) para cambiar el código incrustado a un **Desarrollo** entorno.

1. Vaya a la [Sitio WKND](https://wknd.site/us/en.html) y abra las herramientas para desarrolladores para ver la consola. Select **Mantener registro**.

1. Haga clic en una de las **Teaser** o **Botón** Botones CTA para desplazarse a otra página.

   ![Botón CTA para hacer clic](assets/track-clicked-component/cta-button-to-click.png)

1. Observe en la consola del desarrollador que la variable **Llamada a acción: clic** se ha activado la regla:

   ![Botón CTA en el que se hizo clic](assets/track-clicked-component/cta-button-clicked-log.png)

## Crear elementos de datos

A continuación, cree un elemento de datos para capturar el ID y el título del componente en el que se hizo clic. Recuerde en el ejercicio anterior el resultado de `event.path` era algo similar a `component.button-b6562c963d` y el valor de `event.component['dc:title']` era algo así como &quot;Viaje&quot;.

### ID de componente

1. Vaya al Experience Platform Launch y a la propiedad web integrada con el sitio AEM.
1. Vaya a la **Elementos de datos** y haga clic en **Añadir nuevo elemento de datos**.
1. Para **Nombre** enter **ID de componente**.
1. Para **Tipo de elemento de datos** select **Código personalizado**.

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
   > Recuerde que la variable `event` El objeto está disponible y tiene ámbitos basados en el evento que activó la variable **Regla** en Launch. El valor de un elemento de datos no se establece hasta que el elemento de datos *referenciado* dentro de una regla. Por lo tanto, es seguro utilizar este elemento de datos dentro de una regla como la **Llamada a acción: clic** regla creada en el ejercicio anterior *but* no sería seguro usar en otros contextos.

### Título de componente

1. Vaya a la **Elementos de datos** y haga clic en **Añadir nuevo elemento de datos**.
1. Para **Nombre** enter **Título de componente**.
1. Para **Tipo de elemento de datos** select **Código personalizado**.
1. Haga clic en **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Guarde los cambios.

## Añadir una condición a la regla de llamada a acción en la que se ha hecho clic

A continuación, actualice la variable **Llamada a acción: clic** para garantizar que la regla solo se active cuando la variable `cmp:click` se activa para **Teaser** o **Botón**. Dado que el CTA del teaser se considera un objeto independiente en la capa de datos, es importante comprobar el objeto principal para verificar que proviene de un teaser.

1. En la interfaz de usuario de Launch, vaya a la **Llamada a acción: clic** regla creada anteriormente.
1. En **Condiciones** click **Agregar** para abrir el **Configuración de condición** asistente.
1. Para **Tipo de condición** select **Código personalizado**.

   ![Código personalizado de condición de clic en CTA](assets/track-clicked-component/custom-code-condition.png)

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

   El código anterior primero comprueba si el tipo de recurso era de un **Botón** y, a continuación, comprueba si el tipo de recurso era de una CTA dentro de un **Teaser**.

1. Guarde los cambios.

## Establecimiento de variables de Analytics y señalización de vínculo de seguimiento de déclencheur

Actualmente, la variable **Llamada a acción: clic** la regla simplemente genera una sentencia de consola. A continuación, utilice los elementos de datos y la extensión de Analytics para establecer las variables de Analytics como un **acción**. También estableceremos una acción adicional para el déclencheur del **Seguimiento de vínculos** y enviar los datos recopilados a Adobe Analytics.

1. En el **Llamada a acción: clic** regla **remove** el **Core - Custom Code** acción (las instrucciones de consola):

   ![Eliminar acción de código personalizado](assets/track-clicked-component/remove-console-statements.png)

1. En Acciones (Actions), haga clic en **Agregar** para agregar una nueva acción.
1. Configure las variables **Extensión** escriba a **Adobe Analytics** y establezca la variable **Tipo de acción** a  **Establecer variables**.

1. Establezca los siguientes valores para **eVars**, **Props** y **Eventos**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Configurar Prop y eventos de eVar](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Aquí `%Component ID%` se utiliza , ya que garantiza un identificador único para el CTA en el que se hizo clic. Una desventaja potencial del uso de `%Component ID%` es que el informe de Analytics contendrá valores como `button-2e6d32893a`. Uso `%Component Title%` daría un nombre más descriptivo, pero el valor podría no ser único.

1. A continuación, añada una acción adicional a la derecha del **Adobe Analytics: Establecer variables** tocando el botón **plus** icono:

   ![Añadir una acción de Launch adicional](assets/track-clicked-component/add-additional-launch-action.png)

1. Configure las variables **Extensión** escriba a **Adobe Analytics** y establezca la variable **Tipo de acción** a  **Send Beacon**.
1. En **Seguimiento** establezca el botón de radio en **`s.tl()`**.
1. Para **Tipo de vínculo** elija **Vínculo personalizado** y para **Nombre del vínculo** establezca el valor en: **`%Component Title%: CTA Clicked`**:

   ![Configuración de la señalización de Send Link](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Esto combinará la variable dinámica del elemento de datos **Título de componente** y la cadena estática **Llamada a acción: clic**.

1. Guarde los cambios. La variable **Llamada a acción: clic** ahora debe tener la siguiente configuración:

   ![Configuración de lanzamiento final](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Escuche el `cmp:click` evento.
   * **2.** Compruebe que el evento se activó mediante una **Botón** o **Teaser**.
   * **3.** Configure las variables de Analytics para para que hagan un seguimiento de la variable **ID de componente** como **eVar**, **prop** y **evento**.
   * **4.** Envíe la señalización del vínculo de seguimiento de Analytics (y haga clic en **not** tratarla como una vista de página).

1. Guarde todos los cambios y cree la biblioteca de Launch, promociéndola al entorno adecuado.

## Validación de la señalización del vínculo de seguimiento y la llamada de Analytics

Ahora que la variable **Llamada a acción: clic** envía la señalización de Analytics, debería poder ver las variables de seguimiento de Analytics mediante Experience Platform Debugger.

1. Abra el [Sitio WKND](https://wknd.site/us/en.html) en su navegador.
1. Haga clic en el icono de Debugger ![Icono de Experience Platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) para abrir Experience Platform Debugger.
1. Asegúrese de que Debugger asigne la propiedad de Launch a *your* Entorno de desarrollo, tal como se describió anteriormente y **Registro de consola** está activada.
1. Abra el menú Analytics y compruebe que el grupo de informes está configurado en *your* grupo de informes.

   ![Depurador de pestañas de Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. En el explorador, haga clic en uno de los **Teaser** o **Botón** Botones CTA para desplazarse a otra página.

   ![Botón CTA para hacer clic](assets/track-clicked-component/cta-button-to-click.png)

1. Vuelva a Experience Platform Debugger y desplácese hacia abajo y amplíe **Solicitudes de red** > *Su grupo de informes*. Debería poder encontrar la variable **eVar**, **prop** y **evento** configurado.

   ![Eventos, evar y prop de Analytics rastreados al hacer clic](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Vuelva al explorador y abra la consola del desarrollador. Vaya al pie de página del sitio y haga clic en uno de los vínculos de navegación:

   ![Haga clic en Vínculo de navegación en el pie de página](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observe el mensaje en la consola del explorador *&quot;Código personalizado&quot; para la regla &quot;Llamada a acción: clic&quot; no se cumplió*.

   Esto se debe a que el componente de navegación sí déclencheur un `cmp:click` evento *but* debido a nuestra comprobación de los en contra del tipo de recurso, no se realiza ninguna acción.

   >[!NOTE]
   >
   > Si no ve ningún registro de consola, asegúrese de que **Registro de consola** está marcado en **Launch** en Experience Platform Debugger.

## Felicitaciones!

Solo ha utilizado la capa de datos del cliente de Adobe impulsada por eventos y el Experience Platform Launch para rastrear clics de componentes específicos en un sitio de Adobe Experience Manager.
