---
title: AEM Acciones de Adobe I/O Runtime de la extensión de la consola de fragmentos de contenido
description: Obtenga información sobre cómo crear un modal de extensión de la consola de fragmentos de contenido AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 1%

---


# Acción de Adobe I/O Runtime

![Acciones de tiempo de ejecución de la extensión de fragmento de contenido de AEM](./assets/runtime-action/action-runtime-flow.png){align="center"}

AEM las extensiones de fragmento de contenido pueden incluir, opcionalmente, cualquier número de [Acciones de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).

Las acciones de Adobe I/O Runtime son funciones sin servidor que la extensión puede invocar. Las acciones son útiles para realizar trabajos que requieren interacción con AEM u otros servicios web de Adobe.
Las acciones suelen ser más útiles para realizar tareas de larga duración (que duran más de unos segundos) o para realizar solicitudes HTTP a AEM u otros servicios web.

Los beneficios de utilizar acciones de Adobe I/O Runtime para realizar trabajos son:

+ Las acciones son funciones sin servidor que se ejecutan fuera del contexto de un explorador, lo que elimina la necesidad de preocuparse por el CORS
+ El usuario no puede interrumpir las acciones (por ejemplo, al actualizar el explorador)
+ Las acciones son asincrónicas, por lo que pueden ejecutarse todo el tiempo necesario sin bloquear al usuario

En el contexto de AEM extensiones de fragmento de contenido, las acciones se utilizan generalmente para comunicarse con AEM directamente, a menudo:

+ Recopilación de datos relacionados de AEM sobre los fragmentos de contenido
+ Realización de operaciones personalizadas en fragmentos de contenido
+ Creación personalizada de fragmentos de contenido

Aunque la extensión de fragmento de contenido de AEM aparece en la consola de fragmento de contenido, las extensiones y sus acciones de apoyo, pueden invocar cualquier API HTTP disponible AEM, incluidos los extremos de API de AEM personalizados.

## Invocar una acción

Las acciones de Adobe I/O Runtime se invocan principalmente desde dos lugares en una extensión de fragmento de contenido AEM:

1. La variable [registro de extensión](./extension-registration.md) `onClick(..)` handler
1. Dentro de un [modal](./modal.md)

### Desde registro de extensión

