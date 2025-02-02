---
title: AEM Uso del SDK sin encabezado de la
description: Obtenga información sobre cómo realizar consultas de GraphQL AEM mediante el SDK sin encabezado de.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
duration: 200
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 6%

---

# AEM SDK sin encabezado de

AEM AEM El SDK sin encabezado de la aplicación es un conjunto de bibliotecas que los clientes pueden utilizar para interactuar rápida y fácilmente con las API sin encabezado de la aplicación a través de HTTP.

AEM El SDK sin encabezado de la está disponible para varias plataformas:

+ [SDK de AEM sin encabezado para exploradores del lado del cliente (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [SDK de AEM sin encabezado del lado del servidor/Nodo.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [SDK de AEM sin encabezado para Java™](https://github.com/adobe/aem-headless-client-java)

## Consultas persistentes de GraphQL

AEM Consultar el uso de GraphQL mediante consultas persistentes (a diferencia de las [consultas de GraphQL AEM definidas por el cliente](#graphl-queries)) permite a los desarrolladores mantener una consulta (pero no sus resultados) en el formato de consultas persistentes y, a continuación, solicitar que se ejecute la consulta por su nombre. Las consultas persistentes son similares al concepto de procedimientos almacenados en bases de datos SQL.

Las consultas persistentes tienen un mayor rendimiento que las consultas GraphQL definidas por el cliente, ya que las consultas persistentes se ejecutan mediante la GET AEM HTTP, que se puede almacenar en caché en los niveles CDN y Dispatcher de la. Las consultas persistentes también están en vigor, definen una API y desvinculan la necesidad de que el desarrollador comprenda los detalles de cada modelo de fragmento de contenido.

### Ejemplos de código{#persisted-graphql-queries-code-examples}

A continuación se muestran ejemplos de código de cómo ejecutar una consulta persistente de GraphQL AEM contra los usuarios de la base de datos de.

+++ Ejemplo de JavaScript

Instale [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) ejecutando el comando `npm install` desde la raíz de su proyecto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

AEM En este ejemplo de código se muestra cómo realizar consultas en el módulo [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm utilizando la sintaxis `async/await`, con el fin de realizar consultas en el módulo . AEM El SDK sin encabezado de la para JavaScript también admite [sintaxis Promise](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

AEM AEM Este código supone que se ha creado una consulta persistente con el nombre `wknd/adventureNames` en el autor de la y que se ha publicado en el Publish de la.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ React useEffect(..) ejemplo

Instale [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) ejecutando el comando `npm install` desde la raíz del proyecto React.

```
$ npm i @adobe/aem-headless-client-js
```

En este ejemplo de código se muestra cómo utilizar [React useEffect(..) AEM hook](https://reactjs.org/docs/hooks-effect.html) para ejecutar una llamada asincrónica a GraphQL de la.

El uso de `useEffect` para realizar la llamada asincrónica de GraphQL en React resulta útil porque:

1. AEM Proporciona un envoltorio sincrónico para la llamada asincrónica a la red de servicios de soporte de datos de.
1. AEM Reduce los problemas de acceso a la innecesariamente.

AEM AEM Este código supone que se ha creado en el Autor una consulta persistente con el nombre `wknd-shared/adventure-by-slug` y que se ha publicado en el de Publish mediante GraphiQL.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
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

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

Invoque el vínculo React `useEffect` personalizado desde cualquier parte de un componente React.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Se pueden crear nuevos vínculos `useEffect` para cada consulta persistente que use la aplicación React.

+++

<p> </p>

## Consultas de GraphQL

AEM admite consultas de GraphQL AEM definidas por el cliente, aunque es recomendable usar [consultas de GraphQL persistentes](#persisted-graphql-queries), aunque es recomendable usar este tipo de consultas de forma predeterminada.

## Webpack 5+

AEM El SDK de JS sin encabezado de la tiene dependencias en `util` que no se incluye en Webpack 5+ de forma predeterminada. Si utiliza Webpack 5+ y recibe el siguiente error:

```
Compiled with problems:
× ERROR in ./node_modules/@adobe/aio-lib-core-errors/src/AioCoreSDKErrorWrapper.js 12:13-28
Module not found: Error: Can't resolve 'util' in '/Users/me/Code/wknd-headless-examples/node_modules/@adobe/aio-lib-core-errors/src'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
    - install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "util": false }
```

Agregue el(la) siguiente `devDependencies` a su archivo `package.json`:

```json
  "devDependencies": {
    "buffer": "npm:buffer@^6.0.3",
    "crypto": "npm:crypto-browserify@^3.12.0",
    "http": "npm:stream-http@^3.2.0",
    "https": "npm:https-browserify@^1.0.0",
    "stream": "npm:stream-browserify@^3.0.0",
    "util": "npm:util@^0.12.5",
    "zlib": "npm:browserify-zlib@^0.2.0"
  },
```

A continuación, ejecute `npm install` para instalar las dependencias.
