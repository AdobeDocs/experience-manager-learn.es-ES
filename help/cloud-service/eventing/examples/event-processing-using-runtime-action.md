---
title: AEM Procesamiento de eventos de mediante la acción de Adobe I/O Runtime
description: AEM Obtenga información sobre cómo procesar eventos de recibidos mediante la acción de Adobe I/O Runtime.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
source-git-commit: f0930e517254b6353fe50c3bbf9ae915d9ef6ca3
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---


# AEM Procesamiento de eventos de mediante la acción de Adobe I/O Runtime

AEM Obtenga información sobre cómo procesar eventos de recibidos mediante [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Acción. Este ejemplo mejora el ejemplo anterior [Eventos de acción y de de Adobe I/O Runtime AEM](runtime-action.md), asegúrese de que lo ha completado antes de continuar con este.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

En este ejemplo, el procesamiento de eventos almacena los datos de evento originales y el evento recibido como un mensaje de actividad en el almacenamiento de Adobe I/O Runtime. Sin embargo, si el evento es de _Fragmento de contenido modificado_ AEM escriba y, a continuación, también llama al servicio de autor de para encontrar los detalles de la modificación. SPA Finalmente, muestra los detalles del evento en una aplicación de una sola página ().

## Requisitos previos

Para completar este tutorial, necesita lo siguiente:

- AEM entorno as a Cloud Service con [AEM Eventing activado de](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Además, la muestra [Sitios WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) el proyecto debe implementarse en él.

- Acceso a [Consola de Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [CLI de Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) instalado en el equipo local.

- Proyecto inicializado localmente desde el ejemplo anterior [Eventos de acción y de de Adobe I/O Runtime AEM](./runtime-action.md#initialize-project-for-local-development).

>[!IMPORTANT]
>
>AEM El evento as a Cloud Service solo está disponible para usuarios registrados en el modo previo al lanzamiento. AEM AEM Para habilitar la celebración de eventos en su entorno as a Cloud Service de la, póngase en contacto con [AEM Equipo de eventos de la](mailto:grp-aem-events@adobe.com).

## AEM Acción del procesador de eventos

En este ejemplo, el procesador de eventos [acción](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) realiza las siguientes tareas:

- Analiza el evento recibido en un mensaje de actividad.
- Si el evento recibido es de _Fragmento de contenido modificado_ AEM Escriba, vuelva a llamar al servicio de autor de la para encontrar los detalles de la modificación.
- Conserva los datos de evento, el mensaje de actividad y los detalles de modificación originales (si los hay) en Adobe I/O Runtime Storage.

Para realizar las tareas anteriores, empecemos agregando una acción al proyecto, desarrollemos módulos JavaScript para realizar las tareas anteriores y, finalmente, actualicemos el código de acción para utilizar los módulos desarrollados.

Consulte el adjunto [AEM WKND--Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) para obtener el código completo, y en la sección siguiente se destacan los archivos clave.

### Añadir acción

- Para agregar una acción, ejecute el siguiente comando:

  ```bash
  aio app add action
  ```

- Seleccionar `@adobe/generator-add-action-generic` como plantilla de acción, asigne un nombre a la acción `aem-event-processor`.

  ![Añadir acción](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### Desarrollar módulos de JavaScript

Para realizar las tareas mencionadas anteriormente, vamos a desarrollar los siguientes módulos JavaScript.

- El `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` determina si el evento recibido es de _Fragmento de contenido modificado_ escriba.

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- El `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` AEM el módulo llama al servicio de autor de la para encontrar los detalles de la modificación.

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  Consulte [AEM Tutorial de credenciales de servicio](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en) para obtener más información. Además, la variable [Archivos de configuración de App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) para administrar secretos y parámetros de acción.

- El `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` El módulo almacena los datos de evento, el mensaje de actividad y los detalles de modificación originales (si los hay) en el almacenamiento de Adobe I/O Runtime.

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### Actualizar código de acción

Finalmente, actualice el código de acción en `src/dx-excshell-1/actions/aem-event-processor/index.js` para utilizar los módulos desarrollados.

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## Recursos adicionales

- El `src/dx-excshell-1/actions/model` La carpeta contiene `aemEvent.js` y `errors.js` archivos, que utiliza la acción para analizar el evento recibido y controlar los errores respectivamente.
- El `src/dx-excshell-1/actions/load-processed-aem-events` SPA AEM La carpeta contiene el código de acción. El usuario utiliza esta acción para cargar los eventos de procesados desde el almacenamiento de Adobe I/O Runtime.
- El `src/dx-excshell-1/web-src` SPA AEM La carpeta contiene el código de la, que muestra los eventos de la procesados.
- El `src/dx-excshell-1/ext.config.yaml` contiene parámetros y configuración de acción.

## Concepto y elementos clave

Los requisitos de procesamiento de eventos difieren de un proyecto a otro, pero las principales conclusiones de este ejemplo son:

- El procesamiento de eventos se puede realizar mediante la acción de Adobe I/O Runtime.
- La acción de tiempo de ejecución puede comunicarse con sistemas como las aplicaciones internas, las soluciones de terceros y las soluciones de Adobe.
- La acción de tiempo de ejecución sirve como punto de entrada a un proceso empresarial diseñado en torno a un cambio de contenido.