Las acciones de Adobe I/O Runtime se pueden llamar directamente desde el código de registro de la extensión. El caso de uso más común es enlazar una acción a un [menú encabezado](./header-menu.md#no-modal)del botón que no utiliza [modales](./modal.md).

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
// allActions is an object containing all the actions defined in the extension's manifest
import allActions from '../config.json'

// actionWebInvoke is a helper that invokes an action
import actionWebInvoke from '../utils'
...
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your header menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension',  // Unique ID for the button
              'label': 'My header menu extension',       // Button label 
              'icon': 'Edit'                             // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          onClick() {
            // Set the HTTP headers required to access the Adobe I/O runtime action
            const headers = {
              'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
              'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
            };

            // Set the parameters to pass to the Adobe I/O Runtime action
            const params = {
              aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`, // Pass in the AEM host if the action interacts with AEM
              aemAccessToken: guestConnection.sharedContext.get('auth').imsToken
            };

            try {
              // Invoke Adobe I/O Runtime action named `generic`, with the configured headers and parameters.
              const actionResponse = await actionWebInvoke(allActions['generic'], headers, params);
            } catch (e) {
              // Log and store any errors
              console.error(e)
            }           
          }
        }
      }
    })
  }
  init().catch(console.error);
}
```

### Desde modo

Las acciones de Adobe I/O Runtime se pueden llamar directamente desde los modales para realizar un trabajo más involucrado, especialmente un trabajo que dependa de la comunicación con AEM as a Cloud Service, el servicio web de Adobe o incluso servicios de terceros.

Las acciones de Adobe I/O Runtime son aplicaciones JavaScript basadas en Node.js que se ejecutan en el entorno Adobe I/O Runtime sin servidor. Estas acciones se pueden dirigir mediante HTTP desde la SPA de extensión.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()
  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (!actionResponse) {
    // Else if the modal is ready to render and has not called the Adobe I/O Runtime action yet
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    

                     <ButtonGroup align="end">
                        <Button variant="cta" onPress={doWork}>Do work</Button>
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  } else {
    // Else the modal has called the Adobe I/O Runtime action and is ready to render the response
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        Done! The response from the action is: { actionResponse }
                    </Text>                    

                     <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }

  /**
   * Invoke the Adobe I/O Runtime action and store the response in the React component's state.
   */
  async function doWork() {
    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,
      contentFragmentPaths: contentFragmentPaths
    };

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      // Invoke the Adobe I/O Runtime action named `generic`.
      const actionResponse = await actionWebInvoke(allActions['generic'], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```

## Acción de Adobe I/O Runtime

+ `src/aem-cf-console-admin-1/actions/generic/index.js`

```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

async function main (params) {
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    logger.debug(stringParameters(params))

    // Check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'contentFragmentPaths' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    // Example HTTP request to AEM payload; This updates all 'title' properties of the Content Fragments to 'Hello World'
    const body = {
      "properties": {
        "elements": {
          "title": {
            "value": "Hello World"
          }
        }
      }
    };

    let results = await Promise.all(params.contentFragmentPaths.map(async (contentFragmentPath) => {
      // Invoke the AEM HTTP Assets Content Fragment API to update each Content Fragment
      // The AEM host is passed in as a parameter to the Adobe I/O Runtime action
      const res = await fetch(`${params.aemHost}${contentFragmentPath.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          // Pass in the accessToken as AEM Author service requires authentication/authorization
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated title of ${contentFragmentPath}`);
        return { contentFragmentPath, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update title of ${contentFragmentPath}`);
        return { contentFragmentPath, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    // Return a response to the AEM Content Fragment extension React application
    const response = {
      statusCode: 200,
      body: results
    };
    return response;

  } catch (error) {
    logger.error(error)
    return errorResponse(500, 'server error', logger)
  }
}
```

## API AEM HTTP

Las siguientes API AEM HTTP se utilizan habitualmente para interactuar con AEM de extensiones:

+ [API de AEM GraphQL](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html)
+ [API HTTP de AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html)
   + [Compatibilidad con fragmentos de contenido en la API HTTP de Recursos AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/assets-api-content-fragments.html)
+ [API de AEM QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api.html)
+ [Referencia completa AEM API as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/reference-materials.html)


## Módulos npm de Adobe

Los siguientes son módulos npm útiles para desarrollar acciones de Adobe I/O Runtime:

+ [@adobe/aio-sdk](https://www.npmjs.com/package/@adobe/aio-sdk)
   + [SDK principal](https://github.com/adobe/aio-sdk-core)
   + [Biblioteca de estados](https://github.com/adobe/aio-lib-state)
   + [Biblioteca de archivos](https://github.com/adobe/aio-lib-files)
   + [Biblioteca de Adobe Target](https://github.com/adobe/aio-lib-target)
   + [Biblioteca de Adobe Analytics](https://github.com/adobe/aio-lib-analytics)
   + [Biblioteca de Adobe Campaign Standard](https://github.com/adobe/aio-lib-campaign-standard)
   + [Biblioteca de perfiles del cliente de Adobe](https://github.com/adobe/aio-lib-customer-profile)
   + [Biblioteca de datos de clientes de Adobe Audience Manager](https://github.com/adobe/aio-lib-audience-manager-cd)
   + [Eventos de Adobe I/O](https://github.com/adobe/aio-lib-events)
+ [@adobe/aio-lib-core-network](https://github.com/adobe/aio-lib-core-networking)
+ [@adobe/node-httptransfer](https://github.com/adobe/node-httptransfer)