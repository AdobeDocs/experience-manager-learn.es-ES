---
title: 'AEM API sin encabezado y React: primer tutorial AEM sin encabezado'
description: Obtenga información sobre cómo recuperar datos de fragmento de contenido de AEM API de GraphQL y mostrarlos en la aplicación React.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 0%

---


# AEM API sin encabezado y React

Le damos la bienvenida a este capítulo del tutorial en el que analizaremos la configuración de una aplicación React para conectarse con las API de Adobe Experience Manager (AEM) sin encabezado mediante el SDK AEM sin encabezado. Trataremos de recuperar datos de fragmento de contenido de AEM API de GraphQL y de mostrarlos en la aplicación React.

AEM API sin encabezado permiten acceder AEM contenido desde cualquier aplicación cliente. Le guiaremos a través de la configuración de su aplicación React para conectarse a las API sin encabezado AEM mediante el SDK sin encabezado de AEM. Esta configuración establece un canal de comunicación reutilizable entre la aplicación React y AEM.

A continuación, utilizaremos el SDK sin encabezado de AEM para recuperar los datos de fragmento de contenido de AEM API de GraphQL. Los fragmentos de contenido de AEM proporcionan una administración de contenido estructurada. Con el SDK sin encabezado de AEM, puede consultar y recuperar fácilmente datos de fragmentos de contenido mediante GraphQL.

Una vez que tengamos los datos del fragmento de contenido, lo integraremos en su aplicación React. Aprenderá a dar formato y mostrar los datos de una manera atractiva. Trataremos las prácticas recomendadas para gestionar y procesar los datos de fragmento de contenido en los componentes de React, lo que garantiza una integración perfecta con la interfaz de usuario de su aplicación.

A lo largo del tutorial, proporcionaremos explicaciones, ejemplos de código y sugerencias prácticas. Al final, podrá configurar la aplicación React para que se conecte a AEM API sin encabezado, recuperar los datos del fragmento de contenido mediante el SDK sin encabezado de AEM y mostrarlos sin problemas en la aplicación React. ¡Empecemos!


## Clonar la aplicación React

1. Clonar la aplicación de [Github](https://github.com/lamontacrook/headless-first/tree/main) ejecutando el siguiente comando en la línea de comandos.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Cambie a `headless-first` e instale las dependencias.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configurar la aplicación React

1. Crear un archivo con el nombre `.env` en la raíz del proyecto. En `.env` establezca los siguientes valores:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Puede recuperar un token de desarrollador en Cloud Manager. Iniciar sesión en [Adobe Cloud Manager](https://experience.adobe.com/). Haga clic en __Experience Manager > Cloud Manager__. Elija el programa adecuado y, a continuación, haga clic en los puntos suspensivos junto al entorno.

   ![AEM Developer Console](./assets/2/developer-console.png)

   1. Haga clic en el __Integraciones__ ficha
   1. Haga clic en __Ficha Token local y obtener token de desarrollo local__ botón
   1. Copie el token de acceso que comienza después de la comilla abierta hasta antes de la comilla de cierre.
   1. Pegue el token copiado como valor para `REACT_APP_TOKEN` en el `.env` archivo.
   1. Ahora vamos a crear la aplicación ejecutando `npm ci` en la línea de comandos.
   1. A continuación, inicie la aplicación React y ejecute `npm run start` en la línea de comandos.
   1. En [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) un archivo denominado `context.js`  incluye el código para establecer los valores en la variable `.env` en el contexto de la aplicación.

## Ejecutar la aplicación React

1. Inicie la aplicación React ejecutando `npm run start` en la línea de comandos.

   ```
   $ npm run start
   ```

   La aplicación React se iniciará y abrirá una ventana del explorador a `http://localhost:3000`. Los cambios realizados en la aplicación React se volverán a cargar automáticamente en el explorador.

## Conectarse a las API AEM sin encabezado

1. Para conectar la aplicación React a AEM as a Cloud Service, vamos a agregar algunas cosas a `App.js`. En el `React` importar, añadir `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importar `AppContext` de la variable `context.js` archivo.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Ahora, dentro del código de la aplicación, defina una variable de contexto.

   ```javascript
   const context = useContext(AppContext);
   ```

   Y, finalmente, ajuste el código de retorno en `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   Como referencia, la variable `App.js` debería ser así.

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. Importe el `AEMHeadless` SDK. Este SDK es una biblioteca de ayuda que la aplicación utiliza para interactuar con AEM API sin encabezado.

   Agregue esta instrucción de importación a `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Agregue lo siguiente `{ useContext, useEffect, useState }` a` React` import .

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importe el `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Dentro de `Home` obtenga el `context` desde la variable `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Inicialización del SDK sin AEM en un  `useEffect()`, ya que el SDK sin encabezado de AEM debe cambiar cuando el  `context` cambios en las variables.

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > Hay un `context.js` file under `/utils` que está leyendo elementos del `.env` archivo. Como referencia, la variable `context.url` es la dirección URL del entorno as a Cloud Service AEM. La variable `context.endpoint` es la ruta completa al extremo creado en la lección anterior. Por último, la `context.token` es el token de desarrollador.


1. Cree un estado React que exponga el contenido proveniente del SDK sin encabezado de AEM.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Conecte la aplicación a AEM. Utilice la consulta persistente creada en la lección anterior. Añadamos el siguiente código dentro del `useEffect` después de inicializar el SDK sin encabezado de AEM. Haga que la variable `useEffect` depende de  `context` como se ve a continuación.


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. Abra la vista Red de las herramientas para desarrolladores para revisar la solicitud de GraphQL.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Herramientas de desarrollo de Chrome](./assets/2/dev-tools.png)

   El SDK sin AEM codifica la solicitud para GraphQL y agrega los parámetros proporcionados. Puede abrir la solicitud en el explorador.

   >[!NOTE]
   >
   > Como la solicitud va al entorno de creación, debe iniciar sesión en el entorno en otra pestaña del mismo explorador.


## Representar contenido de fragmento de contenido

1. Muestre los fragmentos de contenido en la aplicación. Devolver un `<div>` con el título del teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Debería ver el campo de título del teaser en la pantalla.

1. El último paso es añadir el teaser a la página. Se incluye un componente teaser React en el paquete. Primero, incluyamos la importación. En la parte superior del `home.js` , añada la línea:

   `import Teaser from '../../components/teaser/teaser';`

   Actualice la sentencia de retorno:

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   Ahora debería ver el teaser con el contenido incluido en el fragmento.


## Pasos siguientes

Felicitaciones. Ha actualizado correctamente la aplicación React para integrarla con AEM API sin encabezado mediante el SDK sin encabezado de AEM.

A continuación, vamos a crear un componente de lista de imágenes más complejo que genere de forma dinámica fragmentos de contenido a los que se hace referencia desde AEM.

[Capítulo siguiente: Creación de un componente de lista de imágenes](./3-complex-components.md)