---
title: 'AEM Cree una aplicación de React que consulte las consultas mediante la API de GraphQL AEM: Introducción a la aplicación sin encabezado GraphQL'
description: Introducción a Adobe Experience Manager AEM () y GraphQL. AEM Cree una aplicación de React que recupere contenido/datos de la API de GraphQL AEM, y también vea cómo se utiliza el SDK de JS sin encabezado, que también se utiliza para crear contenido y datos de.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 678ecb99b1e63b9db6c9668adee774f33b2eefab
workflow-type: tm+mt
source-wordcount: '1188'
ht-degree: 2%

---


# AEM Creación de una aplicación de React que utilice API de GraphQL de la

AEM En este capítulo, explorará cómo las API de GraphQL pueden dirigir la experiencia de en una aplicación externa de.

Se utiliza una aplicación React simple para consultar y mostrar **Equipo** y **Persona** AEM contenido expuesto por las API de GraphQL de. El uso de React no es muy importante, y la aplicación externa de consumo podría escribirse en cualquier marco de trabajo para cualquier plataforma.

## Requisitos previos

Se supone que se han completado los pasos descritos en las partes anteriores de este tutorial de varias partes, o [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) AEM está instalado en los servicios de autor y publicación as a Cloud Service de la.

_Las capturas de pantalla IDE de este capítulo provienen de [Código de Visual Studio](https://code.visualstudio.com/)_

Se debe instalar el siguiente software:

- [Node.js v18](https://nodejs.org/en)
- [Código de Visual Studio](https://code.visualstudio.com/)

## Objetivos

Obtenga información sobre cómo:

- Descargue e inicie la aplicación React de ejemplo
- AEM Consulta de puntos finales de GraphQL mediante la variable [AEM SDK de JS sin encabezado](https://github.com/adobe/aem-headless-client-js)
- AEM Consulta de una lista de equipos y de los miembros a los que se hace referencia
- AEM Consulta de los detalles de un miembro del equipo

## Obtener la aplicación React de ejemplo

AEM En este capítulo, se implementa una aplicación React de ejemplo con el código necesario para interactuar con la API de GraphQL y mostrar los datos de equipo y persona obtenidos de ellas.

El código fuente de la aplicación React de ejemplo está disponible en Github.com en <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Para obtener la aplicación React:

1. Clone la aplicación WKND GraphQL React de muestra desde [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Vaya a `basic-tutorial` y ábrala en su IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![Aplicación React en VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Actualizar `.env.development` AEM para conectarse al servicio de publicación as a Cloud Service de la.

   - Establecer `REACT_APP_HOST_URI`AEM El valor de debe ser la URL de publicación del as a Cloud Service de la (por ejemplo, `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) y `REACT_APP_AUTH_METHOD`El valor de a `none`
   >[!NOTE]
   >
   > Asegúrese de haber publicado la configuración del proyecto, los modelos de fragmento de contenido, los fragmentos de contenido creados, los extremos de GraphQL y las consultas persistentes de pasos anteriores.
   >
   > Si ha realizado los pasos anteriores en el SDK local de AEM Author, puede apuntar a `http://localhost:4502` y `REACT_APP_AUTH_METHOD`El valor de a `basic`.


1. Desde la línea de comandos, vaya a `aem-guides-wknd-graphql/basic-tutorial` carpeta

1. Inicio de la aplicación React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. La aplicación React se inicia en modo de desarrollo el [http://localhost:3000/](http://localhost:3000/). Los cambios realizados en la aplicación React a lo largo del tutorial se reflejan inmediatamente.

![Aplicación React parcialmente implementada](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Esta aplicación de React se ha implementado parcialmente. Siga los pasos de este tutorial para completar la implementación. Los archivos JavaScript que necesitan implementación funcionan con el siguiente comentario. Asegúrese de agregar o actualizar el código de esos archivos con el código especificado en este tutorial.
>
>
> //*********************************
>
>  AEM // TODO :: Implemente esto siguiendo los pasos de Tutorial sin encabezado de
>
>  //*********************************

## Estructura de la aplicación React

La aplicación React de ejemplo tiene tres partes principales:

1. El `src/api` contiene archivos que se utilizan para hacer consultas de GraphQL AEM a los usuarios de la.
   - `src/api/aemHeadlessClient.js` AEM AEM inicializa y exporta el cliente sin encabezado de la que se utiliza para comunicarse con
   - `src/api/usePersistedQueries.js` implementa [Ganchos React personalizados](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components) AEM devolver datos de GraphQL a la `Teams.js` y `Person.js` ver componentes.

1. El `src/components/Teams.js` file muestra una lista de equipos y sus miembros mediante una consulta de lista.
1. El `src/components/Person.js` Este archivo muestra los detalles de una sola persona mediante una consulta parametrizada de un solo resultado.

## Revise el objeto AEMHeadless

Revise la `aemHeadlessClient.js` para obtener información sobre cómo crear el `AEMHeadless` AEM objeto utilizado para comunicarse con el usuario de la red de.

1. Abra `src/api/aemHeadlessClient.js`.

1. Revise las líneas 1-40:

   - La importación `AEMHeadless` declaración de la [AEM Cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js), línea 11.

   - La configuración de la autorización en función de las variables definidas en `.env.development`, línea 14-22 y, la expresión de función arrow `setAuthorization`, línea 31-40.

   - El `serviceUrl` configuración para el incluido [proxy de desarrollo](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) configuración, línea 27.

1. Las líneas 42-49 son las más importantes, ya que crean instancias de `AEMHeadless` y exportarlo para su uso en toda la aplicación React.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## AEM Implemente para ejecutar consultas persistentes de GraphQL de la

Para implementar el genérico `fetchPersistedQuery(..)` AEM para ejecutar las consultas persistentes de GraphQL de la abrir el `usePersistedQueries.js` archivo. El `fetchPersistedQuery(..)` utiliza la función `aemHeadlessClient` del objeto `runPersistedQuery()` para ejecutar query de forma asíncrona, comportamiento basado en promise.

Más adelante, React personalizado `useEffect` AEM El vínculo llama a esta función para recuperar datos específicos de los datos de la.

1. Entrada `src/api/usePersistedQueries.js` **actualizar** `fetchPersistedQuery(..)`, línea 35, con el código siguiente.

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

## Implementación de funcionalidad de equipos

A continuación, cree la funcionalidad para mostrar los equipos y sus miembros en la vista principal de la aplicación React. Esta funcionalidad requiere lo siguiente:

- Un nuevo [Gancho personalizado de React useEffect](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` que invoca el `my-project/all-teams` AEM consulta persistente, que devuelve una lista de fragmentos de contenido de equipo en el cuadro de diálogo de.
- Un componente de React en `src/components/Teams.js` que invoca el nuevo React personalizado `useEffect` y procesa los datos de los equipos.

AEM Una vez finalizada, la vista principal de la aplicación se rellena con los datos de los equipos de la vista de datos de la aplicación de los datos de la.

![Vista de equipos](./assets/graphql-and-external-app/react-app__teams-view.png)

### Etapas

1. Abra `src/api/usePersistedQueries.js`.

1. Busque la función `useAllTeams()`

1. Para crear un `useEffect` vínculo que invoca la consulta persistente `my-project/all-teams` mediante `fetchPersistedQuery(..)`, agregue el siguiente código. AEM El vínculo también solo devuelve los datos relevantes de la respuesta de GraphQL de la en `data?.teamList?.items`, lo que permite que los componentes de la vista React no sean agnósticos de las estructuras JSON principales.

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

1. En el `Teams` AEM Componente React, recupere la lista de equipos desde el que se utiliza el componente de. `useAllTeams()` gancho.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. Realice la validación de datos basada en vistas, mostrando un mensaje de error o un indicador de carga basado en los datos devueltos.

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

1. Finalmente, procese los datos de los equipos. Cada equipo devuelto desde la consulta de GraphQL se procesa usando el proporcionado `Team` Subcomponente React.

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


## Implementación de la funcionalidad Persona

Con el [Funcionalidad de equipos](#implement-teams-functionality) Una vez completada, vamos a implementar la funcionalidad para administrar la visualización de los detalles de un miembro del equipo o de una persona.

Esta funcionalidad requiere lo siguiente:

- Un nuevo [Gancho personalizado de React useEffect](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` que invoca el parámetro `my-project/person-by-name` consulta persistente y devuelve un registro de una sola persona.

- Un componente de React en `src/components/Person.js` que utiliza el nombre completo de una persona como parámetro de consulta, invoca el nuevo React personalizado `useEffect` y procesa los datos de la persona.

Una vez finalizado, al seleccionar el nombre de una persona en la vista Equipos, se procesa la vista de personas.

![Persona](./assets/graphql-and-external-app/react-app__person-view.png)

1. Abra `src/api/usePersistedQueries.js`.

1. Busque la función `usePersonByName(fullName)`

1. Para crear un `useEffect` vínculo que invoca la consulta persistente `my-project/all-teams` mediante `fetchPersistedQuery(..)`, agregue el siguiente código. AEM El vínculo también solo devuelve los datos relevantes de la respuesta de GraphQL de la en `data?.teamList?.items`, lo que permite que los componentes de la vista React no sean agnósticos de las estructuras JSON principales.

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
1. En el `Person` Componente React, analice `fullName` AEM Parámetro de ruta y obtenga los datos de la persona desde el que se usa el parámetro de rutaDeAccesoDeAccesoDeAccesoDeAccesoDeUsuario `usePersonByName(fullName)` gancho.

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

1. Realice la validación de datos basada en vistas, mostrando un mensaje de error o un indicador de carga basado en los datos devueltos.

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

1. Por último, procese los datos de persona.

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

Revise la aplicación [http://localhost:3000/](http://localhost:3000/) y haga clic en _Miembros_ Vínculos. AEM Además, puede añadir más equipos o miembros a Team Alpha añadiendo fragmentos de contenido en las listas de distribución de contenido de la página de la página de la página de la página de la página de la página de la página de la página de la página de.

## Bajo el capó

Abra el del explorador **Herramientas para desarrolladores** > **Red** y _Filtrar_ para `all-teams` solicitud. Observe la solicitud de API de GraphQL `/graphql/execute.json/my-project/all-teams` se crea en `http://localhost:3000` y **NO** frente al valor de `REACT_APP_HOST_URI` (por ejemplo, <https://publish-p123-e456.adobeaemcloud.com>). Las solicitudes se realizan en el dominio de la aplicación React porque [configuración de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) está habilitado mediante `http-proxy-middleware` módulo.


![Solicitud de API de GraphQL mediante proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Revisar el principal `../setupProxy.js` file y en `../proxy/setupProxy.auth.**.js` archivos ver cómo `/content` y `/graphql` las rutas se procesan como proxy e indican que no es un recurso estático.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

El uso del proxy local no es una opción adecuada para la implementación de producción y se pueden encontrar más detalles en _Implementación de producción_ sección.

## Enhorabuena.{#congratulations}

Felicitaciones. AEM ¡Ha creado correctamente la aplicación React para consumir y mostrar datos de las API de GraphQL de como parte del tutorial básico!
