---
title: Instrumentación de la aplicación de React para editar contenido mediante el editor universal
description: Aprenda a instrumentar la aplicación de React para editar el contenido mediante el editor universal.
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 421
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 2a25cd44-cbd1-465e-ae3f-d3876e915114
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 100%

---

# Instrumentación de la aplicación de React para editar contenido mediante el editor universal

Aprenda a instrumentar la aplicación de React para editar el contenido mediante el editor universal.

## Requisitos previos

Ha configurado el entorno de desarrollo local como se describe en el paso anterior [Configuración de desarrollo local](./local-development-setup.md).

## Inclusión de la biblioteca principal del editor universal.

Empecemos por incluir la biblioteca principal del editor universal en la aplicación WKND Teams de React. Es una biblioteca de JavaScript que proporciona la capa de comunicación entre la aplicación editada y el editor universal.

Existen dos formas de incluir la biblioteca principal del editor universal en la aplicación de React:

1. Dependencia del módulo del nodo del registro npm, consulte [@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors).
1. Etiqueta de script (`<script>`) dentro del archivo HTML.

Para este tutorial, vamos a utilizar el método de etiqueta Script.

1. Instale el paquete `react-helmet-async` para administrar la etiqueta `<script>` en la aplicación de React.

   ```bash
   $ npm install react-helmet-async
   ```

1. Actualice el archivo `src/App.js` de la aplicación WKND Teams de React para incluir la biblioteca principal del editor universal.

   ```javascript
   ...
   import { Helmet, HelmetProvider } from "react-helmet-async";
   
   function App() {
   return (
       <HelmetProvider>
           <div className="App">
               <Helmet>
                   {/* AEM Universal Editor :: CORE Library
                     Loads the LATEST Universal Editor library
                   */}
                   <script
                       src="https://universal-editor-service.adobe.io/cors.js"
                       async
                   />
               </Helmet>
               <Router>
                   <header>
                       <Link to={"/"}>
                       <img src={logo} className="logo" alt="WKND Logo" />
                       </Link>
                       <hr />
                   </header>
                   <Routes>
                       <Route path="/" element={<Home />} />
                       <Route path="/person/:fullName" element={<Person />} />
                   </Routes>
               </Router>
           </div>
       </HelmetProvider>
   );
   }
   
   export default App;
   ```

## Añadir metadatos: origen de contenido

Para conectar la aplicación WKND Teams de React _con el origen de contenido_ para su edición, debe proporcionar metadatos de conexión. El servicio del editor universal utiliza estos metadatos para establecer una conexión con el origen de contenido.

Los metadatos de la conexión se almacenan como etiquetas `<meta>` en el archivo HTML. La sintaxis de los metadatos de conexión es la siguiente:

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

Vamos a añadir los metadatos de conexión a la aplicación WKND Teams de React dentro del componente `<Helmet>`. Actualice el archivo `src/App.js` con la siguiente etiqueta `<meta>`. En este ejemplo, el origen de contenido es una instancia local de AEM que se ejecuta en `https://localhost:8443`.

```javascript
...
function App() {
return (
    <HelmetProvider>
        <div className="App">
            <Helmet>
                {/* AEM Universal Editor :: CORE Library
                    Loads the LATEST Universal Editor library
                */}
                <script
                    src="https://universal-editor-service.adobe.io/cors.js"
                    async
                />
                {/* AEM Universal Editor :: Connection metadata 
                    Connects to local AEM instance
                */}
                <meta
                    name="urn:adobe:aue:system:aemconnection"
                    content={`aem:https://localhost:8443`}
                />
            </Helmet>
            ...
    </HelmetProvider>
);
}

export default App;
```

`aemconnection` proporciona un nombre abreviado para el origen de contenido. La instrumentación posterior utiliza el nombre abreviado para hacer referencia al origen de contenido.

## Añadir metadatos: configuración del servicio del editor universal local

En lugar del servicio del editor universal alojado en Adobe, se utiliza una copia local del servicio del editor universal para el desarrollo local. El servicio local enlaza el editor universal y el SDK de AEM, por lo que vamos a añadir los metadatos del servicio del editor universal local a la aplicación de React WKND Teams.

Estos ajustes de configuración también se almacenan como etiquetas `<meta>` en el archivo HTML. La sintaxis de los metadatos del servicio del editor universal local es la siguiente:

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

Vamos a añadir los metadatos de conexión a la aplicación WKND Teams de React dentro del componente `<Helmet>`. Actualice el archivo `src/App.js` con la siguiente etiqueta `<meta>`. En este ejemplo, el servicio de editor universal local se está ejecutando en `https://localhost:8001`.

