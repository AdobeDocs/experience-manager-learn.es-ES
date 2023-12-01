---
title: Uso de la capa de datos del cliente de Adobe AEM con los componentes principales de
description: La capa de datos del cliente de Adobe presenta un método estándar para recopilar y almacenar datos sobre la experiencia de un visitante en una página web y, a continuación, facilitar el acceso a estos datos. La capa de datos del cliente de Adobe no depende de la plataforma, pero está completamente integrada en los componentes principales para su uso con AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
jira: KT-6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
doc-type: Tutorial
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 9%

---

# Uso de la capa de datos del cliente de Adobe AEM con los componentes principales de {#overview}

La capa de datos del cliente de Adobe presenta un método estándar para recopilar y almacenar datos sobre la experiencia de un visitante en una página web y, a continuación, facilitar el acceso a estos datos. La capa de datos del cliente de Adobe no depende de la plataforma, pero está completamente integrada en los componentes principales para su uso con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> ¿Desea habilitar la capa de datos del cliente de Adobe AEM en el sitio de la? [Consulte las instrucciones aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Exploración de la capa de datos

Puede hacerse una idea de la funcionalidad integrada de la capa de datos del cliente de Adobe con solo utilizar las herramientas para desarrolladores de su explorador y el [Sitio de referencia de WKND](https://wknd.site/us/es.html).

>[!NOTE]
>
> Capturas de pantalla a continuación tomadas desde el navegador Chrome.

1. Vaya a [https://wknd.site/us/en.html](https://wknd.site/us/es.html)
1. Abra las herramientas para desarrolladores e introduzca el siguiente comando en la **Consola**:

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM Para ver el estado actual de la capa de datos en un sitio de datos, compruebe la respuesta de un sitio de datos en el que se haya realizado una inspección. Debería ver información sobre la página y los componentes individuales.

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

1. Ejecute el comando `adobeDataLayer.getState()` de nuevo y busque la entrada para `training-data`.
1. A continuación, añada un parámetro de ruta para devolver solo el estado específico de un componente:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Devolver solo una entrada de datos de componente](assets/return-just-single-component.png)

## Uso de eventos

Se recomienda almacenar en déclencheur cualquier código personalizado basado en un evento de la capa de datos. A continuación, explore cómo registrar y escuchar diferentes eventos.

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

   El código anterior inspecciona el `event` y utiliza el `adobeDataLayer.getState` para obtener el estado actual del objeto que activó el evento. A continuación, el método de ayuda inspecciona el `filter` y solo si el actual `dataObject` cumple los criterios de filtro que se devuelven.

   >[!CAUTION]
   >
   > Es importante **no** para actualizar el explorador durante este ejercicio; de lo contrario, se pierde el JavaScript de la consola.

1. A continuación, introduzca un controlador de eventos al que se llame cuando **Teaser** el componente se muestra dentro de un **Carrusel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   El `teaserShownHandler` llama a la función `getDataObjectHelper` y pasa un filtro de `wknd/components/teaser` como el `@type` para filtrar eventos activados por otros componentes.

1. A continuación, inserte un detector de eventos en la capa de datos para detectar el `cmp:show` evento.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   El `cmp:show` se activa por muchos componentes diferentes, como cuando se muestra una nueva diapositiva en la **Carrusel** o cuando se selecciona una nueva pestaña en la **Ficha** componente.

1. En la página, alterne las diapositivas del carrusel y observe las instrucciones de la consola:

   ![Alternar carrusel y ver el detector de eventos](assets/teaser-console-slides.png)

1. Para dejar de escuchar el `cmp:show` evento, elimine el detector de eventos de la capa de datos

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Vuelva a la página y cambie las diapositivas del carrusel. Observe que no se registran más instrucciones y que el evento no se escucha.

1. A continuación, cree un controlador de eventos al que se llame cuando se active el evento Página mostrada:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Observe que el tipo de recurso `wknd/components/page` para filtrar el evento.

1. A continuación, inserte un detector de eventos en la capa de datos para detectar el `cmp:show` evento, llamar a `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Debería ver inmediatamente una instrucción de consola activada con los datos de la página:

   ![Página para mostrar datos](assets/page-show-console-data.png)

   El `cmp:show` el evento de la página se activa cada vez que se carga la página en la parte superior de la misma. Podría preguntar, ¿por qué se activó el controlador de eventos cuando claramente la página ya se ha cargado?

   Una de las características únicas de la capa de datos del cliente de Adobe es que puede registrar detectores de eventos **antes** o **después** Una vez inicializada la capa de datos, ayuda a evitar las condiciones de carrera.

   La capa de datos mantiene una matriz de colas de todos los eventos que se han producido en secuencia. La capa de datos almacenará en déclencheur de forma predeterminada las llamadas de retorno de eventos para los eventos que se produjeron en la **pasado** y eventos en la **futuro**. Es posible filtrar los eventos del pasado o del futuro. [Encontrará más información en la documentación](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Pasos siguientes

Hay dos opciones para seguir aprendiendo: la primera es consultar la [recopilar datos de página y enviarlos a Adobe Analytics](../analytics/collect-data-analytics.md) tutorial que muestra el uso de la capa de datos del cliente de Adobe. La segunda opción es, para aprender a [Personalización de la capa de datos del cliente de Adobe AEM con componentes de](./data-layer-customize.md)


## Recursos adicionales {#additional-resources}

* [Documentación de capa de datos del cliente de Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Uso de la capa de datos del cliente de Adobe y la documentación de componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es)
