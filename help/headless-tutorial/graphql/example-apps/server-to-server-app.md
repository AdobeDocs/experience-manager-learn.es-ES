---
title: 'AEM Aplicación Node.js de servidor a servidor: Ejemplo sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación Node.js del lado del servidor muestra cómo consultar contenido mediante las API de GraphQL de la interfaz de usuario de la aplicación de datos usando consultas persistentes.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM sin encabezado as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
source-git-commit: d3ee129cb228f02d5a5846465400c04ce81dfbb5
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 6%

---

# Aplicación Node.js de servidor a servidor

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación de servidor a servidor muestra cómo consultar contenido mediante API de GraphQL de mediante consultas persistentes e imprimirlo en el terminal.

![AEM Aplicación Node.js de servidor a servidor con interfaz sin encabezado](./assets/server-to-server-app/server-to-server-app.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Node.js v18](https://nodejs.org/en)
+ [Git](https://git-scm.com/)

## AEM requisitos de

AEM La aplicación Node.js funciona con las siguientes opciones de implementación de la. Todas las implementaciones requieren lo siguiente [Sitio WKND 3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) para instalar.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=es)
+ Opcionalmente, [credenciales de servicio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) si autoriza solicitudes (por ejemplo, conectándose al servicio de creación de AEM).

Esta aplicación Node.js puede conectarse a AEM Author o AEM Publish en función de los parámetros de la línea de comandos.

## Utilización

1. Clonar el `adobe/aem-guides-wknd-graphql` repositorio:

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

AEM A continuación se muestra un resumen de cómo se crea la aplicación Node.js de servidor a servidor, cómo se conecta a la aplicación sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

AEM AEM El caso de uso más habitual de las aplicaciones sin encabezado de servidor a servidor es sincronizar los datos de fragmentos de contenido de los servidores de a otros sistemas, aunque esta aplicación es intencionalmente sencilla e imprime los resultados JSON de la consulta persistente.

### Consultas persistentes

AEM AEM Siguiendo las prácticas recomendadas de sin encabezado, la aplicación utiliza consultas persistentes de GraphQL para consultar datos de aventuras, lo que le resulta útil para la consulta. La aplicación utiliza dos consultas persistentes:

+ `wknd/adventures-all` AEM consulta persistente, que devuelve todas las aventuras en con un conjunto abreviado de propiedades de, que se han guardado de forma predeterminada. Esta consulta persistente genera la lista de aventuras de la vista inicial.

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

### AEM Crear cliente sin encabezado

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

AEM Las consultas persistentes se ejecutan a través de la GET HTTP y, por lo tanto, la variable [AEM Cliente sin encabezado para Node.js](https://github.com/adobe/aem-headless-client-nodejs) está acostumbrado a [ejecutar las consultas de GraphQL persistentes](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) AEM contra y recupera el contenido de la aventura.

La consulta persistente se invoca llamando a `aemHeadlessClient.runPersistedQuery(...)`y pasando el nombre de la consulta de GraphQL persistente. Una vez que GraphQL devuelva los datos, páselos al `doSomethingWithDataFromAEM(..)` , que imprime los resultados, pero normalmente enviaría los datos a otro sistema o generaría algún resultado basado en los datos recuperados.

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
