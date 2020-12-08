---
title: AEM de consulta mediante GraphQL desde una aplicación externa - Introducción a AEM sin cabeza - GraphQL
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Explore las API de GraphQL AEM una aplicación de WKND GraphQL React de muestra. Descubra cómo esta aplicación externa hace llamadas de GraphQL a AEM para potenciar su experiencia. Obtenga información sobre cómo realizar la gestión básica de errores.
sub-product: activos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
translation-type: tm+mt
source-git-commit: 2ea667d3bdb73fa4da87b877f14db77d896448a7
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 0%

---


# AEM de consulta mediante GraphQL desde una aplicación externa

>[!CAUTION]
>
> La API de AEM GraphQL para el Envío de fragmentos de contenido se lanzará a principios de 2021.
> La documentación correspondiente está disponible para fines de previsualización.

En este capítulo analizamos cómo se pueden utilizar las API de GraphQL AEM para impulsar la experiencia en una aplicación externa.

Este tutorial utiliza una aplicación React simple para mostrar y consulta contenido de aventura expuesto por las API de AEM GraphQL. El uso de React es en gran medida poco importante, y el consumo de aplicaciones externas podría escribirse en cualquier marco para cualquier plataforma.

## Requisitos previos

Se trata de un tutorial en varias partes y se supone que se han completado los pasos descritos en las partes anteriores.

