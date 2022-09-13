---
title: 'Siguiente.js: ejemplo AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación Next.js muestra cómo consultar contenido mediante las API de GraphQL AEM mediante consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
source-git-commit: b2bf2a8e454d7ccd09819f2a38e58f7c303cb066
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 2%

---

# Aplicación Next.js

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación Next.js muestra cómo consultar contenido mediante las API de GraphQL AEM mediante consultas persistentes. El cliente AEM sin encabezado para JavaScript se utiliza para ejecutar las consultas persistentes de GraphQL que alimentan la aplicación.

![Aplicación Next.js con AEM sin encabezado](./assets/next-js/next-js.png)

Consulte la [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Node.js v16+](https://nodejs.org/en/)
+ [npm 8+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM requisitos

La aplicación Next.js funciona con las siguientes opciones de implementación AEM. Todas las implementaciones requieren [WKND compartido v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest), [Sitio WKND v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)o [Complemento de demostración de referencia](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html) para instalar en el entorno as a Cloud Service AEM.

Este ejemplo de aplicación Next.js está diseñada para conectarse a __AEM Publish__ servicio.

### Requisitos de AEM Author

Next.js está diseñado para conectarse a __AEM Publish__ y acceder a contenido no protegido. El archivo Next.js se puede configurar para conectarse a AEM Author a través del `.env` propiedades descritas a continuación. Las imágenes proporcionadas por AEM Author requieren autenticación y, por lo tanto, el usuario que accede a la aplicación Next.js también debe iniciar sesión en AEM Author.

## Utilización

1. Clonar el `adobe/aem-guides-wknd-graphql` repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite el `aem-guides-wknd-graphql/next-js/.env.local` archivo y conjunto `NEXT_PUBLIC_AEM_HOST` al servicio AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Si se conecta al servicio AEM Author, la autenticación debe proporcionarse, ya que el servicio AEM Author es seguro de forma predeterminada.

   Para utilizar un conjunto de cuentas de AEM local `AEM_AUTH_METHOD=basic` y proporcione el nombre de usuario y la contraseña en la variable `AEM_AUTH_USER` y `AEM_AUTH_PASSWORD` propiedades.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Para usar una [AEM token de desarrollo local as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` y proporcione el valor completo del token de desarrollo en la variable `AEM_AUTH_DEV_TOKEN` propiedad.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Edite el `aem-guides-wknd-graphql/next-js/.env.local` archivo y validar  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` se establece en el extremo correspondiente AEM GraphQL.

   Al usar [WKND compartido](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [Sitio WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), use el `wknd-shared` Punto final de la API de GraphQL.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

   Al usar [Complemento de demostración de referencia](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html), use el `aem-demo-assets` Punto final de la API de GraphQL.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=aem-demo-assets
   ...
   ```

1. Abra un símbolo del sistema e inicie la aplicación Next.js mediante los siguientes comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Una nueva ventana del explorador abre la aplicación Next.js en [http://localhost:3000](http://localhost:3000)
1. La aplicación Next.js muestra una lista de aventuras. Al seleccionar una aventura, se abren sus detalles en una nueva página.

## El código

A continuación se muestra un resumen de cómo se crea la aplicación Next.js, cómo se conecta a AEM sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Consultas persistentes

Siguiendo AEM prácticas recomendadas sin encabezado, la aplicación Next.js utiliza AEM consultas persistentes de GraphQL para consultar datos de aventura. La aplicación utiliza dos consultas persistentes:

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

Cada consulta persistente tiene una función correspondiente en `src/lib//aem-headless-client.js`, que llama al punto final de AEM GraphQL y devuelve los datos de aventura.

Cada función, a su vez, invoca la variable `aemHeadlessClient.runPersistedQuery(...)`, ejecutando la consulta de GraphQL persistente.

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

La aplicación Next.js utiliza dos páginas para presentar los datos de aventura.

+ `src/pages/index.js`

   Usos [getServerSideProps() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) para llamar a `getAllAdventures()` y muestra cada aventura como una tarjeta.

   El uso de `getServerSiteProps()` permite la representación del lado del servidor de esta página de Next.js.

+ `src/pages/adventures/[...slug].js`

   A [Ruta dinámica de Next.js](https://nextjs.org/docs/routing/dynamic-routes) que muestra los detalles de una sola aventura. Esta ruta dinámica recupera previamente los datos de cada aventura utilizando [getStaticProps() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) mediante una llamada a `getAdventureBySlug(..)` usando la variable `slug` parámetro transferido a través de la selección de aventura en la variable `adventures/index.js` página.

   La ruta dinámica puede recuperar previamente los detalles de todas las aventuras mediante [getStaticPaths() de Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) y rellenar todas las permutaciones de ruta posibles en función de la lista completa de aventuras devueltas por la consulta de GraphQL  `getAdventurePaths()`

   El uso de `getStaticPaths()` y `getStaticProps(..)` permitió la generación estática del sitio de estas páginas de Next.js.

## Configuración de implementación

Las aplicaciones de Next.js, especialmente en el contexto del procesamiento del lado del servidor (SSR) y la generación del lado del servidor (SSG), no requieren configuraciones de seguridad avanzadas como el uso compartido de recursos de origen cruzado (CORS).

Sin embargo, si Next.js realiza solicitudes HTTP a AEM desde el contexto del cliente, pueden ser necesarias configuraciones de seguridad en AEM. Consulte la [Tutorial de implementación de aplicación de una sola página AEM sin encabezado](../deployment/spa.md) para obtener más información.

