---
title: Generar token de acceso de servidor a servidor en la acción de App Builder
description: Obtenga información sobre cómo generar un token de acceso mediante las credenciales de servidor a servidor de OAuth para su uso en una acción de App Builder.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: 122
exl-id: 919cb9de-68f8-4380-940a-17274183298f
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Generar token de acceso de servidor a servidor en la acción de App Builder

Es posible que las acciones de App Builder deban interactuar con las API de Adobe que admiten **OAuth Server-to-Server credentials** y que están asociadas con proyectos de Adobe Developer Console en los que está implementada la aplicación de App Builder.

En esta guía se explica cómo generar un token de acceso utilizando _OAuth Server-to-Server credentials_ para su uso en una acción de App Builder.

>[!IMPORTANT]
>
> Las credenciales de la cuenta de servicio (JWT) han quedado obsoletas y pasan a ser credenciales de servidor a servidor OAuth. Sin embargo, todavía hay algunas API de Adobe que solo admiten credenciales de cuenta de servicio (JWT) y la migración de OAuth de servidor a servidor está en curso. Consulte la documentación de la API de Adobe para comprender qué credenciales se admiten.

## Configuraciones del proyecto de Adobe Developer Console

Al agregar la API de Adobe deseada al proyecto de Adobe Developer Console, en el paso _Configurar API_, seleccione el tipo de autenticación **Servidor a servidor OAuth**.

![Adobe Developer Console - Servidor a servidor OAuth](./assets/s2s-auth/oauth-server-to-server.png)

Para asignar la cuenta de servicio de integración creada automáticamente arriba, seleccione el perfil de producto deseado. Por lo tanto, a través del perfil de producto, los permisos de la cuenta de servicio están controlados.

![Adobe Developer Console - Perfil de producto](./assets/s2s-auth/select-product-profile.png)

## archivo .env

En el archivo `.env` del proyecto de App Builder, anexe claves personalizadas para las credenciales de servidor a servidor OAuth del proyecto de Adobe Developer Console. Los valores de credenciales de servidor a servidor de OAuth se pueden obtener de las __credenciales__ > __Servidor a servidor de OAuth__ del proyecto de Adobe Developer Console para un área de trabajo determinada.

![Credenciales de servidor a servidor OAuth de Adobe Developer Console](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

Los valores de `OAUTHS2S_CLIENT_ID`, `OAUTHS2S_CLIENT_SECRET`, `OAUTHS2S_CECREDENTIALS_METASCOPES` se pueden copiar directamente desde la pantalla de credenciales de servidor a servidor OAuth del proyecto de Adobe Developer Console.

## Asignación de entradas

Con el valor de credencial de servidor a servidor OAuth establecido en el archivo `.env`, deben asignarse a entradas de acción de AppBuilder para que puedan leerse en la propia acción. Para ello, agregue entradas para cada variable en la acción `ext.config.yaml` `inputs` con el formato: `PARAMS_INPUT_NAME: $ENV_KEY`.

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
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

Las claves definidas en `inputs` están disponibles en el objeto `params` proporcionado a la acción de App Builder.

## Credenciales de servidor a servidor OAuth para el token de acceso

En la acción App Builder, las credenciales de servidor a servidor OAuth están disponibles en el objeto `params`. Estas credenciales permiten generar el token de acceso con [bibliotecas OAuth 2.0](https://oauth.net/code/). O puede usar la [biblioteca de búsqueda de nodos](https://www.npmjs.com/package/node-fetch) para realizar una solicitud al POST al extremo del token de IMS de Adobe y obtener el token de acceso.

En el ejemplo siguiente se muestra cómo utilizar la biblioteca `node-fetch` para realizar una solicitud de POST al extremo de token de IMS de Adobe a fin de obtener el token de acceso.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const tokenResponse = await fetch(adobeIMSV3TokenEndpointURL, options);
    const tokenResponseJSON = await tokenResponse.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = tokenResponseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const aemDataResponse = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!aemDataResponse.ok) { throw new Error("Request to API failed with status code " + aemDataResponse.status);}

    // API data
    let data = await aemDataResponse.json();

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
