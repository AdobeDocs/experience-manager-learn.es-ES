---
title: Integración de AEM Sites con Adobe Analytics con la extensión de etiquetas de Adobe Analytics
description: Integre AEM Sites con Adobe Analytics mediante la capa de datos del cliente de Adobe impulsada por evento para recopilar datos sobre la actividad del usuario en un sitio web creado con Adobe Experience Manager. Aprenda a utilizar las reglas de etiquetas para detectar estos eventos y enviar datos a un grupo de informes de Adobe Analytics.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="Integración" type="positive"
doc-type: Tutorial
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
duration: 490
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2262'
ht-degree: 2%

---

# Integración de AEM Sites y Adobe Analytics

Aprenda a integrar AEM Sites y Adobe Analytics con la extensión de etiquetas de Adobe Analytics, utilizando las funciones integradas de la [capa de datos del cliente de Adobe con los componentes principales de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es) para recopilar datos sobre una página en Adobe Experience Manager Sites. Las etiquetas [Tags de Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=es) y la [extensión de Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=es) se usan para crear reglas para enviar datos de página a Adobe Analytics.

## Lo que va a generar {#what-build}

![Seguimiento de datos de página](assets/collect-data-analytics/analytics-page-data-tracking.png)

En este tutorial, va a almacenar en déclencheur una regla de etiquetas basada en un evento de la capa de datos del cliente de Adobe. Además, agregue condiciones para cuándo se debe activar la regla y, a continuación, envíe los valores **Nombre de página** y **Plantilla de página** de una página AEM a Adobe Analytics.

### Objetivos {#objective}

1. Cree una regla impulsada por evento en la propiedad de etiqueta que capture los cambios de la capa de datos
1. Asignar propiedades de capa de datos de página a elementos de datos en la propiedad de etiqueta
1. Recopilación y envío de datos de página a Adobe Analytics mediante la señalización de vista de página

## Requisitos previos

Se requiere lo siguiente:

