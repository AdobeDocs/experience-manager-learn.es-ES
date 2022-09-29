---
title: Uso de la capa de datos del cliente de Adobe con AEM componentes principales
description: La capa de datos del cliente de Adobe presenta un método estándar para recopilar y almacenar datos sobre una experiencia de visitante en una página web y, a continuación, facilitar el acceso a estos datos. La capa de datos del cliente de Adobe no depende de la plataforma, pero está completamente integrada en los componentes principales para su uso con AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 8%

---

# Uso de la capa de datos del cliente de Adobe con AEM componentes principales {#overview}

La capa de datos del cliente de Adobe presenta un método estándar para recopilar y almacenar datos sobre una experiencia de visitante en una página web y, a continuación, facilitar el acceso a estos datos. La capa de datos del cliente de Adobe no depende de la plataforma, pero está completamente integrada en los componentes principales para su uso con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> ¿Desea habilitar la capa de datos del cliente de Adobe en su sitio AEM? [Consulte las instrucciones aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Explorar la capa de datos

Puede hacerse una idea de la funcionalidad integrada de la capa de datos del cliente de Adobe con las herramientas para desarrolladores del navegador y de la lista de [Sitio de referencia WKND](https://wknd.site/).

>[!NOTE]
>
> Capturas de pantalla tomadas del navegador Chrome.

1. Vaya a [https://wknd.site](https://wknd.site)
1. Abra las herramientas para desarrolladores e introduzca el siguiente comando en la sección **Consola**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect proporciona la respuesta para ver el estado actual de la capa de datos en un sitio AEM. Debería ver información sobre la página y los componentes individuales.

   ![Respuesta de capa de datos de Adobe](assets/data-layer-state-response.png)

1. Inserte un objeto de datos en la capa de datos introduciendo lo siguiente en la consola:

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. Ejecutar el comando `adobeDataLayer.getState()` y busque la entrada para `training-data`.
1. A continuación, añada un parámetro de ruta para devolver solo el estado específico de un componente:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Devolver solo una entrada de datos de componente](assets/return-just-single-component.png)

## Uso de eventos

Se recomienda almacenar en déclencheur cualquier código personalizado basado en un evento de la capa de datos. A continuación, explore la posibilidad de registrar y escuchar diferentes eventos.

1. Introduzca el siguiente método de ayuda en la consola:

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   El código anterior inspeccionará la variable `event` y utilice la variable `adobeDataLayer.getState` para obtener el estado actual del objeto que activó el evento. El método de ayuda inspeccionará entonces la variable `filter` y solo si la variable `dataObject` cumple con el filtro que se devolverá.

   >[!CAUTION]
   >
   > Es importante **not** para actualizar el explorador a lo largo de este ejercicio, de lo contrario se pierde el JavaScript de la consola.

1. A continuación, introduzca un controlador de eventos al que se llama cuando se **Teaser** se muestra dentro de un **Carrusel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   La variable `teaserShownHandler` llamará a la función `getDataObjectHelper` y pasar un filtro de `wknd/components/teaser` como el `@type` para filtrar eventos activados por otros componentes.

1. A continuación, inserte un detector de eventos en la capa de datos para detectar la `cmp:show` evento.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   La variable `cmp:show` se activa mediante distintos componentes, como cuando se muestra una nueva diapositiva en la variable **Carrusel** o cuando se selecciona una pestaña nueva en la **Tabulación** componente.

1. En la página , cambie las diapositivas de carrusel y observe las instrucciones de la consola:

   ![Alternar Carrusel y ver el detector de eventos](assets/teaser-console-slides.png)

1. Elimine el detector de eventos de la capa de datos para dejar de escuchar el evento `cmp:show` evento:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Vuelva a la página y active las diapositivas de carrusel. Observe que no se registran más instrucciones y que el evento no se está escuchando.

1. A continuación, cree un controlador de eventos al que se llame cuando se active el evento que se muestra en la página:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Observe que el tipo de recurso `wknd/components/page` se utiliza para filtrar el evento.

1. A continuación, inserte un detector de eventos en la capa de datos para detectar la `cmp:show` , llamando a la función `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Debería ver inmediatamente una sentencia de consola activada con los datos de página:

   ![Datos de visualización de página](assets/page-show-console-data.png)

   La variable `cmp:show` para la página se activa cada vez que se carga la página en la parte superior de la página. Podría preguntar, ¿por qué se activó el controlador de eventos cuando la página ya se ha cargado?

   Esta es una de las características únicas de la capa de datos del cliente de Adobe, en la que puede registrar oyentes de eventos **before** o **after** se ha inicializado la capa de datos. Se trata de una característica esencial para evitar condiciones de carrera.

   La capa de datos mantiene una matriz de cola de todos los eventos que se han producido en secuencia. De forma predeterminada, la capa de datos déclencheur las llamadas de retorno de eventos para los eventos que se produjeron en la variable **pasado** así como los eventos en la variable **futuro**. Es posible filtrar los eventos al pasado o al futuro. [Encontrará más información en la documentación](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Siguientes pasos

Consulte el siguiente tutorial para aprender a utilizar la capa de datos del cliente de Adobe impulsada por eventos para [recopilar datos de página y enviarlos a Adobe Analytics](../analytics/collect-data-analytics.md).

O aprenda a [Personalización de la capa de datos del cliente de Adobe con componentes AEM](./data-layer-customize.md)


## Recursos adicionales {#additional-resources}

* [Documentación de capa de datos del cliente de Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Uso de la capa de datos del cliente de Adobe y la documentación de componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
