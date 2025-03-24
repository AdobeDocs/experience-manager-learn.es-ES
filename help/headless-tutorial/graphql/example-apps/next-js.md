---
title: 'Next.js: ejemplo de AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager (AEM). Esta aplicación Next.js muestra cómo consultar contenido mediante las API GraphQL de AEM utilizando consultas persistentes.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 210
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Aplicación Next.js

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager (AEM). Esta aplicación Next.js muestra cómo consultar contenido mediante las API GraphQL de AEM utilizando consultas persistentes. El cliente sin encabezado de AEM para JavaScript se utiliza para ejecutar las consultas persistentes de GraphQL que alimentan la aplicación.

![Aplicación Next.js con AEM Headless](./assets/next-js/next-js.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## Requisitos de AEM

La aplicación Next.js funciona con las siguientes opciones de implementación de AEM. Todas las implementaciones requieren que [WKND Shared v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) estén instalados en el entorno de AEM as a Cloud Service.

Esta aplicación Next.js de ejemplo está diseñada para conectarse al servicio __AEM Publish__.

### Requisitos de AEM Author

El archivo Next.js está diseñado para conectarse al servicio __AEM Publish__ y obtener acceso al contenido no protegido. Se puede configurar Next.js para que se conecte a AEM Author mediante las propiedades `.env` que se describen a continuación. Las imágenes obtenidas desde AEM Author requieren autenticación y, por lo tanto, el usuario que accede a la aplicación Next.js también debe iniciar sesión en AEM Author.

## Cómo usar

1. Clonar el repositorio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite el archivo `aem-guides-wknd-graphql/next-js/.env.local` y establezca `NEXT_PUBLIC_AEM_HOST` en el servicio AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Si se conecta al servicio de AEM Author, la autenticación debe proporcionarse, ya que el servicio de AEM Author es seguro de forma predeterminada.

   Para usar un conjunto de cuentas de AEM local `AEM_AUTH_METHOD=basic` y proporcionar el nombre de usuario y la contraseña en las propiedades `AEM_AUTH_USER` y `AEM_AUTH_PASSWORD`.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Para usar un [token de desarrollo local de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token), establezca `AEM_AUTH_METHOD=dev-token` y proporcione el valor completo del token de desarrollo en la propiedad `AEM_AUTH_DEV_TOKEN`.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Edite el archivo `aem-guides-wknd-graphql/next-js/.env.local` y valide que `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` está establecido en el punto de conexión de AEM GraphQL apropiado.

   Cuando use [WKND compartido](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [Sitio WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), use el extremo de la API de GraphQL `wknd-shared`.

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

A continuación se muestra un resumen de cómo se crea la aplicación Next.js, cómo se conecta a AEM Headless para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Consultas persistentes

Siguiendo las prácticas recomendadas de AEM sin encabezado, la aplicación Next.js utiliza consultas persistentes de AEM GraphQL para consultar datos de aventura. La aplicación utiliza dos consultas persistentes:

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

Las consultas persistentes de AEM se ejecutan a través de HTTP GET y, por lo tanto, [el cliente sin encabezado de AEM para JavaScript](https://github.com/adobe/aem-headless-client-js) se usa para [ejecutar las consultas de GraphQL persistentes](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) en AEM y cargar el contenido de aventura en la aplicación.

Cada consulta persistente tiene una función correspondiente en `src/lib//aem-headless-client.js`, que llama al punto final de AEM GraphQL y devuelve los datos de la aventura.

A su vez, cada función invoca `aemHeadlessClient.runPersistedQuery(...)` y ejecuta la consulta de GraphQL persistente.

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

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### Páginas

La aplicación Next.js utiliza dos páginas para presentar los datos de la aventura.

+ `src/pages/index.js`

  Usa getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) de [Next.js para llamar a `getAllAdventures()` y muestra cada aventura como una tarjeta.

  El uso de `getServerSiteProps()` permite la representación del lado del servidor de esta página Next.js.

+ `src/pages/adventures/[...slug].js`

  Una ruta dinámica [Next.js](https://nextjs.org/docs/routing/dynamic-routes) que muestra los detalles de una sola aventura. Esta ruta dinámica recupera previamente los datos de cada aventura usando [Next.js&#39;s getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) mediante una llamada a `getAdventureBySlug(slug, queryVariables)` usando el parámetro `slug` pasado a través de la selección de aventuras en la página `adventures/index.js`, y `queryVariables` para controlar el formato, la anchura y la calidad de la imagen.

  La ruta dinámica puede recuperar previamente los detalles de todas las aventuras mediante [GetStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) de Next.js y rellenando todas las permutaciones de ruta posibles en función de la lista completa de aventuras devuelta por la consulta de GraphQL `getAdventurePaths()`

  El uso de `getStaticPaths()` y `getStaticProps(..)` permitió la generación estática del sitio de estas páginas de Next.js.

## Configuración de implementación

Las aplicaciones Next.js, especialmente en el contexto del procesamiento del lado del servidor (SSR) y la generación del lado del servidor (SSG), no requieren configuraciones de seguridad avanzadas como el uso compartido de recursos de origen cruzado (CORS).

Sin embargo, si Next.js realiza solicitudes HTTP a AEM desde el contexto del cliente, es posible que se requieran configuraciones de seguridad en AEM. Consulte el [tutorial de implementación de aplicación de una sola página sin encabezado de AEM](../deployment/spa.md) para obtener más información.
