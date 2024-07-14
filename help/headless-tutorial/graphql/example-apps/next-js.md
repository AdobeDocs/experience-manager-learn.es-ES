---
title: 'AEM Siguiente.js: Ejemplo de la versión sin encabezado de'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación Next.js muestra cómo consultar contenido utilizando las API de GraphQL que utilizan consultas persistentes, de manera que se pueda usar el contenido de la aplicación para la búsqueda de datos de manera predeterminada.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM sin encabezado as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 210
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Aplicación Next.js

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). AEM Esta aplicación Next.js muestra cómo consultar contenido utilizando las API de GraphQL que utilizan consultas persistentes, de manera que se pueda usar el contenido de la aplicación para la búsqueda de datos de manera predeterminada. AEM El cliente sin encabezado para JavaScript se utiliza para ejecutar las consultas persistentes de GraphQL que alimentan la aplicación.

AEM ![Aplicación Next.js con encabezado sin encabezado](./assets/next-js/next-js.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM requisitos de

AEM La aplicación Next.js funciona con las siguientes opciones de implementación de. Todas las implementaciones requieren que [WKND Shared v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) o [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) estén instalados en el entorno de AEM as a Cloud Service.

AEM Esta aplicación Next.js de ejemplo está diseñada para conectarse al servicio __Publish__.

### AEM Requisitos de autor de

AEM El archivo Next.js está diseñado para conectarse al servicio __Publish__ y tener acceso a contenido no protegido. AEM El archivo Next.js se puede configurar para que se conecte a la interfaz de usuario de Autor mediante las propiedades de `.env` que se describen a continuación. AEM AEM Las imágenes proporcionadas desde Autor de la requieren autenticación y, por lo tanto, el usuario que acceda a la aplicación Next.js también debe iniciar sesión en Autor de la.

## Cómo usar

1. Clonar el repositorio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. AEM Edite el archivo `aem-guides-wknd-graphql/next-js/.env.local` y establezca `NEXT_PUBLIC_AEM_HOST` en el servicio de.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   AEM AEM Si se conecta a servicio de Author, la autenticación debe proporcionarse ya que el servicio de Author es seguro de forma predeterminada.

   AEM Para usar un conjunto de cuentas de usuario local `AEM_AUTH_METHOD=basic` y proporcionar el nombre de usuario y la contraseña en las propiedades `AEM_AUTH_USER` y `AEM_AUTH_PASSWORD`.

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

1. AEM Edite el archivo `aem-guides-wknd-graphql/next-js/.env.local` y valide que `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` se ha establecido en el extremo de GraphQL de la cuenta de usuario correspondiente.

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

AEM A continuación se muestra un resumen de cómo se crea la aplicación Next.js, cómo se conecta a la aplicación sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Consultas persistentes

AEM AEM Siguiendo las prácticas recomendadas de sin encabezado de la aplicación, Next.js utiliza consultas persistentes de GraphQL de la aplicación para consultar los datos de aventura de la aplicación de la manera más rápida y sencilla. La aplicación utiliza dos consultas persistentes:

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

AEM Cada consulta persistente tiene una función correspondiente en `src/lib//aem-headless-client.js`, que llama al punto final de GraphQL de la y devuelve los datos de la aventura.

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

AEM AEM Sin embargo, si Next.js realiza solicitudes HTTP a las que se debe acceder desde el contexto del cliente, es posible que se requieran las configuraciones de seguridad en las que se realiza la petición de acceso desde el. AEM Revise el [tutorial de implementación de aplicación de una sola página sin encabezado](../deployment/spa.md) para obtener más detalles.
