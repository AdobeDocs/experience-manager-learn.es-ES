---
title: Carga y activación de una llamada de Target
description: Obtenga información sobre cómo cargar, pasar parámetros a una solicitud de página y activar una llamada de Target desde la página del sitio mediante una regla de Launch. La información de la página se recupera y pasa como parámetros mediante la capa de datos del cliente de Adobe, que permite recopilar y almacenar datos sobre la experiencia de los visitantes en una página web y, a continuación, facilitar el acceso a estos datos.
feature: Core Components, Adobe Client Data Layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 3%

---


# Carga y activación de una llamada de Target {#load-fire-target}

Obtenga información sobre cómo cargar, pasar parámetros a una solicitud de página y activar una llamada de Target desde la página del sitio mediante una regla de Launch. La información de la página web se recupera y pasa como parámetros mediante la capa de datos del cliente de Adobe, que permite recopilar y almacenar datos sobre la experiencia de los visitantes en una página web y, a continuación, facilitar el acceso a estos datos.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regla de carga de página

La capa de datos del cliente de Adobe es una capa de datos controlada por evento. Cuando se carga la capa de datos de la página de AEM, se activa un evento `cmp:show` . En el vídeo, la regla `Launch Library Loaded` se invoca mediante un evento personalizado. A continuación, puede encontrar los fragmentos de código utilizados en el vídeo para el evento personalizado así como para los elementos de datos.

### Evento de página personalizada mostrada{#page-event}

![Página mostrada: configuración de eventos y código personalizado](assets/load-and-fire-target-call.png)

En la propiedad de Launch, agregue un nuevo **Evento** a la **Regla**

+ __Extensión:__ Core
+ __Tipo de evento:__ código personalizado
+ __Nombre:__ Controlador de eventos de programa de la página (o algo descriptivo)

Pulse el botón __Abrir editor__ y pegue el siguiente fragmento de código. Este código __debe__ agregarse a la __Configuración del evento__ y a una __Acción__ posterior.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Una función personalizada define el `pageShownEventHandler` y escucha los eventos emitidos por los componentes principales de AEM, obtiene la información relevante del componente principal, la empaqueta en un objeto de evento y activa el evento de Launch con la información de evento derivada en su carga útil.

La regla de Launch se activa utilizando la función `trigger(...)` de Launch, que __solo__ está disponible dentro de la definición de fragmento de código personalizado de un evento de regla.

La función `trigger(...)` toma un objeto de evento como parámetro que, a su vez, se expone en los elementos de datos de Launch, con otro nombre reservado en Launch denominado `event`. Los elementos de datos de Launch ahora pueden hacer referencia a los datos de este objeto de evento desde el objeto `event` mediante sintaxis como `event.component['someKey']`.

Si `trigger(...)` se utiliza fuera del contexto del tipo de evento de Custom Code de un Evento (por ejemplo, en una Acción), el error `trigger is undefined` de JavaScript se genera en el sitio web integrado con la propiedad Launch.


### Elementos de datos

![Elementos de datos](assets/data-elements.png)

Los elementos de datos de Adobe Launch asignan los datos del objeto de evento [activado en el evento personalizado Página mostrada](#page-event) a las variables disponibles en Adobe Target, a través del Tipo de elemento de datos de código personalizado de la extensión principal.

#### Elemento de datos ID de página

```
if (event && event.id) {
    return event.id;
}
```

Este código devuelve el ID exclusivo generado por el componente principal.

![ID de página](assets/pageid.png)

### Elemento de datos de ruta de página

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Este código devuelve la ruta de la página de AEM.

![Ruta de página](assets/pagepath.png)

### Elemento de datos Título de página

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Este código devuelve el título de la página de AEM.

![Título de página](assets/pagetitle.png)

## Solución de problemas

### ¿Por qué no se activan los mboxes en mis páginas web?

#### Mensaje de error cuando no se ha establecido la cookie mboxDisable

![Error de dominio de cookie de Target](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### Solución

Los clientes de Target a veces utilizan instancias basadas en la nube con Target para realizar pruebas o simplemente exponer conceptos. Estos dominios, y muchos otros, forman parte de la Lista pública de sufijos .
Los navegadores modernos no guardarán las cookies si utiliza estos dominios, a menos que personalice la configuración `cookieDomain` mediante `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Pasos siguientes

+ [Exportar fragmento de experiencia a Adobe Target](./export-experience-fragment-target.md)

## Compatibilidad con vínculos

+ [Documentación de la capa de datos del cliente de Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger: Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Uso de la capa de datos del cliente de Adobe y la documentación de componentes principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Introducción a Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)