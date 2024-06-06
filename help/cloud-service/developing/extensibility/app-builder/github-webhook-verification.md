---
title: Verificación del webhook de Github.com
description: Obtenga información sobre cómo verificar una solicitud de webhook de Github.com en una acción del Generador de aplicaciones.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
source-git-commit: 4b9f784de5fff7d9ba8cf7ddbe1802c271534010
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Verificación del webhook de Github.com

Los webhooks permiten crear o configurar integraciones que se suscriben a ciertos eventos en GitHub.com. Cuando se activa uno de esos eventos, GitHub envía una carga útil de POST HTTP a la dirección URL configurada del gancho web. Sin embargo, por motivos de seguridad, es importante comprobar que la solicitud de webhook entrante proviene en realidad de GitHub y no de un agente malintencionado. Este tutorial le guía para comprobar una solicitud de webhook GitHub.com en una acción del Generador de aplicaciones de Adobe mediante un secreto compartido.

## Configurar el secreto de Github en AppBuilder

1. **Añadir secreto a `.env` archivo:**

   En el del proyecto del Creador de aplicaciones `.env` , agregue una clave personalizada para el secreto GitHub.com webhook:

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **Actualizar `ext.config.yaml` archivo:**

   El `ext.config.yaml` el archivo debe actualizarse para comprobar la solicitud de webhook de GitHub.com.

   - Establecer la acción de AppBuilder `web` configuración a `raw` para recibir el cuerpo de la solicitud sin procesar de GitHub.com.
   - En `inputs` en la configuración de acción de AppBuilder, agregue `GITHUB_SECRET` , asignándolo a la variable `.env` campo que contiene el secreto. El valor de esta clave es `.env` nombre de campo con el prefijo `$`.
   - Configure las variables `require-adobe-auth` anotación en la configuración de acciones de AppBuilder a `false` para permitir que se llame a la acción sin requerir la autenticación de Adobe.

   El resultante `ext.config.yaml` el archivo debería tener un aspecto similar al siguiente:

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## Añadir código de verificación a la acción de AppBuilder

A continuación, añada el código JavaScript que se proporciona a continuación (copiado de [Documentación de GitHub.com](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)) a su acción de AppBuilder. Asegúrese de exportar el `verifySignature` función.

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## Implementar la verificación en la acción de AppBuilder

A continuación, compruebe que la solicitud proviene de GitHub comparando la firma del encabezado de la solicitud con la firma generada por `verifySignature` función.

En el de la acción de AppBuilder `index.js`, agregue el siguiente código a `main` función:


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## Configuración del webhook en GitHub

De nuevo en GitHub.com, proporcione el mismo valor secreto a GitHub.com al crear el webhook. Utilice el valor secreto especificado en su `.env` del archivo `GITHUB_SECRET` clave.

En GitHub.com, vaya a la configuración del repositorio y edite el webhook. En la configuración del webhook, proporcione el valor secreto en `Secret` field. Clic __Actualizar webhook__ en la parte inferior para guardar los cambios.

![Secreto de webhook de Github](./assets/github-webhook-verification/github-webhook-settings.png)

Al seguir estos pasos, se asegura de que la acción del Generador de aplicaciones pueda verificar de forma segura que las solicitudes entrantes de los ganchos web provienen de su gancho web GitHub.com.
