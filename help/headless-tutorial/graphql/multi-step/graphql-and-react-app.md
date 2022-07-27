---
title: 'Creación de una aplicación React que AEM Consultas con la API de GraphQL: Introducción a AEM sin encabezado: GraphQL'
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Cree una aplicación React que obtenga contenido/datos de AEM API de GraphQL, y también vea cómo se utiliza AEM SDK de JS sin encabezado.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 294ad688b17a5fc9559fea39fc99ebf5e95cad39
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 2%

---


# Cree una aplicación React que aproveche AEM API de GraphQL

En este capítulo se explica cómo AEM API de GraphQL pueden impulsar la experiencia en una aplicación externa.

Se utiliza una aplicación React simple para consultar y mostrar **Equipo** y **Persona** contenido expuesto por las API de AEM GraphQL. El uso de React no es muy importante, y la consumidora aplicación externa podría ser escrita en cualquier marco para cualquier plataforma.

## Requisitos previos

Se da por hecho que los pasos descritos en las partes anteriores de este tutorial en varias partes se han completado o [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) está instalado en los servicios AEM as a Cloud Service Author y Publish.

_Las capturas de pantalla IDE de este capítulo provienen de [Código de Visual Studio](https://code.visualstudio.com/)_

Se debe instalar el siguiente software:

- [Node.js v12+](https://nodejs.org/en/)
- [npm 6.4+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- [Código de Visual Studio](https://code.visualstudio.com/)

## Objetivos

Aprenda a:

- Descargue e inicie la aplicación React de ejemplo
- Consulte AEM extremos de GraphQL usando la variable [SDK de JS AEM sin encabezado](https://github.com/adobe/aem-headless-client-js)
- Consulta AEM una lista de equipos y sus miembros a los que se hace referencia
- AEM de consulta de los detalles de un miembro del equipo

## Obtenga la aplicación React de ejemplo

En este capítulo, se ha implementado una aplicación React de muestra de código combinado con el código necesario para interactuar con AEM API de GraphQL y mostrar los datos de equipo y persona obtenidos de ellas.

El código fuente de la aplicación React de muestra está disponible en Github.com en:

- https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial

Para obtener la aplicación React:

1. Clonar la aplicación WKND GraphQL React de ejemplo de [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone --branch git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Vaya a `basic-tutorial` y ábrala en su IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![React App in VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Actualizar `.env.development` para conectarse a AEM servicio de publicación as a Cloud Service.

   - Establezca `REACT_APP_HOST_URI`para que sea la URL de publicación de su AEM as a Cloud Service (por ejemplo, `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) y `REACT_APP_AUTH_METHOD`del valor a `none`
   >[!NOTE]
   >
   > Asegúrese de haber publicado la configuración del proyecto, los modelos de fragmento de contenido, los fragmentos de contenido creados, los extremos de GraphQL y las consultas persistentes de pasos anteriores. O si ha realizado los pasos anteriores en el SDK de AEM local, puede señalar a `http://localhost:4502` y `REACT_APP_AUTH_METHOD`del valor a `basic`.


1. Desde la línea de comandos, vaya a la `aem-guides-wknd-graphql/basic-tutorial` carpeta
1. Inicio de la aplicación React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. La aplicación React se inicia en el modo de desarrollo en [http://localhost:3000/](http://localhost:3000/). Los cambios realizados en la aplicación React a lo largo del tutorial se reflejarán automáticamente.

## Anatomía de la aplicación React

La aplicación React de ejemplo tiene tres partes principales:

1. `src/api` contiene archivos utilizados para realizar consultas de GraphQL a AEM.
   - `src/api/aemHeadlessClient.js` inicializa y exporta el cliente AEM sin encabezado utilizado para comunicarse con AEM
   - `src/api/usePersistedQueries.js` implementaciones [vínculos React personalizados](https://reactjs.org/docs/hooks-custom.html) devolver datos de AEM GraphQL a la variable `Teams.js` y `Person.js` ver componentes.

1. `src/components/Teams.js` muestra una lista de equipos y sus miembros mediante una consulta de lista.
1. `src/components/Person.js` muestra los detalles de una sola persona mediante una consulta parametrizada de un solo resultado.

## Crear el cliente sin encabezado de AEMH

Consulte `aemHeadlessClient.js` para saber cómo inicializar el cliente AEM sin encabezado utilizado para comunicarse con AEM.

1. Abra `src/api/aemHeadlessClient.js`.
1. Líneas 1-40, importación `AEMHeadless` desde el SDK, autorización de configuración basada en variables definidas en `.env.development`y configure la variable `serviceUrl` para el [proxy de desarrollo](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) configuración.
1. Las líneas 42-49 son más importantes, ya que crean instancias del cliente AEMHeadless y lo exportan para utilizarlo en toda la aplicación React.

   ```javascript
   // Initialize the AEM Headless Client and export it for other files to use
   const aemHeadlessClient = new AEMHeadless({
     serviceURL: serviceURL,
     endpoint: REACT_APP_GRAPHQL_ENDPOINT,
     auth: setAuthorization(),
   });
   
   export default aemHeadlessClient;
   ```

## Conexión a AEM extremos de GraphQL

Apertura `usePersistedQueries.js` para implementar el genérico `fetchPersistedQuery(..)` para conectarse a AEM extremos de GraphQL. `fetchPersistedQuery(..)` invoca la variable `aemHeadlessClient` exportado desde `aemHeadlessClient.js`.

Posteriormente, los enlaces React useEffect personalizados llaman a esta función para recuperar datos específicos de AEM.

1. En `src/api/usePersistedQueries.js` actualizar `fetchPersistedQuery(..)` con el siguiente código.

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
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

  // Return the GraphQL and any errors
  return { data, err };
}
```

## Implementación de la funcionalidad Equipos

A continuación, cree la funcionalidad para mostrar los equipos y sus miembros en la vista principal de la aplicación React. Esta funcionalidad requiere:

- Un nuevo [vínculo useEffect personalizado de React](https://reactjs.org/docs/hooks-custom.html) en `src/api/usePersistedQueries.js` que invoca la variable `my-project/all-teams` consulta persistente, devuelve una lista de fragmentos de contenido de equipo en AEM.
- Un componente React en `src/components/Teams.js` que invoca el nuevo vínculo personalizado React useEffect y procesa los datos de los equipos.

Una vez finalizada, la vista principal de la aplicación se rellena con los datos de los equipos de AEM.

![Vista Equipos](./assets/graphql-and-external-app/react-app__teams-view.png)

### Etapas

1. Abra `src/api/usePersistedQueries.js`.
1. Localizar la función `useAllTeams()`
1. Para crear un `useEffect` vínculo que invoca la consulta persistente `my-project/all-teams` via `fetchPersistedQuery(..)`, añada el siguiente código. El vínculo también solo devuelve los datos relevantes de la respuesta de AEM GraphQL en `data?.teamList?.items`, lo que permite que los componentes de la vista React sean independientes de las estructuras JSON principales.

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. Abra `src/components/Teams.js`
1. En el `Teams` Reaccione el componente, busque la lista de equipos de AEM usando la variable `useAllTeams()` gancho.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```

1. Realice una validación de datos basada en la vista, mostrando un mensaje de error o un indicador de carga basado en los datos devueltos.

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. Finalmente, represente los datos de los equipos. Cada equipo devuelto desde la consulta de GraphQL se procesa mediante el `Team` Reaccione el subcomponente.

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## Implementar la funcionalidad de persona

Con la variable [Funcionalidad de equipos](#implement-teams-functionality) complete, vamos a implementar la funcionalidad para administrar la visualización en los detalles de un miembro del equipo o de una persona.

Esta funcionalidad requiere:

- Un nuevo [vínculo useEffect personalizado de React](https://reactjs.org/docs/hooks-custom.html) en `src/api/usePersistedQueries.js` que invoca el parámetro `my-project/person-by-name` consulta persistente y devuelve un registro de persona única.

- Un componente React en `src/components/Person.js` que utiliza el nombre completo de una persona como parámetro de consulta, invoca el nuevo vínculo react useEffect personalizado y procesa los datos de la persona.

Una vez finalizado, si selecciona el nombre de una persona en la vista Equipos, se muestra la vista de personas.

![Persona](./assets/graphql-and-external-app/react-app__person-view.png)

1. Abra `src/api/usePersistedQueries.js`.
1. Localizar la función `usePersonByName(fullName)`
1. Para crear un `useEffect` vínculo que invoca la consulta persistente `my-project/all-teams` via `fetchPersistedQuery(..)`, añada el siguiente código. El vínculo también solo devuelve los datos relevantes de la respuesta de AEM GraphQL en `data?.teamList?.items`, lo que permite que los componentes de la vista React sean independientes de las estructuras JSON principales.

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. Abra `src/components/Person.js`
1. En el `Person` Reaccione, analice el `fullName` enrute el parámetro y recupere los datos de persona de AEM usando la variable `usePersonByName(fullName)` gancho.

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. Realice una validación de datos basada en la vista, mostrando un mensaje de error o un indicador de carga basado en los datos devueltos.

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. Finalmente, represente los datos de la persona.

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## Pruebe la aplicación

Revisar la aplicación [http://localhost:3000/](http://localhost:3000/) y haga clic en _Miembros_ vínculos. Además, puede agregar más equipos y/o miembros al Team Alpha.

## Debajo Del Capó

Si abre el **Herramientas para desarrolladores** > **Red** y *Filtro* para `all-teams` solicitud, verá la solicitud de API de GraphQL `/graphql/execute.json/my-project/all-teams` se hizo contra `http://localhost:3000` y **NOT** frente al valor de `REACT_APP_HOST_URI`(por ejemplo, https://publish-p123-e456.adobeaemcloud.com), esto se debe a que [configuración de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware` módulo.


![Solicitud de API de GraphQL mediante proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Revise el `../setupProxy.js` archivo y en `../proxy/setupProxy.auth.**.js` los archivos observan cómo `/content` y `/graphql` las rutas son sustituidas por proxy e indican que no es un recurso estático.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

Sin embargo, esta no es una opción adecuada para **implementación de producción** pero funciona bien durante el desarrollo, y se pueden encontrar más detalles en [_Implementación_](../deployment/spa.md) para obtener más información.

## Felicitaciones!{#congratulations}

Felicitaciones! Como parte del tutorial básico, ha creado correctamente la aplicación React para consumir y mostrar datos de AEM API de GraphQL.
