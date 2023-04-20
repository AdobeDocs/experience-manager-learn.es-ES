---
title: Cómo trabajar con grandes conjuntos de resultados en AEM sin encabezado
description: Aprenda a trabajar con grandes conjuntos de resultados con AEM sin encabezado.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
source-git-commit: 9eb706e49f12a3ebd5222e733f540db4cf2c8748
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 1%

---


# Grandes conjuntos de resultados en AEM sin encabezado

AEM consultas de GraphQL sin encabezado pueden devolver grandes resultados. Este artículo describe cómo trabajar con resultados grandes en AEM sin encabezado para garantizar el mejor rendimiento para su aplicación.

AEM Headless admite un [offset/limit](#list-query) y [paginación basada en el cursor](#paginated-query) consulta a subconjuntos más pequeños de un conjunto de resultados más grande. Se pueden realizar varias solicitudes para recopilar tantos resultados como sea necesario.

Los ejemplos siguientes utilizan pequeños subconjuntos de resultados (cuatro registros por solicitud) para demostrar las técnicas. En una aplicación real, se utilizaría un mayor número de registros por solicitud para mejorar el rendimiento. 50 registros por solicitud es una buena línea de base.

## Modelo de fragmento de contenido

La paginación y la ordenación se pueden usar con cualquier modelo de fragmento de contenido.

## Consultas persistentes de GraphQL

Cuando se trabaja con conjuntos de datos grandes, se puede utilizar la paginación tanto offset como limitada y basada en el cursor para recuperar un subconjunto específico de los datos. Sin embargo, hay algunas diferencias entre las dos técnicas que pueden hacer que una sea más apropiada que la otra en determinadas situaciones.

### Desplazamiento/límite

Enumerar consultas, usar `limit` y `offset` proporcione un enfoque directo que especifique el punto de inicio (`offset`) y el número de registros que se van a recuperar (`limit`). Este método permite seleccionar un subconjunto de resultados desde cualquier lugar dentro del conjunto de resultados completo, como saltar a una página específica de resultados. Aunque es fácil de implementar, puede ser lento e ineficiente cuando se trata de resultados grandes, ya que la recuperación de muchos registros requiere el análisis a través de todos los registros anteriores. Este método también puede provocar problemas de rendimiento cuando el valor de desplazamiento es alto, ya que puede requerir la recuperación y la eliminación de muchos resultados.

#### Consulta de GraphQL

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### Variables de consulta

```json
{
  "offset": 1,
  "limit": 4
}
```

#### Respuesta de GraphQL

La respuesta JSON resultante contiene la segunda, tercera, cuarta y quinta aventuras más caras. Las dos primeras aventuras de los resultados tienen el mismo precio (`4500` de modo que [consulta de lista](#list-queries) especifica las aventuras con el mismo precio y luego se ordena por título en orden ascendente).

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### Consulta paginada

La paginación basada en el cursor, disponible en consultas paginadas, implica el uso de un cursor (una referencia a un registro específico) para recuperar el siguiente conjunto de resultados. Este método es más eficaz, ya que evita la necesidad de analizar todos los registros anteriores para recuperar el subconjunto de datos requerido. Las consultas paginadas son buenas para iterar a través de grandes conjuntos de resultados desde el principio, hasta algún punto en el medio o hasta el final. Enumerar consultas, usar `limit` y `offset` proporcione un enfoque directo que especifique el punto de inicio (`offset`) y el número de registros que se van a recuperar (`limit`). Este método permite seleccionar un subconjunto de resultados desde cualquier lugar dentro del conjunto de resultados completo, como saltar a una página específica de resultados. Aunque es fácil de implementar, puede ser lento e ineficiente cuando se trata de resultados grandes, ya que la recuperación de muchos registros requiere el análisis a través de todos los registros anteriores. Este método también puede provocar problemas de rendimiento cuando el valor de desplazamiento es alto, ya que puede requerir la recuperación y la eliminación de muchos resultados.

#### Consulta de GraphQL

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### Variables de consulta

```json
{
  "first": 3
}
```

#### Respuesta de GraphQL

La respuesta JSON resultante contiene la segunda, tercera, cuarta y quinta aventuras más caras. Las dos primeras aventuras de los resultados tienen el mismo precio (`4500` de modo que [consulta de lista](#list-queries) especifica las aventuras con el mismo precio y luego se ordena por título en orden ascendente).

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### Siguiente conjunto de resultados paginados

El siguiente conjunto de resultados se puede obtener utilizando la variable `after` y `endCursor` de la consulta anterior. Si no hay más resultados que recuperar, `hasNextPage` es `false`.

##### Variables de consulta

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## Ejemplos de reacción

Los siguientes son ejemplos de React que muestran cómo usar [desplazamiento y límite](#offset-and-limit) y [paginación basada en el cursor](#cursor-based-pagination) enfoques. Normalmente, el número de resultados por solicitud es mayor; sin embargo, a los efectos de estos ejemplos, el límite se establece en 5.

### Ejemplo de desvío y límite

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

Con el desplazamiento y el límite, los subconjuntos de resultados se pueden recuperar y mostrar fácilmente.

#### vínculo useEffect

La variable `useEffect` El enlace invoca una consulta persistente (`adventures-by-offset-and-limit`) que recupera una lista de aventuras. La consulta utiliza la variable `offset` y `limit` para especificar el punto de inicio y el número de resultados que se van a recuperar. La variable `useEffect` el enlace se invoca cuando la variable `page` cambia de valor.


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### Componente

El componente utiliza la variable `useOffsetLimitAdventures` para recuperar una lista de aventuras. La variable `page` se incrementa y se reduce para obtener el conjunto de resultados siguiente y anterior. La variable `hasMore` se utiliza para determinar si el botón de página siguiente debe estar habilitado.

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### Ejemplo paginado

![Ejemplo paginado](./assets/large-results/paginated-example.png)

_Cada cuadro rojo representa una consulta HTTP GraphQL paginada discreta._

Mediante la paginación basada en el cursor, se pueden recuperar y mostrar fácilmente grandes conjuntos de resultados, recopilando gradualmente los resultados y concatenándolos a los resultados existentes.


#### Enlace UseEffect

La variable `useEffect` El enlace invoca una consulta persistente (`adventures-by-paginated`) que recupera una lista de aventuras. La consulta utiliza la variable `first` y `after` para especificar el número de resultados que se recuperarán y el cursor desde el que se iniciará. `fetchData` bucles continuos, recopilando el siguiente conjunto de resultados paginados, hasta que no haya más resultados que recuperar.

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### Componente

El componente utiliza la variable `usePaginatedAdventures` para recuperar una lista de aventuras. La variable `queryCount` se utiliza para mostrar el número de solicitudes HTTP realizadas para recuperar la lista de aventuras.

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
