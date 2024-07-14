---
title: 'AEM Aplicación React: Ejemplo De Aplicación Sin Encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación de React muestra cómo consultar contenido utilizando las API de GraphQL de utilizando consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM sin encabezado as a Cloud Service" before-title="false"
duration: 256
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 2%

---

# Aplicación React{#react-app}

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación de React muestra cómo consultar contenido utilizando las API de GraphQL de utilizando consultas persistentes. AEM El cliente sin encabezado para JavaScript se utiliza para ejecutar las consultas persistentes de GraphQL que alimentan la aplicación.

AEM ![Reaccionar aplicación sin encabezado](./assets/react-app/react-app.png) con la aplicación sin encabezado

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

Hay disponible un [tutorial paso a paso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es) que describe cómo se creó esta aplicación de React.

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM requisitos de

AEM La aplicación React funciona con las siguientes opciones de implementación de. Todas las implementaciones requieren que esté instalado [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest).

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)
   + Requiere [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)

AEM La aplicación React está diseñada para conectarse a un entorno de __Publish AEM__, pero puede obtener contenido de la aplicación Author si se proporciona autenticación en la configuración de la aplicación React.

## Cómo usar

1. Clonar el repositorio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. AEM Edite el archivo `aem-guides-wknd-graphql/react-app/.env.development` y configure `REACT_APP_HOST_URI` para que apunte a su destino de manera que se ejecute de manera más fácil y más fácil de usar

   Actualice el método de autenticación si se conecta a una instancia de autor.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. Abra un terminal y ejecute los comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Se debe cargar una nueva ventana del explorador en [http://localhost:3000](http://localhost:3000)
1. En la aplicación se debe mostrar una lista de las aventuras del sitio de referencia de WKND.

## El código

AEM A continuación se muestra un resumen de cómo se crea la aplicación React, cómo se conecta a las consultas sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Consultas persistentes

AEM AEM Siguiendo las prácticas recomendadas sin encabezado de la aplicación React, utiliza consultas persistentes de GraphQL para consultar datos de aventuras, lo cual es muy útil para las consultas. La aplicación utiliza dos consultas persistentes:

+ AEM `wknd/adventures-all` persistió en la consulta, lo que devuelve todas las aventuras en la que se ha realizado un seguimiento con un conjunto abreviado de propiedades. Esta consulta persistente genera la lista de aventuras de la vista inicial.

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

+ `wknd/adventure-by-slug` persistió en la consulta, lo que devuelve una sola aventura de `slug` (una propiedad personalizada que identifica de forma exclusiva una aventura) con un conjunto completo de propiedades. Esta consulta persistente activa las vistas de detalles de aventura.

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Ejecutar consulta persistente de GraphQL

AEM consultas persistentes se ejecutan a través de la GET AEM HTTP y, por lo tanto, [Cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js) se usa para [ejecutar las consultas persistentes de GraphQL AEM](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) en contra de las consultas y cargar el contenido de aventura en la aplicación.

AEM Cada consulta persistente tiene un vínculo React [useEffect](https://reactjs.org/docs/hooks-effect.html) correspondiente en `src/api/usePersistedQueries.js`, que llama asincrónicamente al punto final de la consulta persistente de la GET HTTP de la aplicación y devuelve los datos de la aventura.

A su vez, cada función invoca `aemHeadlessClient.runPersistedQuery(...)` y ejecuta la consulta de GraphQL persistente.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}
```

### Vistas

La aplicación React utiliza dos vistas para presentar los datos de la aventura en la experiencia web.

+ `src/components/Adventures.js`

  Invoca `getAdventuresByActivity(..)` desde `src/api/usePersistedQueries.js` y muestra las aventuras devueltas en una lista.

+ `src/components/AdventureDetail.js`

  Invoca a `getAdventureBySlug(..)` mediante el parámetro `slug` pasado mediante la selección de aventuras en el componente `Adventures` y muestra los detalles de una sola aventura.

### Variables de entorno

AEM Se usan varias [variables de entorno](https://create-react-app.dev/docs/adding-custom-environment-variables) para conectarse a un entorno de la. AEM El valor predeterminado se conecta a Publish de la que se ejecuta en `http://localhost:4503`. AEM Actualice el archivo `.env.development` para cambiar la conexión de la:

+ AEM `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`: establecido en el host de destino de la
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: establezca la ruta de acceso del extremo de GraphQL. Esta aplicación de React no la utiliza, ya que solo utiliza consultas persistentes.
+ `REACT_APP_AUTH_METHOD=`: método de autenticación preferido. Opcional. De forma predeterminada, no se utiliza ninguna autenticación.
   + `service-token`: use las credenciales del servicio para obtener un token de acceso en AEM as a Cloud Service
   + `dev-token`: use el token de desarrollo para el desarrollo local en AEM as a Cloud Service
   + AEM `basic`: usar usuario/pase para el desarrollo local con el autor de la local
   + AEM Déjelo en blanco para conectarse a la red sin tener que realizar la autenticación.
+ AEM `REACT_APP_AUTHORIZATION=admin:admin`: establezca las credenciales de autenticación básicas que se usarán si se conecta a un entorno de autor de (solo para desarrollo). Si se conecta a un entorno de Publish, esta configuración no es necesaria.
+ `REACT_APP_DEV_TOKEN`: cadena de token de desarrollador. Para conectarse a una instancia remota, además de la autenticación básica (user:pass), puede utilizar la autenticación del portador con el token de DEV de la consola de Cloud
+ `REACT_APP_SERVICE_TOKEN`: ruta al archivo de credenciales de servicio. Para conectarse a una instancia remota, la autenticación también se puede realizar con el token de servicio (descargue el archivo desde Developer Console).

### AEM Solicitudes de proxy

Al usar el servidor de desarrollo de Webpack (`npm start`), el proyecto depende de una [configuración proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) que usa `http-proxy-middleware`. El archivo está configurado en [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) y depende de varias variables de entorno personalizadas establecidas en `.env` y `.env.development`.

AEM Si se conecta a un entorno de autor de, el método de autenticación [correspondiente debe configurarse](#environment-variables).

### Uso compartido de recursos de origen cruzado (CORS)

AEM AEM Esta aplicación de React se basa en una configuración CORS basada en el que se ejecuta en el entorno de destino de la aplicación de React y supone que la aplicación de React se ejecuta en `http://localhost:3000` en modo de desarrollo.  AEM Revise la [documentación de implementación sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html) para obtener más información sobre cómo configurar CORS.
