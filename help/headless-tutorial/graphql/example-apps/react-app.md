---
title: 'Aplicación React: Ejemplo AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación React muestra cómo consultar contenido mediante las API de GraphQL AEM mediante consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 5%

---

# React App

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación React muestra cómo consultar contenido mediante las API de GraphQL AEM mediante consultas persistentes. El cliente AEM sin encabezado para JavaScript se utiliza para ejecutar las consultas persistentes de GraphQL que alimentan la aplicación.

![Reaccione la aplicación con AEM sin encabezado](./assets/react-app/react-app.png)

Consulte la [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [tutorial paso a paso completo](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es) describe cómo está disponible la compilación de esta aplicación React.

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM requisitos

La aplicación React funciona con las siguientes opciones de implementación AEM. Todas las implementaciones requieren la variable [Sitio WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) para instalar.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)
+ [Inicio rápido de AEM 6.5 SP13+](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es?lang=en#install-local-aem-instances)

La aplicación React está diseñada para conectarse a un __AEM Publish__ , sin embargo, puede obtener contenido de AEM Author si se proporciona autenticación en la configuración de la aplicación React.

## Utilización

1. Clonar el `adobbe/aem-guides-wknd-graphql` repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite el `aem-guides-wknd-graphql/react-app/.env.development` archivo y conjunto `REACT_APP_HOST_URI` para que apunte a su AEM de destino.

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
1. Se debe mostrar en la aplicación una lista de las aventuras del sitio de referencia WKND.

## El código

A continuación se muestra un resumen de cómo se crea la aplicación React, cómo se conecta a AEM sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Consultas persistentes

Siguiendo AEM prácticas recomendadas sin encabezado, la aplicación React utiliza consultas persistentes AEM GraphQL para consultar datos de aventura. La aplicación utiliza dos consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que devuelve todas las aventuras en AEM con un conjunto abreviado de propiedades. Esta consulta persistente impulsa la lista de aventuras de la vista inicial.

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

+ `wknd/adventure-by-slug` consulta persistente, que devuelve una sola aventura de `slug` (una propiedad personalizada que identifica de forma exclusiva una aventura) con un conjunto completo de propiedades. Esta consulta persistente potencia las vistas de detalles de aventura.

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

AEM las consultas persistentes se ejecutan a través de GET HTTP y, por lo tanto, la variable [AEM cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js) se usa para [ejecutar las consultas de GraphQL persistentes](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) con AEM y cargue el contenido de aventura en la aplicación.

Cada consulta persistente tiene una función correspondiente en `src/api/persistedQueries.js`, que llama de forma asíncrona al punto final del GET HTTP AEM y devuelve los datos de aventura.

Cada función, a su vez, invoca la variable `aemHeadlessClient.runPersistedQuery(...)`, ejecutando la consulta de GraphQL persistente.

```js
// src/api/persistedQueries.js

/**
 * Queries a list of all Adventures using the persisted path "wknd-shared/adventures-all"
 * @returns {data, errors}
 */
export const getAllAdventures = async function() {
    return executePersistedQuery('wknd-shared/adventures-all');
}

...

/**
 * Uses the AEM Headless SDK to execute a query besed on a persistedQueryPath and optional query variables
 * @param {*} persistedQueryPath 
 * @param {*} queryVariables 
 * @returns 
 */
 const executePersistedQuery = async function(persistedQueryPath, queryVariables) {

    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

### Vistas

La aplicación React utiliza dos vistas para presentar los datos de aventura en la experiencia web.

+ `src/components/Adventures.js`

   Invocaciones `getAllAdventures()` from `src/api/persistedQueries.js`  y muestra las aventuras devueltas en una lista.

+ `src/components/AdventureDetail.js`

   Invoca el `getAdventureBySlug(..)` usando la variable `slug` parámetro transferido a través de la selección de aventura en la variable `Adventures` y muestra los detalles de una sola aventura.


### Variables de entorno

Varios [variables de entorno](https://create-react-app.dev/docs/adding-custom-environment-variables) se utilizan para conectarse a un entorno de AEM. El valor predeterminado se conecta a AEM Publish que se ejecuta en `http://localhost:4503`. Para cambiar la conexión de AEM, actualice el `.env.development` archivo:

+ `REACT_APP_HOST_URI=http://localhost:4502`: Establecer en AEM host de destino
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Establezca la ruta de acceso del extremo de GraphQL. Esta aplicación React no la usa, ya que esta aplicación solo utiliza consultas persistentes.
+ `REACT_APP_AUTH_METHOD=`: Método de autenticación preferido. Opcional, de forma predeterminada no se utiliza ninguna autenticación.
   + `service-token`: Usar credenciales de servicio para obtener un token de acceso en AEM as a Cloud Service
   + `dev-token`: Utilice el token dev para el desarrollo local en AEM as a Cloud Service
   + `basic`: Utilice user/pass para desarrollo local con AEM Author local
   + Deje en blanco para conectarse a AEM sin autenticación
+ `REACT_APP_AUTHORIZATION=admin:admin`: Establezca credenciales de autenticación básicas para utilizarlas si se conecta a un entorno de AEM Author (solo para desarrollo). Si se conecta a un entorno de publicación, esta configuración no es necesaria.
+ `REACT_APP_DEV_TOKEN`: Cadena de token de desarrollador. Para conectarse a una instancia remota, junto a la autenticación básica (user:pass), puede utilizar la autenticación del portador con el token DEV de la consola de la nube
+ `REACT_APP_SERVICE_TOKEN`: Ruta al archivo de credenciales de servicio. Para conectarse a una instancia remota, la autenticación se puede realizar con el token de servicio también (descargue el archivo desde Developer Console).

### Solicitudes de AEM de proxy

Al utilizar el servidor de desarrollo de webpack (`npm start`) el proyecto se basa en un [configuración de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. El archivo está configurado en [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) y se basa en varias variables de entorno personalizadas configuradas en `.env` y `.env.development`.

Si se conecta a un entorno de creación de AEM, el [es necesario configurar el método de autenticación](#environment-variables).

### Uso compartido de recursos de origen diverso (CORS)

Esta aplicación React se basa en una configuración CORS basada en AEM que se ejecuta en el entorno de AEM de destino y asume que la aplicación React se ejecuta en `http://localhost:3000` en modo de desarrollo. La variable [Configuración CORS](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) forma parte del [Sitio WKND](https://github.com/adobe/aem-guides-wknd).

![Configuración CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)