_Las capturas de pantalla IDE de este capítulo provienen del código de  [Visual Studio](https://code.visualstudio.com/)_

Opcionalmente, instale una extensión de explorador como [Red GraphQL](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm) para poder vista de más detalles sobre una consulta GraphQL.

## Objetivos

En este capítulo, aprenderemos a:

* Inicio y comprensión de la funcionalidad de la aplicación React de muestra
* Explore cómo se realizan llamadas desde la aplicación externa a los puntos finales de AEM GraphQL
* Definir una consulta de GraphQL para filtrar una lista de fragmentos de contenido de aventuras por actividad
* Actualice la aplicación React para proporcionar controles para filtrar mediante GraphQL, la lista de aventuras por actividad

## Inicio de la aplicación React

Dado que este capítulo se centra en el desarrollo de un cliente para que consuma fragmentos de contenido a través de GraphQL, el ejemplo de [código fuente de la aplicación WKND GraphQL React debe descargarse y configurarse](./setup.md#react-app) en el equipo local, y el [SDK de AEM se está ejecutando como el servicio Autor](./setup.md#aem-sdk) con el [sitio WKND de muestra instalado](./setup.md#wknd-site).

El inicio de la aplicación React se describe con más detalle en el capítulo [Configuración rápida](./setup.md), pero se pueden seguir las instrucciones abreviadas:

1. Si aún no lo ha hecho, clona la aplicación WKND GraphQL React de muestra de [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra la aplicación WKND GraphQL React en su IDE

   ![Reaccionar aplicación en VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Desde la línea de comandos, desplácese a la carpeta `react-app`
1. Inicio de la aplicación WKND GraphQL React, ejecutando el siguiente comando desde la raíz del proyecto (la carpeta `react-app`)

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Revise la aplicación en [http://localhost:3000/](http://localhost:3000/). La aplicación React de muestra consta de dos partes principales:

   * La experiencia doméstica actúa como un índice de las aventuras de WKND, pero consultando los fragmentos de contenido de __aventura__ en AEM usando GraphQL. En este capítulo, modificaremos esta vista para admitir el filtrado de aventuras por actividad.

      ![Aplicación WKND GraphQL React - Experiencia doméstica](./assets/graphql-and-external-app/react-home-view.png)

   * La experiencia de detalles de la aventura usa GraphQL para consulta del fragmento de contenido específico __Aventura__ y muestra más puntos de datos.

      ![Aplicación WKND GraphQL React: experiencia detallada](./assets/graphql-and-external-app/react-details-view.png)

1. Utilice las herramientas de desarrollo del explorador y una extensión del explorador como [Red GraphQL](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm) para inspeccionar las consultas GraphQL enviadas a AEM y sus respuestas JSON. Este método se puede utilizar para supervisar las solicitudes y respuestas de GraphQL a fin de asegurarse de que se formulan correctamente y de que sus respuestas son las esperadas.

   ![Consulta sin procesar para adventureList](assets/graphql-and-external-app/raw-query-chrome-extension.png)

   *Consulta de GraphQL enviada a AEM desde la aplicación React*

   ![Respuesta JSON de GraphQL](assets/graphql-and-external-app/graphql-json-response.png)

   *Respuesta JSON de AEM a la aplicación React*

   Las consultas y la respuesta deben coincidir con lo que se vio en el IDE de GraphiQL.

   >[!NOTE]
   >
   > Durante el desarrollo, la aplicación React se configura en solicitudes HTTP proxy a través del servidor de desarrollo de webpack para AEM. La aplicación React está realizando solicitudes a `http://localhost:3000`, que las envía al servicio AEM Author que se ejecuta en `http://localhost:4502`. Revise el archivo `src/setupProxy.js` y `env.development` para obtener más detalles.
   >
   > En escenarios que no son de desarrollo, la aplicación React se configuraría directamente para realizar solicitudes a AEM.

## Explore el código GraphQL de la aplicación

1. En su IDE, abra el archivo `src/api/useGraphQL.js`.

   Se trata de un [enlace de efecto de reacción](https://reactjs.org/docs/hooks-overview.html#effect-hook) que escucha los cambios en el `query` de la aplicación y, al cambiar, realiza una solicitud de POST HTTP al punto final de AEM GraphQL y devuelve la respuesta JSON a la aplicación.

   Cada vez que la aplicación React necesita hacer una consulta de GraphQL, invoca este `useGraphQL(query)` enlace personalizado, pasando el GraphQL para enviarlo a AEM.

   Este enlace utiliza el módulo simple `fetch` para realizar la solicitud de POST HTTP GraphQL, pero otros módulos como el cliente [Apollo GraphQL](https://www.apollographql.com/docs/react/) pueden utilizarse de manera similar.

1. Abra `src/components/Adventures.js` en el IDE, que es responsable de la lista de aventuras de la vista principal, y revise la invocación del enlace `useGraphQL`.

   Este código establece que el `query` predeterminado sea el `allAdventuresQuery` como se define más abajo en este archivo.

   ```javascript
   const [query, setQuery] = useState(allAdventuresQuery);
   ```

   ... y cada vez que cambia la variable `query`, se invoca el enlace `useGraphQL`, que a su vez ejecuta la consulta de GraphQL contra AEM, devolviendo el JSON a la variable `data`, que luego se utiliza para representar la lista de aventuras.

   ```javascript
   const { data, errorMessage } = useGraphQL(query);
   ```

   La `allAdventuresQuery` es una consulta GraphQL constante definida en el archivo, que consulta todos los fragmentos de contenido de aventura, sin ningún filtro, y devuelve sólo los puntos de datos necesarios para representar la vista principal.

   ```javascript
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
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
   `;
   ```

1. Abra `src/components/AdventureDetail.js`, el componente React responsable de mostrar la experiencia de detalles de aventura. Esta vista solicita un fragmento de contenido específico, utilizando su ruta JCR como ID única y representa los detalles proporcionados.

   De manera similar a `Adventures.js`, el enlace de reacción personalizado `useGraphQL` se reutiliza para realizar la consulta de GraphQL con AEM.

   La ruta del fragmento de contenido se recopila de la `props` parte superior del componente para la que se debe especificar el fragmento de contenido para el que se va a realizar la consulta.

   ```javascript
   const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
   ```

   ... y la consulta parametrizada de GraphQL se construye utilizando la función `adventureDetailQuery(..)` y se pasa a `useGraphQL(query)`, que ejecuta la consulta de GraphQL con AEM y devuelve los resultados a la variable `data`.

   ```javascript
   const { data, errorMessage } = useGraphQL(adventureDetailQuery(contentFragmentPath));
   ```

   La función `adventureDetailQuery(..)` simplemente envuelve una consulta GraphQL de filtrado, que utiliza AEM sintaxis `<modelName>ByPath` para la consulta de un único fragmento de contenido identificado por su ruta JCR, y devuelve todos los puntos de datos especificados necesarios para representar los detalles de la aventura.

   ```javascript
   function adventureDetailQuery(_path) {
   return `{
       adventureByPath (_path: "${_path}") {
         item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
         }
       }
   }
   `;
   }
   ```

## Creación de una consulta GraphQL parametrizada

A continuación, vamos a modificar la aplicación React para realizar consultas parametrizadas y de GraphQL que restringen la vista doméstica según la actividad de las aventuras.

1. En su IDE, abra el archivo: `src/components/Adventures.js`. Este archivo representa el componente de aventuras de la experiencia principal, que consulta y muestra las tarjetas de aventuras.
1. Inspect utiliza la función `filterQuery(activity)`, que no se utiliza, pero que está preparada para formular una consulta de GraphQL que filtros aventuras por `activity`.

   Observe que el parámetro `activity` se inyecta en la consulta GraphQL como parte de un `filter` en el campo `adventureActivity`, lo que requiere que el valor del campo coincida con el valor del parámetro.

   ```javascript
   function filterQuery(activity) {
       return `
           {
           adventures (filter: {
               adventureActivity: {
               _expressions: [
                   {
                   value: "${activity}"
                   }
                 ]
               }
           }){
               items {
               _path
               adventureTitle
               adventurePrice
               adventureTripLength
               adventurePrimaryImage {
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
       `;
   }
   ```

1. Actualice la sentencia `return` del componente React Adventures para agregar botones que invoquen el nuevo `filterQuery(activity)` parametrizado para proporcionar las aventuras a la lista.

   ```javascript
   function Adventures() {
       ...
       return (
           <div className="adventures">
   
           {/* Add these three new buttons that set the GraphQL query accordingly */}
   
           {/* The first button uses the default `allAdventuresQuery` */}
           <button onClick={() => setQuery(allAdventuresQuery)}>All</button>
   
           {/* The 2nd and 3rd button use the `filterQuery(..)` to filter by activity */}
           <button onClick={() => setQuery(filterQuery('Camping'))}>Camping</button>
           <button onClick={() => setQuery(filterQuery('Surfing'))}>Surfing</button>
   
           <ul className="adventure-items">
           ...
       )
   }
   ```

1. Guarde los cambios y vuelva a cargar la aplicación React en el navegador web. Los tres nuevos botones aparecen en la parte superior y, al hacer clic en ellos, se vuelven a poner en consulta automáticamente los AEM de fragmentos de contenido de aventura con la actividad correspondiente.

   ![Filtrar las aventuras por Actividad](./assets/graphql-and-external-app/filter-by-activity.png)

1. Intente agregar más botones de filtrado para las actividades: `Rock Climbing`, `Cycling` y `Skiing`

## Gestión de errores de GraphQL

GraphQL tiene un establecimiento inflexible de tipos y, por tanto, puede devolver mensajes de error útiles si la consulta no es válida. A continuación, simulemos una consulta incorrecta para ver el mensaje de error devuelto.

1. Vuelva a abrir el archivo `src/api/useGraphQL.js`. Inspect el siguiente fragmento de código para ver la gestión de errores:

   ```javascript
   //useGraphQL.js
   .then(({data, errors}) => {
           //If there are errors in the response set the error message
           if(errors) {
               setErrors(mapErrors(errors));
           }
           //Otherwise if data in the response set the data as the results
           if(data) {
               setData(data);
           }
       })
       .catch((error) => {
           setErrors(error);
       });
   ```

   La respuesta se inspecciona para ver si incluye un objeto `errors`. El objeto `errors` será enviado por AEM si hay problemas con la consulta GraphQL, como un campo no definido basado en el esquema. Si no hay ningún objeto `errors`, se establece `data` y se devuelve.

   El `window.fetch` incluye una sentencia `.catch` para *catch* cualquier error común como una solicitud HTTP no válida o si no se puede realizar la conexión con el servidor.

1. Abra el archivo `src/components/Adventures.js`.
1. Modifique `allAdventuresQuery` para incluir una propiedad no válida `adventurePetPolicy`:

   ```javascript
   /**
    * Query for all Adventures
    * adventurePetPolicy has been added beneath items
   */
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           adventurePetPolicy
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
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
   `;
   ```

   Sabemos que `adventurePetPolicy` no es parte del modelo de aventura, por lo que esto debería generar un error.

1. Guarde los cambios y vuelva al explorador. Debería ver un mensaje de error como el siguiente:

   ![Error de propiedad no válida](assets/graphql-and-external-app/invalidProperty.png)

   La API de GraphQL detecta que `adventurePetPolicy` no está definida en `AdventureModel` y devuelve un mensaje de error adecuado.

1. Inspect muestra la respuesta de AEM utilizando las herramientas de desarrollador del explorador para ver el objeto `errors` JSON:

   ![Errors JSON (objeto)](assets/graphql-and-external-app/error-json-response.png)

   El objeto `errors` es detallado e incluye información sobre la ubicación de la consulta mal formada y la clasificación del error.

1. Vuelva a `Adventures.js` y revierta el cambio de consulta para devolver la aplicación a su estado correcto.

## Felicitaciones!{#congratulations}

Felicitaciones! Ha explorado con éxito el código de la aplicación WKND GraphQL React de muestra y lo ha actualizado para que utilice parametrizados y filtre las consultas de GraphQL a las aventuras de lista por actividad. También tuvo la oportunidad de explorar la administración básica de errores.

## Próximos pasos {#next-steps}

En el siguiente capítulo, [Modelado de datos avanzado con Referencias de fragmento](./fragment-references.md) aprenderá a utilizar la función Referencia de fragmento para crear una relación entre dos fragmentos de contenido diferentes. También aprenderá a modificar una consulta de GraphQL para incluir un campo de un modelo al que se hace referencia.
