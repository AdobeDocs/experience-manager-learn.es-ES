---
title: 'AEM Cree una aplicación de React que consulte las consultas mediante la API de GraphQL AEM: Introducción a la aplicación sin encabezado GraphQL'
description: Introducción a Adobe Experience Manager AEM () y GraphQL. AEM Cree una aplicación de React que recupere contenido/datos de la API de GraphQL AEM, y también vea cómo se utiliza el SDK de JS sin encabezado, que también se utiliza para crear contenido y datos de.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 410
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1181'
ht-degree: 0%

---


# AEM Crear una aplicación de React que use las API de GraphQL para la creación de

AEM En este capítulo, se explica cómo las API de GraphQL de pueden impulsar la experiencia en una aplicación externa.

AEM Se usa una aplicación React simple para consultar y mostrar el contenido de **Equipo** y **Persona** expuesto por las API de GraphQL que se están utilizando para la creación de informes de la aplicación. El uso de React no es muy importante, y la aplicación externa de consumo podría escribirse en cualquier marco de trabajo para cualquier plataforma.

## Requisitos previos

Se da por hecho que se han completado los pasos descritos en las partes anteriores de este tutorial de varias partes o que [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip) está instalado en los servicios de AEM as a Cloud Service Author y Publish.

_Las capturas de pantalla IDE de este capítulo provienen de [Visual Studio Code](https://code.visualstudio.com/)_

Se debe instalar el siguiente software:

- [Node.js v18](https://nodejs.org/en)
- [Código de Visual Studio](https://code.visualstudio.com/)

## Objetivos

Obtenga información sobre cómo:

- Descargue e inicie la aplicación React de ejemplo
- AEM Consulta de puntos finales de GraphQL AEM que utilizan el [SDK de JS sin encabezado ](https://github.com/adobe/aem-headless-client-js)
- AEM Consulta de una lista de equipos y de los miembros a los que se hace referencia
- AEM Consulta de los detalles de un miembro del equipo

## Obtener la aplicación React de ejemplo

AEM En este capítulo, se implementa una aplicación React de ejemplo con el código necesario para interactuar con la API de GraphQL de la aplicación y mostrar los datos de equipo y persona obtenidos de ellas.

El código fuente de la aplicación React de ejemplo está disponible en Github.com en <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Para obtener la aplicación React:

1. Clona la aplicación WKND GraphQL React de ejemplo de [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Vaya a la carpeta `basic-tutorial` y ábrala en su IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![Aplicación React en VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Actualice `.env.development` para conectarse al servicio AEM as a Cloud Service Publish.

   - Establezca el valor de `REACT_APP_HOST_URI` como la URL de Publish de su AEM as a Cloud Service (por ejemplo, `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) y el valor de `REACT_APP_AUTH_METHOD` a `none`

   >[!NOTE]
   >
   > Asegúrese de haber publicado la configuración del proyecto, los modelos de fragmento de contenido, los fragmentos de contenido creados, los extremos de GraphQL y las consultas persistentes de pasos anteriores.
   >
   > AEM Si ha realizado los pasos anteriores en el SDK local de Autor de, puede señalar a `http://localhost:4502` y el valor de `REACT_APP_AUTH_METHOD` a `basic`.


1. Desde la línea de comandos, vaya a la carpeta `aem-guides-wknd-graphql/basic-tutorial`

1. Inicio de la aplicación React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. La aplicación React se inicia en modo de desarrollo en [http://localhost:3000/](http://localhost:3000/). Los cambios realizados en la aplicación React a lo largo del tutorial se reflejan inmediatamente.

![Aplicación React implementada parcialmente](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Esta aplicación de React se ha implementado parcialmente. Siga los pasos de este tutorial para completar la implementación. Los archivos JavaScript que necesitan implementación funcionan con el siguiente comentario. Asegúrese de agregar o actualizar el código de esos archivos con el código especificado en este tutorial.
>
>
> //*********************************
>
>  AEM // TODO Implemente esto siguiendo los pasos de Tutorial sin encabezado de
>
>  //*********************************
>

## Estructura de la aplicación React

La aplicación React de ejemplo tiene tres partes principales:

1. La carpeta `src/api` contiene archivos utilizados para hacer consultas de GraphQL AEM a los que se va a realizar una.
   - AEM AEM `src/api/aemHeadlessClient.js` inicializa y exporta el cliente sin encabezado de la que se utiliza para comunicarse con el usuario de la red de área de trabajo
   - AEM `src/api/usePersistedQueries.js` implementa [los enlaces personalizados de React](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components) devuelven datos de GraphQL de la aplicación de la vista de datos de la aplicación de datos de la aplicación de datos de la aplicación de datos de la aplicación de datos de `Teams.js` y `Person.js`.

1. El archivo `src/components/Teams.js` muestra una lista de equipos y sus miembros mediante una consulta de lista.
1. El archivo `src/components/Person.js` muestra los detalles de una sola persona mediante una consulta parametrizada de un solo resultado.

## Revise el objeto AEMHeadless

AEM Revise el archivo `aemHeadlessClient.js` para ver cómo crear el objeto `AEMHeadless` usado para comunicarse con el usuario de la red de la red de la red de la red de la red de la red de área de trabajo.

1. Abra `src/api/aemHeadlessClient.js`.

1. Revise las líneas 1-40:

   - AEM La declaración `AEMHeadless` de importación del cliente sin encabezado [para JavaScript](https://github.com/adobe/aem-headless-client-js), línea 11.

   - La configuración de la autorización se basa en las variables definidas en `.env.development`, línea 14-22, y, la expresión de función de flecha `setAuthorization`, línea 31-40.

   - La configuración de `serviceUrl` para la configuración de [proxy de desarrollo](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) incluida, línea 27.

1. Las líneas 42-49 son las más importantes, ya que crean una instancia del cliente `AEMHeadless` y lo exportan para su uso en toda la aplicación React.

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

AEM Para implementar la función `fetchPersistedQuery(..)` genérica con el fin de ejecutar las consultas persistentes de GraphQL de la, abra el archivo `usePersistedQueries.js`. La función `fetchPersistedQuery(..)` utiliza la función `runPersistedQuery()` del objeto `aemHeadlessClient` para ejecutar el comportamiento basado en promesas de forma asincrónica.

AEM Más adelante, el vínculo React `useEffect` personalizado llama a esta función para recuperar datos específicos de las llamadas de la función de datos de la cuenta de usuario de la cuenta de usuario de.

1. En `src/api/usePersistedQueries.js` **actualizar** `fetchPersistedQuery(..)`, línea 35, con el código siguiente.

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

- AEM Un nuevo [vínculo personalizado de React useEffect](https://react.dev/reference/react/useEffect#useeffect) en `src/api/usePersistedQueries.js` que invoca la consulta persistente `my-project/all-teams`, devuelve una lista de fragmentos de contenido de equipo en la que se ha realizado un seguimiento de tiempo de ejecución de la.
- Componente de React en `src/components/Teams.js` que invoca el nuevo vínculo personalizado de React `useEffect` y procesa los datos de los equipos.

AEM Una vez finalizada, la vista principal de la aplicación se rellena con los datos de los equipos de la vista de datos de la aplicación de los datos de la.

![Vista de equipos](./assets/graphql-and-external-app/react-app__teams-view.png)

### Etapas

1. Abra `src/api/usePersistedQueries.js`.

1. Busque la función `useAllTeams()`

1. Para crear un vínculo `useEffect` que invoque la consulta persistente `my-project/all-teams` a través de `fetchPersistedQuery(..)`, agregue el siguiente código. AEM El vínculo también devuelve únicamente los datos relevantes de la respuesta de GraphQL de la en `data?.teamList?.items`, lo que permite que los componentes de la vista React no sean agnósticos a las estructuras JSON principales.

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

1. Abrir `src/components/Teams.js`

1. AEM En el componente React `Teams`, recupere la lista de equipos de los que se ha hecho uso del vínculo `useAllTeams()`.

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

1. Finalmente, procese los datos de los equipos. Cada equipo devuelto desde la consulta de GraphQL se procesa usando el subcomponente React `Team` proporcionado.

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

Una vez completada la funcionalidad [Equipos](#implement-teams-functionality), implementemos la funcionalidad para administrar la visualización de los detalles de un miembro del equipo o de una persona.

Esta funcionalidad requiere lo siguiente:

- Un nuevo [vínculo personalizado de React useEffect](https://react.dev/reference/react/useEffect#useeffect) en `src/api/usePersistedQueries.js` que invoca la consulta persistente `my-project/person-by-name` parametrizada y devuelve un registro de una sola persona.

- Un componente React de `src/components/Person.js` que usa el nombre completo de una persona como parámetro de consulta, invoca el nuevo vínculo React `useEffect` personalizado y procesa los datos de la persona.

Una vez finalizado, al seleccionar el nombre de una persona en la vista Equipos, se procesa la vista de personas.

![Persona](./assets/graphql-and-external-app/react-app__person-view.png)

1. Abra `src/api/usePersistedQueries.js`.

1. Busque la función `usePersonByName(fullName)`

1. Para crear un vínculo `useEffect` que invoque la consulta persistente `my-project/all-teams` a través de `fetchPersistedQuery(..)`, agregue el siguiente código. AEM El vínculo también devuelve únicamente los datos relevantes de la respuesta de GraphQL de la en `data?.teamList?.items`, lo que permite que los componentes de la vista React no sean agnósticos a las estructuras JSON principales.

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

1. Abrir `src/components/Person.js`
1. AEM En el componente React `Person`, analice el parámetro de ruta `fullName` y recupere los datos de la persona mediante el vínculo `usePersonByName(fullName)`.

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

Revise la aplicación [http://localhost:3000/](http://localhost:3000/) y haga clic en los vínculos _Miembros_. Además, puede añadir más equipos o miembros al Alpha AEM de equipos añadiendo fragmentos de contenido en la lista de fragmentos de contenido en la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de.

>[!IMPORTANT]
>
>Para comprobar los cambios de implementación o si no puede hacer que la aplicación funcione después de los cambios anteriores, consulte la rama de solución [basic-tutorial](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial).

## Bajo el capó

Abra **Herramientas para desarrolladores** > **Red** y _Filtro_ del explorador para la solicitud `all-teams`. Observe que la solicitud de API de GraphQL `/graphql/execute.json/my-project/all-teams` se realizó con `http://localhost:3000` y **NO** con el valor de `REACT_APP_HOST_URI`, por ejemplo `<https://publish-pxxx-exxx.adobeaemcloud.com`. Las solicitudes se realizan en el dominio de la aplicación React porque [proxy setup](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) está habilitado mediante el módulo `http-proxy-middleware`.


![Solicitud de API de GraphQL mediante proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Revise el archivo principal `../setupProxy.js` y, en `../proxy/setupProxy.auth.**.js` archivos, observe cómo se procesan como proxy las rutas de acceso `/content` y `/graphql` e indique que no es un recurso estático.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

El uso del proxy local no es una opción adecuada para la implementación de producción y se pueden encontrar más detalles en la sección _Implementación de producción_.

## Enhorabuena.{#congratulations}

Enhorabuena. AEM ¡Ha creado correctamente la aplicación React para consumir y mostrar datos de las API de GraphQL de la como parte del tutorial básico!
