---
title: 'AEM Siguiente.js: Ejemplo de la versión sin encabezado de'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación Next.js muestra cómo realizar consultas en el contenido mediante API de GraphQL de utilizando consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 1%

---

# Aplicación Next.js

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación Next.js muestra cómo realizar consultas en el contenido mediante API de GraphQL de utilizando consultas persistentes. AEM El cliente sin encabezado para JavaScript se utiliza para ejecutar las consultas persistentes de GraphQL que alimentan la aplicación.

![AEM Aplicación Next.js con sin encabezado](./assets/next-js/next-js.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM requisitos de

AEM La aplicación Next.js funciona con las siguientes opciones de implementación de. Todas las implementaciones requieren [WKND compartido v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [Sitio WKND v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) AEM que se va a instalar en el entorno as a Cloud Service de la.

Esta aplicación Next.js de ejemplo está diseñada para conectarse a __AEM Publish__ servicio.

### Requisitos de AEM Author

Next.js está diseñado para conectarse a __AEM Publish__ y acceder a contenido no protegido. Next.js se puede configurar para conectarse a AEM Author mediante el `.env` propiedades que se describen a continuación. Las imágenes obtenidas desde AEM Author requieren autenticación y, por lo tanto, el usuario que accede a la aplicación Next.js también debe iniciar sesión en AEM Author.

## Utilización

1. Clonar el `adobe/aem-guides-wknd-graphql` repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite el `aem-guides-wknd-graphql/next-js/.env.local` archivo y establecer `NEXT_PUBLIC_AEM_HOST` AEM al servicio de la.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Si se conecta al servicio de AEM Author, debe proporcionarse autenticación, ya que el servicio de AEM Author es seguro de forma predeterminada.

   AEM Para usar un conjunto de cuentas de la cuenta de local `AEM_AUTH_METHOD=basic` y proporcione el nombre de usuario y la contraseña en la `AEM_AUTH_USER` y `AEM_AUTH_PASSWORD` propiedades.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Para utilizar un [AEM Token de desarrollo local as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` y proporcione el valor de token de desarrollo completo en la variable `AEM_AUTH_DEV_TOKEN` propiedad.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Edite el `aem-guides-wknd-graphql/next-js/.env.local` archivar y validar  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` AEM se establece en el extremo de GraphQL de la aplicación correspondiente.

   Al utilizar [WKND compartido](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [Sitio WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), use el `wknd-shared` Extremo de API de GraphQL.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. Abra un símbolo del sistema e inicie la aplicación Next.js con los siguientes comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Una nueva ventana del explorador abre la aplicación Next.js en [http://localhost:3000](http://localhost:3000)
1. La aplicación Next.js muestra una lista de aventuras. Al seleccionar una aventura, se abren sus detalles en una nueva página.

## El código

AEM A continuación se muestra un resumen de cómo se crea la aplicación Next.js, cómo se conecta a la aplicación sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Consultas persistentes

AEM AEM Siguiendo las prácticas recomendadas de sin encabezado de la aplicación, Next.js utiliza consultas persistentes de GraphQL de la aplicación para consultar los datos de aventura de la aplicación de la manera más rápida y sencilla. La aplicación utiliza dos consultas persistentes:

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

Cada consulta persistente tiene una función correspondiente en `src/lib//aem-headless-client.js`AEM , que llama al punto final de GraphQL de la y devuelve los datos de la aventura.

Cada función, a su vez, invoca el `aemHeadlessClient.runPersistedQuery(...)`, ejecutando la consulta GraphQL persistente.

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### Páginas

La aplicación Next.js utiliza dos páginas para presentar los datos de la aventura.

+ `src/pages/index.js`

   Usos [getServerSideProps() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) para llamar a `getAllAdventures()` y muestra cada aventura como una tarjeta.

   El uso de `getServerSiteProps()` permite la representación del lado del servidor de esta página Next.js.

+ `src/pages/adventures/[...slug].js`

   A [Ruta dinámica de Next.js](https://nextjs.org/docs/routing/dynamic-routes) que muestra los detalles de una sola aventura. Esta ruta dinámica recupera previamente los datos de cada aventura mediante [getStaticProps() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) mediante una llamada a `getAdventureBySlug(..)` uso del `slug` parámetro pasado a través de la selección de aventuras en `adventures/index.js` página.

   La ruta dinámica puede recuperar previamente los detalles de todas las aventuras utilizando [getStaticPaths() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) y rellenando todas las permutaciones de ruta posibles en función de la lista completa de aventuras devueltas por la consulta de GraphQL  `getAdventurePaths()`

   El uso de `getStaticPaths()` y `getStaticProps(..)` permitía la generación estática del sitio de estas páginas de Next.js.

## Configuración de implementación

Las aplicaciones Next.js, especialmente en el contexto del procesamiento del lado del servidor (SSR) y la generación del lado del servidor (SSG), no requieren configuraciones de seguridad avanzadas como el uso compartido de recursos de origen cruzado (CORS).

AEM AEM Sin embargo, si Next.js realiza solicitudes HTTP a las que se debe acceder desde el contexto del cliente, es posible que se requieran las configuraciones de seguridad en las que se realiza la petición de acceso desde el. Revise la [AEM Tutorial de implementación de aplicación de una sola página sin encabezado](../deployment/spa.md) para obtener más información.
