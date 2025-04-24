---
title: Generar token de acceso JWT en la acción de App Builder
description: Obtenga información sobre cómo generar un token de acceso utilizando las credenciales de JWT para su uso en una acción de App Builder.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 151
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 1%

---

# Generar token de acceso JWT en la acción de App Builder

Es posible que las acciones de App Builder deban interactuar con las API de Adobe asociadas con los proyectos de Adobe Developer Console en los que está implementada la aplicación de App Builder.

Esto puede requerir que la acción de App Builder genere su propio token de acceso JWT asociado con el proyecto de Adobe Developer Console deseado.

>[!IMPORTANT]
>
> Revise la [documentación de seguridad de App Builder](https://developer.adobe.com/app-builder/docs/guides/security/) para saber cuándo es apropiado generar tokens de acceso en comparación con los tokens de acceso proporcionados.
>
> Es posible que la acción personalizada tenga que proporcionar sus propias comprobaciones de seguridad para garantizar que solo los consumidores permitidos puedan acceder a la acción de App Builder y a los servicios de Adobe subyacentes.


## archivo .env

En el archivo `.env` del proyecto de App Builder, anexe claves personalizadas para cada una de las credenciales JWT del proyecto de Adobe Developer Console. Los valores de credenciales de JWT se pueden obtener de __Credentials__ > __Service Account (JWT)__ del proyecto de Adobe Developer Console para un área de trabajo determinada.

![Credenciales del servicio JWT de Adobe Developer Console](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

Los valores de `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` se pueden copiar directamente desde la pantalla Credenciales JWT del proyecto Adobe Developer Console.

### Metascopios

Determine las API de Adobe y sus metascopios con los que interactúa la acción de App Builder. Enumerar metascopios con delimitadores de coma en la clave `JWT_METASCOPES`. Los metascopios válidos se enumeran en [Documentación del Metascopio JWT de Adobe](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/scopes).


Por ejemplo, el siguiente valor podría agregarse a la clave `JWT_METASCOPES` en `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Clave privada

`JWT_PRIVATE_KEY` debe tener un formato especial, ya que es un valor multilínea de forma nativa, algo que no se admite en `.env` archivos. La forma más sencilla es codificar en base64 la clave privada. La codificación en base64 de la clave privada (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) se puede realizar utilizando las herramientas nativas proporcionadas por el sistema operativo.

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

Por ejemplo, la siguiente clave privada codificada en base64 se puede agregar a la clave `JWT_PRIVATE_KEY` en `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Asignación de entradas

Con el valor de credencial JWT establecido en el archivo `.env`, deben asignarse a las entradas de acción de AppBuilder para que se puedan leer en la propia acción. Para ello, agregue entradas para cada variable en la acción `ext.config.yaml` `inputs` con el formato: `PARAMS_INPUT_NAME: $ENV_KEY`.

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

Las claves definidas en `inputs` están disponibles en el objeto `params` proporcionado a la acción de App Builder.


## Credenciales JWT para el token de acceso

En la acción App Builder, las credenciales JWT están disponibles en el objeto `params` y [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) las puede usar para generar un token de acceso, que a su vez puede acceder a otras API y servicios de Adobe.

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
