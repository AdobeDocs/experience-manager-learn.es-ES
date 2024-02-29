---
title: Generar token de acceso JWT en la acción del Generador de aplicaciones
description: Obtenga información sobre cómo generar un token de acceso mediante credenciales de JWT para su uso en una acción del Generador de aplicaciones.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 161
source-git-commit: c77dd9c2872e7e43863d83837cedbff50a7d3c1a
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 1%

---

# Generar token de acceso JWT en la acción del Generador de aplicaciones

Es posible que las acciones del Generador de aplicaciones tengan que interactuar con las API de Adobe asociadas con los proyectos de la consola de Adobe Developer en los que está implementada la aplicación del Generador de aplicaciones.

Esto puede requerir que la acción del Generador de aplicaciones genere su propio token de acceso JWT asociado con el proyecto de consola de Adobe Developer deseado.

>[!IMPORTANT]
>
> Revisar [Documentación de seguridad del App Builder](https://developer.adobe.com/app-builder/docs/guides/security/) para saber cuándo es adecuado generar tokens de acceso en lugar de utilizar los tokens de acceso proporcionados.
>
> Es posible que la acción personalizada tenga que proporcionar sus propias comprobaciones de seguridad para garantizar que solo los consumidores permitidos puedan acceder a la acción del Generador de aplicaciones y a los servicios de Adobe subyacentes.


## archivo .env

En el del proyecto del Creador de aplicaciones `.env` , añada claves personalizadas para cada una de las credenciales JWT del proyecto de la consola Adobe Developer. Los valores de credenciales de JWT se pueden obtener del proyecto de consola de Adobe Developer __Credenciales__ > __Cuenta de servicio (JWT)__ para un espacio de trabajo determinado.

![Credenciales del servicio JWT de la consola Adobe Developer](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

Los valores de `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` se puede copiar directamente desde la pantalla Credenciales de JWT del proyecto de la consola de Adobe Developer.

### Metascopios

Determine las API de Adobe y sus metaámbitos con los que interactúa la acción del Generador de aplicaciones. Enumerar metascopios con delimitadores de coma en `JWT_METASCOPES` clave. Los metascopios válidos se enumeran en [Documentación del Metascope JWT de Adobe](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


Por ejemplo, el siguiente valor podría agregarse al `JWT_METASCOPES` clave en el `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Clave privada

El `JWT_PRIVATE_KEY` debe tener un formato especial, ya que es un valor multilínea de forma nativa, lo que no es compatible con `.env` archivos. La forma más sencilla es codificar en base64 la clave privada. Base64 codifica la clave privada (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) se puede realizar utilizando las herramientas nativas proporcionadas por el sistema operativo.

>[!BEGINTABS]

>[!TAB macOS]

1. Abrir `Terminal`
1. Ejecute el comando `base64 -i /path/to/private.key | pbcopy`
1. La salida de base64 se copia automáticamente en el portapapeles
1. Pegar en `.env` como valor de la clave correspondiente

>[!TAB Windows]

1. Abrir `Command Prompt`
1. Ejecute el comando `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. Ejecute el comando `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. Copiar la salida de base64 en el portapapeles
1. Pegar en `.env` como valor de la clave correspondiente

>[!TAB Linux®]

1. Abrir terminal
1. Ejecute el comando `base64 private.key`
1. Copiar la salida de base64 en el portapapeles
1. Pegar en `.env` como valor de la clave correspondiente

>[!ENDTABS]

Por ejemplo, se puede agregar la siguiente clave privada codificada en base64 al `JWT_PRIVATE_KEY` clave en el `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Asignación de entradas

Con el valor de credencial JWT establecido en `.env` , deben asignarse a las entradas de acción de AppBuilder para que se puedan leer en la propia acción. Para ello, añada entradas para cada variable en la variable `ext.config.yaml` acción `inputs` con el formato: `PARAMS_INPUT_NAME: $ENV_KEY`.

Por ejemplo:

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

Las claves definidas en `inputs` están disponibles en la `params` objeto proporcionado a la acción del Generador de aplicaciones.


## Credenciales JWT para el token de acceso

En la acción del Generador de aplicaciones, las credenciales de JWT están disponibles en el `params` objeto, y se puede utilizar por [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) para generar un token de acceso, que a su vez puede acceder a otras API y servicios de Adobe.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
