---
title: Carga y activación de una llamada de Target
description: Obtenga información sobre cómo cargar, pasar parámetros a una solicitud de página y activar una llamada de Target desde la página del sitio mediante una regla de etiquetas.
feature: Core Components, Adobe Client Data Layer
version: Experience Manager as a Cloud Service
jira: KT-6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
duration: 588
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 1%

---

# Carga y activación de una llamada de Target {#load-fire-target}

Obtenga información sobre cómo cargar, pasar parámetros a una solicitud de página y activar una llamada de Target desde la página del sitio mediante una regla de etiquetas. La información de la página web se recupera y pasa como parámetros mediante la capa de datos del cliente de Adobe, que permite recopilar y almacenar datos sobre la experiencia de los visitantes en una página web y, a continuación, facilitar el acceso a estos datos.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regla de carga de página

La capa de datos del cliente de Adobe es una capa de datos impulsada por evento. Cuando se carga la capa de datos de la página de AEM, se déclencheur un evento `cmp:show` En el vídeo, la regla `tags Library Loaded` se invoca mediante un evento personalizado. A continuación, se pueden encontrar los fragmentos de código utilizados en el vídeo para el evento personalizado y para los elementos de datos.

### Evento personalizado de página mostrada{#page-event}

![Página mostrada con configuración de evento y código personalizado](assets/load-and-fire-target-call.png)

En la propiedad de etiquetas, agregue un nuevo **Evento** a la **Regla**

+ __Extensión:__ Core
+ __Tipo de evento:__ Código personalizado
+ __Nombre:__ controlador de eventos de muestra de página (o algo descriptivo)

Pulse el botón __Abrir editor__ y pegue el siguiente fragmento de código. Este código __debe__ agregarse a la __Configuración de evento__ y a una __Acción__ posterior.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the tags Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        // Trigger the tags Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other tags data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Data Collection, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Una función personalizada define `pageShownEventHandler` y escucha los eventos emitidos por los componentes principales de AEM, deriva la información relevante del componente principal, lo empaqueta en un objeto de evento y almacena en déclencheur el evento de etiquetas con la información de evento derivada en su carga útil.

La regla de etiquetas se activa usando la función `trigger(...)` de las etiquetas, que es __solo__ disponible dentro de la definición de fragmento de código personalizado de un evento de regla.

La función `trigger(...)` toma un objeto de evento como parámetro que, a su vez, se expone en elementos de datos de etiquetas con otro nombre reservado en etiquetas denominadas `event`. Los elementos de datos de las etiquetas ahora pueden hacer referencia a los datos de este objeto de evento del objeto `event` mediante sintaxis como `event.component['someKey']`.

Si se usa `trigger(...)` fuera del contexto del tipo de evento Custom Code de un evento (por ejemplo, en una acción), se generará el error de JavaScript `trigger is undefined` en el sitio web integrado con la propiedad tags.


### Elementos de datos

![Elementos de datos](assets/data-elements.png)

Los elementos de datos de etiquetas asignan los datos del objeto de evento [activado en el evento personalizado Página mostrada](#page-event) a las variables disponibles en Adobe Target, a través del tipo de elemento de datos de código personalizado de la extensión principal.

#### Elemento de datos de ID de página

```
if (event && event.id) {
    return event.id;
}
```

Este código devuelve el ID único generado del componente principal.

![Id. de página](assets/pageid.png)

### Elemento de datos de ruta de página

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Este código devuelve la ruta de la página AEM.

![Ruta de la página](assets/pagepath.png)

### Elemento de datos Page Title

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Este código devuelve el título de la página de AEM.

![Título de página](assets/pagetitle.png)

## Solución de problemas

### ¿Por qué no se activan los mboxes en mis páginas web?

#### Mensaje de error cuando no se establece la cookie mboxDisable

![Error de dominio de cookie de Target](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### Solución

Los clientes de utilizan en ocasiones instancias basadas en la nube con Target para realizar pruebas o simplemente exponer conceptos. Estos dominios, y muchos otros, son parte de la Lista pública de sufijos .
Los exploradores modernos no guardarán las cookies si utiliza estos dominios a menos que personalice la configuración de `cookieDomain` mediante `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Siguientes pasos

+ [Exportar fragmento de experiencia a Adobe Target](./export-experience-fragment-target.md)

## Vínculos de soporte

+ [Documentación de la capa de datos del cliente Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Uso de la capa de datos del cliente de Adobe y la documentación de componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es)
+ [Introducción a Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=es)
