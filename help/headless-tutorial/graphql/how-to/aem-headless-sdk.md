---
title: Uso del SDK sin AEM
description: Obtenga información sobre cómo realizar consultas de GraphQL mediante el SDK AEM sin encabezado.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 595d990b7d8ed3c801a085892fef38d780082a15
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 4%

---

# SDK AEM sin encabezado

El SDK sin encabezado de AEM es un conjunto de bibliotecas que los clientes pueden utilizar para interactuar rápida y fácilmente con AEM API sin encabezado a través de HTTP.

El SDK sin AEM está disponible para varias plataformas:

+ [AEM SDK sin encabezado para exploradores del lado del cliente (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM SDK sin encabezado para server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM SDK sin encabezado para Java™](https://github.com/adobe/aem-headless-client-java)

## Consultas persistentes de GraphQL

Realización de consultas AEM mediante GraphQL con consultas persistentes (en oposición a [consultas de GraphQL definidas por el cliente](#graphl-queries)) permite a los desarrolladores mantener una consulta (pero no sus resultados) en AEM y luego solicitar que la consulta se ejecute con el nombre. Las consultas persistentes son similares al concepto de procedimientos almacenados en bases de datos SQL.

Las consultas persistentes tienen más rendimiento que las consultas GraphQL definidas por el cliente, ya que las consultas persistentes se ejecutan mediante GET HTTP, que se puede almacenar en caché en los niveles CDN y AEM Dispatcher. Las consultas persistentes también están en vigor, definen una API y desvinculan la necesidad de que el desarrollador entienda los detalles de cada modelo de fragmento de contenido.

### Ejemplos de código{#persisted-graphql-queries-code-examples}

Los siguientes son ejemplos de código de cómo ejecutar una consulta persistente de GraphQL contra AEM.

+++ Ejemplo de JavaScript

Instale el [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) ejecutando `npm install` desde la raíz del proyecto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Este ejemplo de código muestra cómo consultar AEM usando la variable [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) módulo npm usando `async/await` sintaxis. El SDK sin AEM para JavaScript también es compatible [Sintaxis de la promesa](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Este código asume una consulta persistente con el nombre `wknd/adventureNames` se ha creado en AEM Author y se ha publicado en AEM Publish.

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

Instale el [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) ejecutando `npm install` desde la raíz del proyecto React.

```
$ npm i @adobe/aem-headless-client-js
```

Este ejemplo de código muestra cómo utilizar la variable [React useEffect(..) gancho](https://reactjs.org/docs/hooks-effect.html) para ejecutar una llamada asincrónica a AEM GraphQL.

Uso `useEffect` hacer que la llamada asincrónica de GraphQL en React sea útil porque:

1. Proporciona un envoltorio sincrónico para la llamada asincrónica a AEM.
1. Reduce las AEM innecesarias.

Este código asume una consulta persistente con el nombre `wknd-shared/adventure-by-slug` se ha creado en AEM Author y se ha publicado en AEM Publish mediante GraphiQL.

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

Invocar el React personalizado `useEffect` enganche desde otra parte de un componente React.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Nuevo `useEffect` se pueden crear vínculos para cada consulta persistente que utilice la aplicación React.

+++

<p> </p>

## Consultas de GraphQL

AEM admite consultas de GraphQL definidas por el cliente, pero AEM práctica recomendada utilizar [consultas de GraphQL persistentes](#persisted-graphql-queries).

