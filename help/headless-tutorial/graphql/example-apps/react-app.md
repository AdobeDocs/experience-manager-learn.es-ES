---
title: 'AEM Aplicación React: Ejemplo De Aplicación Sin Encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación de React muestra cómo consultar contenido mediante API de GraphQL de utilizando consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-11-09T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 6%

---

# Aplicación React{#react-app}

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación de React muestra cómo consultar contenido mediante API de GraphQL de utilizando consultas persistentes. AEM El cliente sin encabezado para JavaScript se utiliza para ejecutar las consultas persistentes de GraphQL que alimentan la aplicación.

![AEM Aplicación React con sin encabezado](./assets/react-app/react-app.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [tutorial paso a paso completo](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es) La descripción de cómo se creó esta aplicación React está disponible.

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM requisitos de

AEM La aplicación React funciona con las siguientes opciones de implementación de. Todas las implementaciones requieren lo siguiente [Sitio WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0) para instalar.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=es)
+ Configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)
+ [AEM Inicio rápido de 6.5 SP13+](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es?lang=es#install-local-aem-instances)

La aplicación React está diseñada para conectarse a un __AEM Publish__ , sin embargo, puede obtener contenido de AEM Author si la autenticación se proporciona en la configuración de la aplicación React.

## Utilización

1. Clonar el `adobe/aem-guides-wknd-graphql` repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite el `aem-guides-wknd-graphql/react-app/.env.development` archivo y establecer `REACT_APP_HOST_URI` AEM para apuntar a su objetivo de la.

   Actualice el método de autenticación si se conecta a una instancia de autor.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   
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

+ `wknd/adventures-all` AEM consulta persistente, que devuelve todas las aventuras en con un conjunto abreviado de propiedades de, que se han guardado de forma predeterminada. Esta consulta persistente genera la lista de aventuras de la vista inicial.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` consulta persistente, que devuelve una sola aventura de `slug` (una propiedad personalizada que identifica de forma exclusiva una aventura) con un conjunto completo de propiedades. Esta consulta persistente activa las vistas de detalles de aventura.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

AEM Las consultas persistentes se ejecutan a través de la GET HTTP y, por lo tanto, la variable [AEM Cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js) está acostumbrado a [ejecutar las consultas de GraphQL persistentes](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) AEM contra y cargar el contenido de la aventura en la aplicación.

Cada consulta persistente tiene un React correspondiente [useEffect](https://reactjs.org/docs/hooks-effect.html) conectar `src/api/usePersistedQueries.js`AEM , que llama de forma asíncrona al punto final de consulta persistente de la GET HTTP de la y devuelve los datos de la aventura.

Cada función, a su vez, invoca el `aemHeadlessClient.runPersistedQuery(...)`, ejecutando la consulta GraphQL persistente.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity) {
  ...
  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    const queryParameters = { activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryParameters);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all");
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

   Invoca `getAdventuresByActivity(..)` de `src/api/usePersistedQueries.js` y muestra las aventuras devueltas en una lista.

+ `src/components/AdventureDetail.js`

   Invoca el `getAdventureBySlug(..)` uso del `slug` parámetro pasado a través de la selección de aventuras en `Adventures` y muestra los detalles de una sola aventura.

### Variables de entorno

Varios [variables de entorno](https://create-react-app.dev/docs/adding-custom-environment-variables) AEM se utilizan para conectarse a un entorno de. El valor predeterminado se conecta a AEM Publish, que se ejecuta en `http://localhost:4503`. Actualice el `.env.development` AEM , para cambiar la conexión de la:

+ `REACT_APP_HOST_URI=http://localhost:4502`AEM : Se establece en el host de destino de
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: establezca la ruta del extremo de GraphQL. Esta aplicación de React no la utiliza, ya que solo utiliza consultas persistentes.
+ `REACT_APP_AUTH_METHOD=`: el método de autenticación preferido. Opcional. De forma predeterminada, no se utiliza ninguna autenticación.
   + `service-token`AEM : utilice las credenciales del servicio para obtener un token de acceso en el as a Cloud Service de la
   + `dev-token`AEM : utilice el token de desarrollo para el desarrollo local en el as a Cloud Service
   + `basic`: utilice usuario/pase para el desarrollo local con el autor local de AEM
   + AEM Déjelo en blanco para conectarse a la red sin tener que realizar la autenticación.
+ `REACT_APP_AUTHORIZATION=admin:admin`: establezca las credenciales de autenticación básicas que se utilizarán si se conecta a un entorno de AEM Author (solo para desarrollo). Si se conecta a un entorno de publicación, esta configuración no es necesaria.
+ `REACT_APP_DEV_TOKEN`: cadena de token de desarrollador. Para conectarse a una instancia remota, además de la autenticación básica (user:pass), puede utilizar la autenticación del portador con el token de DEV de la consola de Cloud
+ `REACT_APP_SERVICE_TOKEN`: ruta al archivo de credenciales de servicio. Para conectarse a una instancia remota, la autenticación también se puede realizar con el token de servicio (descargue el archivo desde Developer Console).

### AEM Solicitudes de proxy

Al utilizar el servidor de desarrollo de Webpack (`npm start`) el proyecto se basa en un [configuración de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) usando `http-proxy-middleware`. El archivo se configura en [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) y se basa en varias variables de entorno personalizadas establecidas en `.env` y `.env.development`.

AEM Si se conecta a un entorno de autor de, la variable [es necesario configurar el método de autenticación](#environment-variables).

### Uso compartido de recursos de origen cruzado (CORS)

AEM AEM Esta aplicación de React se basa en una configuración CORS basada en el que se ejecuta en el entorno de destino de la aplicación de React y supone que la aplicación de React se ejecuta en `http://localhost:3000` en modo de desarrollo. El [Configuración de CORS](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) forma parte de [Sitio WKND](https://github.com/adobe/aem-guides-wknd).

![Configuración de CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)
