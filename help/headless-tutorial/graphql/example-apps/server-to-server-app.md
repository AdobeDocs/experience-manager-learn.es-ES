---
title: 'Aplicación Node.js de servidor a servidor: ejemplo sin encabezado de AEM'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager (AEM). Esta aplicación Node.js del lado del servidor muestra cómo consultar contenido mediante las API GraphQL de AEM utilizando consultas persistentes.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 135
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 1%

---

# Aplicación Node.js de servidor a servidor

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager (AEM). Esta aplicación de servidor a servidor muestra cómo consultar contenido mediante las API de GraphQL de AEM utilizando consultas persistentes e imprimirlo en el terminal.

![Aplicación Node.js de servidor a servidor con AEM Headless](./assets/server-to-server-app/server-to-server-app.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Node.js v18](https://nodejs.org/en)
+ [Git](https://git-scm.com/)

## Requisitos de AEM

La aplicación Node.js funciona con las siguientes opciones de implementación de AEM. Todas las implementaciones requieren que esté instalado [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest).

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=es)
+ Opcionalmente, [credenciales de servicio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=es) si autoriza solicitudes (por ejemplo, si se conecta al servicio de autor de AEM).

Esta aplicación Node.js puede conectarse a AEM Author o AEM Publish según los parámetros de la línea de comandos.

## Cómo usar

1. Clonar el repositorio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra un terminal y ejecute los comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. La aplicación se puede ejecutar mediante el comando:

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   Por ejemplo, para ejecutar la aplicación con AEM Publish sin autorización:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   Para ejecutar la aplicación con AEM Author con autorización:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Se debe imprimir en el terminal una lista JSON de las aventuras del sitio de referencia de WKND.

## El código

A continuación se muestra un resumen de cómo se crea la aplicación Node.js de servidor a servidor, cómo se conecta a AEM Headless para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

El caso de uso más habitual de las aplicaciones sin encabezado de AEM de servidor a servidor es sincronizar datos de fragmentos de contenido de AEM con otros sistemas, aunque esta aplicación es intencionalmente sencilla e imprime los resultados JSON de la consulta persistente.

### Consultas persistentes

Siguiendo las prácticas recomendadas de AEM sin encabezado, la aplicación utiliza consultas persistentes de AEM GraphQL para consultar los datos de aventura. La aplicación utiliza dos consultas persistentes:

+ `wknd/adventures-all` persistió en la consulta, lo que devuelve todas las aventuras en AEM con un conjunto abreviado de propiedades. Esta consulta persistente genera la lista de aventuras de la vista inicial.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

### Crear cliente sin encabezado de AEM

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### Ejecutar consulta persistente de GraphQL

Las consultas persistentes de AEM se ejecutan a través de HTTP GET y, por lo tanto, el cliente [AEM Headless para Node.js](https://github.com/adobe/aem-headless-client-nodejs) se usa para [ejecutar las consultas persistentes de GraphQL](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) contra AEM y recuperar el contenido de aventura.

La consulta persistente se invoca llamando a `aemHeadlessClient.runPersistedQuery(...)` y pasando el nombre de la consulta GraphQL persistente. Una vez que GraphQL devuelva los datos, páselos a la función `doSomethingWithDataFromAEM(..)` simplificada, que imprime los resultados, pero normalmente enviaría los datos a otro sistema o generaría algún resultado basado en los datos recuperados.

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
