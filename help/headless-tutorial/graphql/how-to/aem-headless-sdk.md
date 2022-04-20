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
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# SDK AEM sin encabezado

El SDK sin encabezado de AEM es un conjunto de bibliotecas que los clientes pueden utilizar para interactuar rápida y fácilmente con AEM API sin encabezado a través de HTTP.

El SDK sin AEM está disponible para varias plataformas:

+ [AEM SDK sin encabezado para exploradores del lado del cliente (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM SDK sin encabezado para server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM SDK sin encabezado para Java™](https://github.com/adobe/aem-headless-client-java)

## Consultas de GraphQL

Realización de consultas AEM usando GraphQL mediante consultas (en lugar de [consultas de GraphQL persistentes](#persisted-graphql-queries)) permite a los desarrolladores definir consultas en el código, especificando exactamente qué contenido solicitar de AEM.

Las consultas de GraphQL tienden a tener un menor rendimiento que las consultas persistentes, ya que se ejecutan mediante el POST HTTP, que es menos caché en los niveles de CDN y AEM Dispatcher.

### Ejemplos de código{#graphql-queries-code-examples}

A continuación se indican algunos ejemplos de código de cómo ejecutar una consulta de GraphQL con AEM.

+++ Ejemplo de JavaScript

Instale el [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) ejecutando `npm install` desde la raíz del proyecto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Este ejemplo de código muestra cómo consultar AEM usando la variable [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) módulo npm usando `async/await` sintaxis. El SDK sin AEM para JavaScript también es compatible [Sintaxis de la promesa](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
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

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

Importe y utilice el `useGraphQL` enlace en el componente React para consultar AEM.

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## Consultas persistentes de GraphQL

Realización de consultas AEM mediante GraphQL con consultas persistentes (en oposición a [consultas regulares de GraphQL](#graphl-queries)) permite a los desarrolladores mantener una consulta (pero no sus resultados) en AEM y luego solicitar que la consulta se ejecute con el nombre. Las consultas persistentes son similares al concepto de procedimientos almacenados en bases de datos SQL.

Las consultas persistentes tienden a ser más eficaces que las consultas regulares de GraphQL, ya que las consultas persistentes se ejecutan mediante GET HTTP, que es más caché en los niveles de CDN y AEM Dispatcher. Las consultas persistentes también están en vigor, definen una API y desvinculan la necesidad de que el desarrollador entienda los detalles de cada modelo de fragmento de contenido.

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
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

Este código asume una consulta persistente con el nombre `wknd/adventureNames` se ha creado en AEM Author y se ha publicado en AEM Publish.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

E invoque este código desde cualquier otra parte del código de React.

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