```javascript
...

function App() {
  return (
    <HelmetProvider>
      <div className="App">
        <Helmet>
          {/* AEM Universal Editor :: CORE Library
              Loads the LATEST Universal Editor library
          */}
          <script
            src="https://universal-editor-service.adobe.io/cors.js"
            async
          />
          {/* AEM Universal Editor :: Connection metadata 
              Connects to local AEM instance
          */}
          <meta
            name="urn:adobe:aue:system:aemconnection"
            content={`aem:https://localhost:8443`}
          />
          {/* AEM Universal Editor :: Configuration for Service
              Using locally running Universal Editor service
          */}
          <meta
            name="urn:adobe:aue:config:service"
            content={`https://localhost:8001`}
          />
        </Helmet>
        ...
    </HelmetProvider>
);
}
export default App;
```

## Instrumentación de los componentes de React

Para editar el contenido de la aplicación WKND Teams de React, como _el título y la descripción del equipo_, debe instrumentar los componentes de React. La instrumentación significa añadir atributos de datos relevantes (`data-aue-*`) a los elementos de HTML que desea hacer editables mediante el editor universal. Para obtener más información sobre los atributos de datos, consulte [Atributos y tipos](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types).

### Definición de los elementos editables

Empecemos por definir los elementos que desea editar con el editor universal. En la aplicación WKND Teams de React, el título y la descripción del equipo se almacenan en el fragmento de contenido del equipo en AEM, por lo tanto, son los mejores candidatos para la edición.

Vamos a instrumentar el componente React de `Teams` para poder editar el título y la descripción del equipo.

1. Abra el archivo `src/components/Teams.js` de la aplicación WKND Teams de React.
1. Añada el atributo `data-aue-prop`, `data-aue-type` y `data-aue-label` a los elementos de título y descripción del equipo.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       return (
           <div className="team">
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <h2 className="team__title" data-aue-prop="title" data-aue-type="text" data-aue-label="title">{title}</h2>
               <p className="team__description" data-aue-prop="description" data-aue-type="richtext" data-aue-label="description">{description.plaintext}</p>
               ...
           </div>
       );
   }
   
   export default Teams;
   ```

