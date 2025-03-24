---
title: Uso de AEM Headless SDK
description: Aprenda a hacer consultas de GraphQL con AEM sin encabezado SDK.
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
duration: 200
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 6%

---

# AEM Headless SDK

AEM Headless SDK es un conjunto de bibliotecas que los clientes pueden utilizar para interactuar rápida y fácilmente con las API de AEM Headless a través de HTTP.

AEM Headless SDK está disponible para varias plataformas:

+ [SDK de AEM sin encabezado para exploradores del lado del cliente (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [SDK de AEM sin encabezado del lado del servidor/Nodo.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [SDK de AEM sin encabezado para Java™](https://github.com/adobe/aem-headless-client-java)

## Consultas persistentes de GraphQL

Consultar AEM con GraphQL mediante consultas persistentes (a diferencia de [consultas de GraphQL definidas por el cliente](#graphl-queries)) permite a los desarrolladores mantener una consulta (pero no sus resultados) en AEM y, a continuación, solicitar que la consulta se ejecute por nombre. Las consultas persistentes son similares al concepto de procedimientos almacenados en bases de datos SQL.

Las consultas persistentes tienen un mayor rendimiento que las consultas GraphQL definidas por el cliente, ya que las consultas persistentes se ejecutan mediante HTTP GET, que puede almacenarse en caché en los niveles CDN y AEM Dispatcher. Las consultas persistentes también están en vigor, definen una API y desvinculan la necesidad de que el desarrollador comprenda los detalles de cada modelo de fragmento de contenido.

### Ejemplos de código{#persisted-graphql-queries-code-examples}

A continuación se muestran ejemplos de código de cómo ejecutar una consulta persistente de GraphQL contra AEM.

+++ Ejemplo de JavaScript

Instale [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) ejecutando el comando `npm install` desde la raíz de su proyecto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Este ejemplo de código muestra cómo consultar AEM usando el módulo [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm con sintaxis `async/await`. AEM Headless SDK for JavaScript también admite [Sintaxis de promesas](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Este código supone que se ha creado una consulta persistente con el nombre `wknd/adventureNames` en AEM Author y que se ha publicado en AEM Publish.

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

+++ Ejemplo de React useEffect(..)

Instale [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) ejecutando el comando `npm install` desde la raíz del proyecto React.

```
$ npm i @adobe/aem-headless-client-js
```

En este ejemplo de código se muestra cómo utilizar el enlace [React useEffect(..)](https://reactjs.org/docs/hooks-effect.html) para ejecutar una llamada asincrónica a AEM GraphQL.

El uso de `useEffect` para realizar la llamada asincrónica de GraphQL en React resulta útil porque:

1. Proporciona un contenedor sincrónico para la llamada asincrónica a AEM.
1. Reduce la necesidad de volver a consultar AEM.

Este código supone que se ha creado una consulta persistente con el nombre `wknd-shared/adventure-by-slug` en AEM Author y que se ha publicado en AEM Publish mediante GraphiQL.

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

AEM admite consultas GraphQL AEM definidas por el cliente, pero es recomendable que use [consultas GraphQL persistentes](#persisted-graphql-queries).

## Webpack 5+

El SDK de JS sin encabezado de AEM tiene dependencias en `util` que no se incluye en Webpack 5+ de forma predeterminada. Si utiliza Webpack 5+ y recibe el siguiente error:

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