* **Propiedad de etiqueta** en Experience Platform
* **Adobe Analytics**: ID del grupo de informes de prueba/desarrollo y servidor de seguimiento. Consulte la siguiente documentación para [crear un grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=es).
* Extensión de explorador [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=es). Capturas de pantalla de este tutorial capturadas desde el explorador Chrome.
* (Opcional) Sitio de AEM con la [capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es#installation-activation). Este tutorial utiliza el sitio público [WKND](https://wknd.site/us/es.html), pero puede usar su propio sitio.

>[!NOTE]
>
> ¿Necesita ayuda con la integración de la propiedad de etiquetas y el sitio de AEM? [Ver esta serie de vídeos](../experience-platform/data-collection/tags/overview.md).

## Cambiar el entorno de etiquetas para el sitio WKND

[WKND](https://wknd.site/us/es.html) es un sitio público creado a partir de [un proyecto de código abierto](https://github.com/adobe/aem-guides-wknd) diseñado como referencia y [tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es) para una implementación de AEM.

En lugar de configurar un entorno de AEM e instalar la base de código WKND, puede usar el depurador de Experience Platform para **cambiar** el [sitio WKND](https://wknd.site/us/es.html) activo a *su propiedad de etiquetas*. Sin embargo, puede usar su propio sitio de AEM si ya tiene habilitada la [capa de datos del cliente de Adobe](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es#installation-activation).

1. Inicie sesión en Experience Platform y [cree una propiedad de etiquetas](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=es) (si aún no lo ha hecho).
1. Asegúrese de que se ha creado una biblioteca [JavaScript &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html?lang=es#create-a-library) de etiquetas inicial y de que se ha promocionado a la etiqueta [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=es).
1. Copie el código incrustado de JavaScript del entorno de etiquetas en el que se ha publicado la biblioteca.

   ![Copiar código incrustado de propiedad de etiqueta](assets/collect-data-analytics/launch-environment-copy.png)

1. En el explorador, abra una pestaña nueva y vaya al [Sitio WKND](https://wknd.site/us/es.html)
1. Abra la extensión del explorador de Experience Platform Debugger.

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Vaya a **Etiquetas Experience Platform** > **Configuración** y en **Códigos incrustados insertados** reemplace el código incrustado existente por *su código incrustado* copiado del paso 3.

   ![Reemplazar código incrustado](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Habilite **Registro de consola** y **Bloquee** el depurador en la ficha WKND.

   ![Registro de consola](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verificar la capa de datos del cliente de Adobe en el sitio WKND

El [proyecto de referencia WKND](https://github.com/adobe/aem-guides-wknd) se ha creado con los componentes principales de AEM y tiene la [capa de datos del cliente de Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es#installation-activation) de forma predeterminada. A continuación, compruebe que la capa de datos del cliente de Adobe esté habilitada.

1. Vaya a [Sitio WKND](https://wknd.site/us/es.html).
1. Abra las herramientas para desarrolladores del explorador y vaya a la **Consola**. Ejecute el siguiente comando:

   ```js
   adobeDataLayer.getState();
   ```

   El código anterior devuelve el estado actual de la capa de datos del cliente de Adobe.

   ![Estado de la capa de datos de Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Expanda la respuesta e inspeccione la entrada `page`. Debería ver un esquema de datos como el siguiente:

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

   Para enviar datos de página a Adobe Analytics, usemos propiedades estándar como `dc:title`, `xdm:language` y `xdm:template` de la capa de datos.

   Para obtener más información, revise el [esquema de página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es#page) de los esquemas de datos de los componentes principales.

   >[!NOTE]
   >
   > ¿Si no ve el objeto JavaScript `adobeDataLayer`? Asegúrese de que la [capa de datos del cliente de Adobe se ha habilitado](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es#installation-activation) en el sitio.

## Crear una regla de carga de página

La capa de datos del cliente de Adobe es una capa de datos **impulsada por evento**. Cuando se carga la capa de datos de la página de AEM, se déclencheur un evento `cmp:show`. Cree una regla que se active cuando el evento `cmp:show` se active desde la capa de datos de página.

1. Vaya a Experience Platform y a la propiedad de etiquetas integrada con el sitio de AEM.
1. Vaya a la sección **Reglas** en la interfaz de usuario de la propiedad de etiquetas y haga clic en **Crear nueva regla**.

   ![Crear regla](assets/collect-data-analytics/analytics-create-rule.png)

1. Asigne un nombre a la regla **Página cargada**.
1. En la subsección **Eventos**, haga clic en **Agregar** para abrir el asistente **Configuración de eventos**.
1. Para el campo **Tipo de evento**, seleccione **Código personalizado**.

   ![Asigne un nombre a la regla y agregue el evento de código personalizado](assets/collect-data-analytics/custom-code-event.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

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

   El fragmento de código anterior agrega un detector de eventos al [insertar una función](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) en la capa de datos. Cuando se activa el evento `cmp:show`, se llama a la función `pageShownEventHandler`. En esta función, se agregan algunas comprobaciones y se construye un nuevo(a) `event` con el [estado más reciente de la capa de datos](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) del componente que activó el evento.

   Finalmente se llama a la función `trigger(event)`. La función `trigger()` es un nombre reservado en la propiedad de etiqueta y **déclencheur** la regla. El objeto `event` se pasa como parámetro, que a su vez se expone con otro nombre reservado en la propiedad de etiqueta. Los elementos de datos de la propiedad de etiqueta ahora pueden hacer referencia a varias propiedades mediante fragmentos de código como `event.component['someKey']`.

1. Guarde los cambios.
1. A continuación, en **Acciones**, haga clic en **Agregar** para abrir el asistente de **Configuración de la acción**.
1. Para el campo **Tipo de acción**, elija **Código personalizado**.

   ![Tipo de acción de código personalizado](assets/collect-data-analytics/action-custom-code.png)

1. Haga clic en **Abrir editor** en el panel principal e introduzca el siguiente fragmento de código:

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   El objeto `event` se ha pasado desde el método `trigger()` llamado en el evento personalizado. En este caso, `component` es la página actual derivada de la capa de datos `getState` en el evento personalizado.

1. Guarde los cambios y ejecute una [compilación](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html?lang=es) en la propiedad de etiquetas para promocionar el código al [entorno](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=es) utilizado en su sitio de AEM.

   >[!NOTE]
   >
   > Puede resultar útil usar [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=es) para cambiar el código incrustado a un entorno **Development**.

1. Vaya al sitio de AEM y abra las herramientas para desarrolladores para ver la consola. Actualice la página y debería ver que se han registrado los mensajes de la consola:

![Mensajes de consola cargados de página](assets/collect-data-analytics/page-show-event-console.png)

## Crear elementos de datos

A continuación, cree varios elementos de datos para capturar valores diferentes de la capa de datos del cliente de Adobe. Como se ha visto en el ejercicio anterior, es posible acceder a las propiedades de la capa de datos directamente a través del código personalizado. La ventaja de utilizar elementos de datos es que se pueden reutilizar en las reglas de etiquetas.

Los elementos de datos se asignan a las propiedades `@type`, `dc:title` y `xdm:template`.

### Tipo de medio de componente

1. Vaya a Experience Platform y a la propiedad de etiquetas integrada con el sitio de AEM.
1. Vaya a la sección **Elementos de datos** y haga clic en **Crear nuevo elemento de datos**.
1. Para el campo **Nombre**, escriba el **Tipo de recurso de componente**.
1. Para el campo **Tipo de elemento de datos**, seleccione **Código personalizado**.

   ![Tipo de recurso de componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Haga clic en el botón **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Guarde los cambios.

   >[!NOTE]
   >
   > Recuerde que el objeto `event` está disponible y con ámbito en función del evento que activó la **regla** en la propiedad de etiqueta. No se establece el valor de un elemento de datos hasta que se haga referencia al elemento de datos *1&rbrace; en una regla.* Por lo tanto, es seguro usar este elemento de datos dentro de una regla como la regla **Page Loaded** creada en el paso anterior *pero* no sería seguro usarla en otros contextos.

### Nombre de página

1. Haga clic en el botón **Agregar elemento de datos**
1. Para el campo **Nombre**, ingrese **Nombre de página**.
1. Para el campo **Tipo de elemento de datos**, seleccione **Código personalizado**.
1. Haga clic en el botón **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Guarde los cambios.

### Plantilla de la página

1. Haga clic en el botón **Agregar elemento de datos**
1. Para el campo **Nombre**, ingrese **Plantilla de página**.
1. Para el campo **Tipo de elemento de datos**, seleccione **Código personalizado**.
1. Haga clic en el botón **Abrir editor** e introduzca lo siguiente en el editor de código personalizado:

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

1. Vaya a Experience Platform y a la propiedad de etiquetas integrada con el sitio de AEM.
1. Ir a **Extensiones** > **Catálogo**
1. Busque la extensión **Adobe Analytics** y haga clic en **Instalar**

   ![Extensión de Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. En **Administración de biblioteca** > **Grupos de informes**, introduzca los identificadores de los grupos de informes que desee usar en cada entorno de etiquetas.

   ![Escriba los identificadores del grupo de informes](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Es aceptable utilizar un grupo de informes para todos los entornos en este tutorial, pero en la vida real le recomendamos utilizar grupos de informes separados, como se muestra en la siguiente imagen

   >[!TIP]
   >
   >Se recomienda usar la opción *Administrar la biblioteca por mí* como la configuración de Administración de bibliotecas, ya que facilita la tarea de mantener la biblioteca `AppMeasurement.js` actualizada.

1. Marque la casilla para habilitar **Usar Activity Map**.

   ![Habilitar el uso de Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. En **General** > **Servidor de seguimiento**, ingrese su servidor de seguimiento, por ejemplo, `tmd.sc.omtrdc.net`. Escriba su servidor de seguimiento SSL si su sitio admite `https://`

   ![Introduzca los servidores de seguimiento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Haga clic en **Guardar** para guardar los cambios.

## Añadir una condición a la regla Page Loaded

A continuación, actualice la regla **Página cargada** para usar el elemento de datos **Tipo de recurso de componente** para garantizar que la regla solo se active cuando el evento `cmp:show` sea para **Página**. Otros componentes pueden activar el evento `cmp:show`; por ejemplo, el componente Carrusel lo activa cuando cambian las diapositivas. Por lo tanto, es importante agregar una condición para esta regla.

1. En la interfaz de usuario de la propiedad Etiqueta, navegue hasta la regla **Página cargada** creada anteriormente.
1. En **Condiciones**, haga clic en **Agregar** para abrir el asistente de **Configuración de condición**.
1. Para el campo **Tipo de condición**, seleccione la opción **Comparación de valores**.
1. Establezca el primer valor del campo de formulario en `%Component Resource Type%`. Puede usar el icono de elemento de datos ![icono de elemento de datos](assets/collect-data-analytics/cylinder-icon.png) para seleccionar el elemento de datos **Tipo de recurso de componente**. Deje el comparador establecido en `Equals`.
1. Establezca el segundo valor en `wknd/components/page`.

   ![Configuración de condición para regla cargada en la página](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Es posible agregar esta condición dentro de la función de código personalizado que escucha el evento `cmp:show` creado anteriormente en el tutorial. Sin embargo, agregarla dentro de la interfaz de usuario da más visibilidad a los usuarios adicionales que pueden necesitar realizar cambios en la regla. Además, podemos utilizar nuestro elemento de datos.

1. Guarde los cambios.

## Establecer variables de Analytics y señalización de vista de página déclencheur

Actualmente, la regla **Página cargada** simplemente genera una instrucción de consola. A continuación, utilice los elementos de datos y la extensión de Analytics para establecer variables de Analytics como una **acción** en la regla **Página cargada**. También establecemos una acción adicional para almacenar en déclencheur la **señalización de vista de página** y enviar los datos recopilados a Adobe Analytics.

1. En la regla Page Loaded, **quite** la acción **Core - Custom Code** (las instrucciones de la consola):

   ![Quitar acción de código personalizado](assets/collect-data-analytics/remove-console-statements.png)

1. En la subsección Acciones, haga clic en **Agregar** para agregar una acción nueva.

1. Establezca el tipo de **Extension** en **Adobe Analytics** y establezca **Action Type** en **Set Variables**

   ![Establecer extensión de acción en variables de conjunto de Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. En el panel principal, seleccione un **eVar** disponible y establézcalo como el valor del elemento de datos **Plantilla de página**. Utilice el icono de elementos de datos ![icono de elementos de datos](assets/collect-data-analytics/cylinder-icon.png) para seleccionar el elemento **Plantilla de página**.

   ![Establecer como plantilla de página eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Desplácese hacia abajo, bajo **Configuración adicional**, establezca **Nombre de página** en el elemento de datos **Nombre de página**:

   ![Conjunto de variables de entorno con nombre de página](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Guarde los cambios.

1. A continuación, agregue una acción adicional a la derecha de **Adobe Analytics - Set Variables** tocando el icono **plus**:

   ![Agregar una acción adicional de regla de etiqueta](assets/collect-data-analytics/add-additional-launch-action.png)

1. Establezca el tipo de **Extension** en **Adobe Analytics** y establezca **Action Type** en **Send Beacon**. Dado que esta acción se considera una vista de página, deje el seguimiento predeterminado establecido en **`s.t()`**.

   ![Enviar acción de Adobe Analytics de señalización](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Guarde los cambios. La regla **Page Loaded** debe tener la configuración siguiente:

   ![Configuración final de la regla de etiquetas](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Escuche el evento `cmp:show`.
   * **2.** Compruebe que el evento se activó mediante una página.
   * **3.** estableció variables de Analytics para **Nombre de página** y **Plantilla de página**
   * **4.** Enviar la señalización de vista de página de Analytics

1. Guarde todos los cambios y cree su biblioteca de etiquetas, promocionando al entorno adecuado.

## Validar la señalización de vista de página y la llamada de Analytics.

Ahora que la regla **Page Loaded** envía la señalización de Analytics, debería poder ver las variables de seguimiento de Analytics mediante Experience Platform Debugger.

1. Abra el [sitio WKND](https://wknd.site/us/es.html) en su explorador.
1. Haga clic en el icono de Debugger ![Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) para abrir Experience Platform Debugger.
1. Asegúrese de que Debugger asigne la propiedad de etiqueta a *su entorno de desarrollo*, tal como se describió anteriormente, y de que **Registro de consola** esté activado.
1. Abra el menú Analytics y verifique que el grupo de informes esté establecido en *su* grupo de informes. También debe rellenarse el Nombre de página:

   ![Debugger de ficha de Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Desplácese hacia abajo y expanda **Solicitudes de red**. Debería poder encontrar el conjunto **evar** para la **plantilla de página**:

   ![Evar y conjunto de nombres de página](assets/collect-data-analytics/evar-page-name-set.png)

1. Vuelva al explorador y abra la consola de desarrollador. Haz clic en **Carrusel** en la parte superior de la página.

   ![Clic en la página de carrusel](assets/collect-data-analytics/click-carousel-page.png)

1. Observe en la consola del explorador la instrucción de consola:

   ![No se cumple la condición](assets/collect-data-analytics/condition-not-met.png)

   Esto se debe a que el carrusel no almacena en déclencheur un evento `cmp:show` *pero* debido a la comprobación del **Tipo de recurso de componente**, no se activa ningún evento.

   >[!NOTE]
   >
   > Si no ve ningún registro de consola, asegúrese de que **Registro de consola** esté marcado en **Etiquetas de Experience Platform** en Experience Platform Debugger.

1. Vaya a una página de artículo como [Australia occidental](https://wknd.site/us/en/magazine/western-australia.html). Observe que Nombre de página y Tipo de plantilla cambian.

## Enhorabuena.

Acaba de utilizar la capa de datos del cliente de Adobe impulsada por eventos y las etiquetas de Experience Platform para recopilar datos de página de un sitio de AEM y enviarlos a Adobe Analytics.

### Siguientes pasos

Consulte el siguiente tutorial para aprender a utilizar la capa de datos del cliente de Adobe impulsada por evento para [rastrear clics de componentes específicos en un sitio de Adobe Experience Manager](track-clicked-component.md).