1. Actualice la página del editor universal en el explorador que carga la aplicación WKND Teams de React. Ahora puede ver que el título del equipo y los elementos de descripción son editables.

   ![Editor universal: título y descripción editables de WKND Teams](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. Si intenta editar el título o la descripción del equipo mediante la edición en línea o el carril de propiedades, se mostrará un indicador giratorio de carga, pero no le permitirá editar el contenido. Esto se debe a que el editor universal no tiene en cuenta los detalles de recursos de AEM para cargar y guardar el contenido.

   ![Editor universal: carga de título y descripción de WKND Teams](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

En resumen, los cambios anteriores marcan los elementos de título y descripción del equipo como editables en el editor universal. Sin embargo, **no puede editar (a través del carril en línea o de propiedades) y guardar los cambios todavía**. Para ello necesita añadir los detalles del recurso de AEM usando el atributo `data-aue-resource`. Lo haremos en el siguiente paso.

### Definición de detalles de recursos de AEM

Para guardar el contenido editado de nuevo en AEM y también para cargar el contenido en el carril de propiedades, debe proporcionar los detalles del recurso de AEM al editor universal.

En este caso, el recurso de AEM es la ruta del fragmento de contenido del equipo, así que vamos a añadir los detalles del recurso al componente React de `Teams` en el elemento de nivel superior `<div>`.

1. Actualice el archivo `src/components/Teams.js` para añadir los atributos `data-aue-resource`, `data-aue-type` y `data-aue-label` al elemento `<div>` de nivel superior.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       // Render single Team
       function Team({ _path, title, shortName, description, teamMembers }) {
           // Must have title, shortName and at least 1 team member
           if (!_path || !title || !shortName || !teamMembers) {
               return null;
           }
   
         return (
           // AEM Universal Editor :: Instrumentation using data-aue-* attributes
           <div className="team" data-aue-resource={`urn:aemconnection:${_path}/jcr:content/data/master`} data-aue-type="reference" data-aue-label={title}>
           ...
           </div>
       );
       }
   }
   export default Teams;
   ```

   El valor del atributo `data-aue-resource` es la ruta de recursos de AEM del fragmento de contenido del equipo. El prefijo `urn:aemconnection:` utiliza el nombre abreviado del origen de contenido definido en los metadatos de conexión.

1. Actualice la página del editor universal en el explorador que carga la aplicación WKND Teams de React. Ahora puede ver que el elemento Equipo de nivel superior es editable, pero el carril de propiedades sigue sin cargar el contenido. En la pestaña de red del explorador, puede ver el error 401 No autorizado para la solicitud `details` que carga el contenido. Está intentando utilizar el token de IMS para la autenticación, pero el SDK de AEM local no admite la autenticación IMS.

   ![Editor universal: equipo de WKND Teams editable](./assets/universal-editor-wknd-teams-team-editable.png)

1. Para corregir el error 401 No autorizado, debe proporcionar los detalles de autenticación locales del SDK de AEM al editor universal mediante la opción **Encabezados de autenticación** en el editor universal. Al igual que el SDK local de AEM, establezca el valor en `Basic YWRtaW46YWRtaW4=` para las credenciales de `admin:admin`.

   ![Editor universal: añadir encabezados de autenticación](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. Actualice la página del editor universal en el explorador que carga la aplicación WKND Teams de React. Ahora puede ver que el carril de propiedades está cargando el contenido y puede editar el título y la descripción del equipo en línea o mediante el carril de propiedades.

   ![Editor universal: equipo de WKND Teams editable](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### Internamente

El carril de propiedades carga el contenido del recurso de AEM mediante el servicio de editor universal local. Mediante la pestaña de la red del explorador, puede ver la petición POST en el servicio del editor universal local (`https://localhost:8001/details`) para cargar el contenido.

Cuando edita el contenido mediante la edición en línea o el carril de propiedades, los cambios se guardan de nuevo en el recurso de AEM mediante el servicio del editor universal local. Mediante la pestaña de red del explorador, puede ver la petición POST en el servicio del editor universal (`https://localhost:8001/update` o `https://localhost:8001/patch`) para guardar el contenido.

![Editor universal: equipo de WKND Teams editable](./assets/universal-editor-under-the-hood-request.png)

El objeto JSON de carga útil de solicitud contiene los detalles necesarios, como el servidor de contenido (`connections`), la ruta de acceso de recurso (`target`) y el contenido actualizado (`patch`).

![Editor universal: equipo de WKND Teams editable](./assets/universal-editor-under-the-hood-payload.png)

### Expansión del contenido editable

Vamos a expandir el contenido editable y aplicar la instrumentación a los **integrantes del equipo** para que pueda editar los integrantes del equipo mediante el carril de propiedades.

Como en el caso anterior, vamos a añadir los atributos `data-aue-*` relevantes a los integrantes del equipo en el componente `Teams` de React.

1. Actualice el archivo `src/components/Teams.js` para añadir atributos de datos al elemento `<li key={index} className="team__member">`.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {/* Render the referenced Person models associated with the team */}
               {teamMembers.map((teamMember, index) => {
                   return (
                       // AEM Universal Editor :: Instrumentation using data-aue-* attributes
                       <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                       <Link to={`/person/${teamMember.fullName}`}>
                           {teamMember.fullName}
                       </Link>
                       </li>
                   );
               })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

   El valor del atributo `data-aue-type` es `component`, ya que los integrantes del equipo se almacenan como fragmentos de contenido `Person` en AEM y ayudan a indicar las partes movibles o eliminables del contenido.

1. Actualice la página del editor universal en el explorador que carga la aplicación WKND Teams de React. Ahora puede ver que los integrantes del equipo se pueden editar mediante el carril de propiedades.

   ![Editor universal: integrantes del equipo de WKND Teams editables](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### Internamente

Al igual que en el caso anterior, la recuperación y el almacenamiento de contenido se realizan mediante el servicio del editor universal local. Las solicitudes `/details`, `/update` o `/patch` se realizan en el servicio del editor universal local para cargar y guardar el contenido.

### Definición de la adición y eliminación de contenido

Hasta ahora, ha hecho que el contenido existente sea editable, pero ¿qué sucede si desea añadir contenido nuevo? Vamos a añadir la posibilidad de añadir o eliminar integrantes del equipo al equipo de WKND mediante el editor universal. Por lo tanto, los autores de contenido no necesitan ir a AEM para añadir o eliminar integrantes del equipo.

Sin embargo, como resumen rápido, los integrantes del equipo WKND se almacenan como fragmentos de contenido `Person` en AEM y se asocian al fragmento de contenido del equipo mediante la propiedad `teamMembers`. Para revisar la definición del modelo en AEM, visite [my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project).

1. Primero, cree el archivo de definición del componente `/public/static/component-definition.json`. Este archivo contiene la definición del componente para el fragmento de contenido `Person`. El complemento `aem/cf` permite insertar fragmentos de contenido, basados en un modelo y una plantilla que proporcionan los valores predeterminados que aplique.

   ```json
   {
       "groups": [
           {
           "title": "Content Fragments",
           "id": "content-fragments",
           "components": [
               {
               "title": "Person",
               "id": "person",
               "plugins": {
                   "aem": {
                       "cf": {
                           "name": "person",
                           "cfModel": "/conf/my-project/settings/dam/cfm/models/person",
                           "cfFolder": "/content/dam/my-project/en",
                           "title": "person",
                           "template": {
                               "fullName": "New Person",
                               "biographyText": "This is biography of new person"
                               }
                           }
                       }
                   }
               }
           ]
           }
       ]
   }
   ```

1. A continuación, consulte el archivo de definición del componente anterior en el archivo `index.html` de la aplicación WKND Team de React. Actualice la sección `public/index.html` del archivo `<head>` para incluir el archivo de definición del componente.

   ```html
   ...
   <script
       type="application/vnd.adobe.aue.component+json"
       src="/static/component-definition.json"
   ></script>
   <title>WKND App - Basic GraphQL Tutorial</title>
   </head>
   ...
   ```

1. Finalmente, actualice el archivo `src/components/Teams.js` para añadir atributos de datos. Para que la sección **MEMBERS** actúe como un contenedor de los integrantes del equipo, vamos a añadir los atributos `data-aue-prop`, `data-aue-type` y `data-aue-label` al elemento `<div>`.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       {/* AEM Universal Editor :: Team Members as container */}
       <div data-aue-prop="teamMembers" data-aue-type="container" data-aue-label="members">
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
           {/* Render the referenced Person models associated with the team */}
           {teamMembers.map((teamMember, index) => {
               return (
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                   <Link to={`/person/${teamMember.fullName}`}>
                   {teamMember.fullName}
                   </Link>
               </li>
               );
           })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

1. Actualice la página del editor universal en el explorador que carga la aplicación WKND Teams de React. Ahora puede ver que la sección **MEMBERS** actúa como un contenedor. Puede insertar nuevos integrantes del equipo mediante el carril de propiedades y el icono **+**.

   ![Editor universal: inserción de miembros del equipo de WKND Teams](./assets/universal-editor-wknd-teams-add-team-members.png)

1. Para eliminar un miembro del equipo, selecciónelo y haga clic en el icono **Eliminar**.

   ![Editor universal: los integrantes del equipo de WKND Teams eliminan](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### Internamente

Las operaciones de adición y eliminación de contenido las realiza el servicio de editor universal local. La petición POST para `/add` o `/remove` con una carga útil detallada se realiza al servicio del editor universal local para añadir o eliminar el contenido en AEM.

## Archivos de solución

Para comprobar los cambios de implementación o si no puede hacer que la aplicación WKND Teams de React funcione con el editor universal, consulte la rama de la solución [basic-tutorial-instrumented-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE).

La comparación archivo por archivo con la rama de **basic-tutorial** en funcionamiento está disponible [aquí](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1).

## Enhorabuena.

La aplicación WKND Teams de React se ha instrumentado correctamente para añadir, editar y eliminar contenido mediante el editor universal. Ha aprendido a incluir la biblioteca principal, añadir conexión y los metadatos del servicio de editor universal local, e instrumentar el componente React mediante varios atributos de datos (`data-aue-*`).
