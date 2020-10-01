---
title: Carga y activación de una llamada de Destinatario
description: Obtenga información sobre cómo cargar, pasar parámetros a una solicitud de página y activar una llamada de Destinatario desde la página del sitio mediante una regla de inicio. La información de la página se recupera y pasa como parámetros mediante la capa de datos del cliente de Adobe, que permite recopilar y almacenar datos sobre la experiencia de los visitantes en una página web y, a continuación, facilitar el acceso a estos datos.
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 4%

---


# Carga y activación de una llamada de Destinatario {#load-fire-target}

Obtenga información sobre cómo cargar, pasar parámetros a una solicitud de página y activar una llamada de Destinatario desde la página del sitio mediante una regla de inicio. La información de la página se recupera y pasa como parámetros mediante la capa de datos del cliente de Adobe, que permite recopilar y almacenar datos sobre la experiencia de los visitantes en una página web y, a continuación, facilitar el acceso a estos datos.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regla de carga de página

La capa de datos del cliente de Adobe es una capa de datos controlada por eventos. Cuando se carga la capa de datos de la página de AEM, se desencadena un evento `cmp:show` . En el vídeo, la `Launch Library Loaded` regla se invoca mediante un evento personalizado. A continuación, puede encontrar los fragmentos de código utilizados en el vídeo para el evento personalizado, así como para los elementos de datos.

### Evento personalizado

El fragmento de código siguiente agregará un detector de eventos insertando una función en la capa de datos. Cuando se activa el `cmp:show` evento, se llama a la `pageShownEventHandler` función. En esta función, se añaden algunas comprobaciones de integridad y `dataObject` se crea una nueva con el estado más reciente de la capa de datos para el componente que activó el evento.

Después de eso `trigger(dataObject)` se llama. `trigger()` es un nombre reservado en Launch y &quot;activará&quot; la regla de inicio. Pasamos el objeto evento como un parámetro que, a su vez, será expuesto por otro nombre reservado en Launch llamado evento. Los elementos de datos de Launch ahora pueden hacer referencia a varias propiedades como, por ejemplo: `event.component['someKey']`.

```javascript
var pageShownEventHandler = function(evt) {
// defensive coding to avoid a null pointer exception
if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
   //trigger Launch Rule and pass event
   console.debug("cmp:show event: " + evt.eventInfo.path);
   var event = {
      //include the id of the component that triggered the event
      id: evt.eventInfo.path,
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

### ID de página de capa de datos

```
if(event && event.id) {
    return event.id;
}
```

![ID de página](assets/pageid.png)

### Ruta de acceso de página

```
if(event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

![Ruta de página](assets/pagepath.png)

### Título de página

```
if(event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

![Título de página](assets/pagetitle.png)

### Problemas comunes

#### ¿Por qué mis mboxes no se activan en mis páginas web?

**Mensaje de error cuando no se configuró la cookie mboxDisable**

![Error de dominio de cookie de destinatario](assets/target-cookie-error.png)

**Solución**

A veces, los clientes de destinatario utilizan instancias basadas en la nube con Destinatario para realizar pruebas o con fines sencillos de prueba del concepto. Estos dominios, y muchos otros, son parte de la Lista de Sufijo Público .
Los navegadores modernos no guardarán las cookies si utiliza estos dominios a menos que personalice la `cookieDomain` configuración mediante `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain 
};
```

## Vínculos de soporte

* [Documentación de la capa de datos del cliente de Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
* [Uso de la capa de datos del cliente de Adobe y de la documentación de componentes principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/data-layer/overview.html)
* [Introducción a Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)