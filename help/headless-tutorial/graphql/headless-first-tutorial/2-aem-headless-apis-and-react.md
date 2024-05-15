---
title: 'AEM AEM API sin encabezado y React: primer tutorial sin encabezado'
description: AEM Obtenga información sobre cómo cubrir la recuperación de datos de fragmentos de contenido desde las API de GraphQL y su visualización en la aplicación React.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
duration: 225
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# AEM API sin encabezado y React

Le damos la bienvenida a este capítulo del tutorial, en el que exploraremos la configuración de una aplicación de React para conectarse con las API de Adobe Experience Manager AEM AEM () sin encabezado mediante el SDK sin encabezado de la aplicación de. AEM Cubriremos la recuperación de datos de fragmentos de contenido de las API de GraphQL de la aplicación de y su visualización en la aplicación de React.

AEM AEM Las API sin encabezado de permiten acceder a contenido de la desde cualquier aplicación de cliente. AEM AEM Le guiaremos a través de la configuración de su aplicación React para conectarse a las API de sin encabezado de la red mediante el SDK sin encabezado de la interfaz de usuario de. AEM Esta configuración establece un canal de comunicación reutilizable entre la aplicación React y el usuario de la aplicación de la aplicación de.

AEM AEM A continuación, utilizaremos el SDK sin encabezado de la para recuperar datos de fragmentos de contenido de las API de GraphQL de la. AEM Los fragmentos de contenido en los que se proporciona la administración de contenido estructurada de los. AEM Al utilizar el SDK sin encabezado de, puede consultar y recuperar fácilmente datos de fragmentos de contenido mediante GraphQL.

Una vez que tengamos los datos del fragmento de contenido, los integraremos en su aplicación React. Aprenderá a dar formato y mostrar los datos de una manera atractiva. Cubriremos las prácticas recomendadas para administrar y procesar datos de fragmentos de contenido en los componentes de React, lo que garantiza una integración perfecta con la interfaz de usuario de la aplicación.

A lo largo del tutorial, proporcionaremos explicaciones, ejemplos de código y sugerencias prácticas. AEM AEM Al final, podrá configurar la aplicación React para conectarse a las API de sin encabezado, recuperar datos de fragmentos de contenido mediante el SDK sin encabezado de la aplicación React y mostrarlos sin problemas en la aplicación React. ¡Vamos a empezar!


## Clonar la aplicación React

1. Clonar la aplicación desde [Github](https://github.com/lamontacrook/headless-first/tree/main) ejecutando el siguiente comando en la línea de comandos.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Cambie a la `headless-first` e instale las dependencias.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configuración de la aplicación React

1. Cree un archivo llamado `.env` en la raíz del proyecto. Entrada `.env` establezca los siguientes valores:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Puede recuperar un token de desarrollador en Cloud Manager. Iniciar sesión en [Adobe Cloud Manager](https://experience.adobe.com/). Clic __Experience Manager > Cloud Manager__. Elija el Programa adecuado y, a continuación, haga clic en los puntos suspensivos junto al Entorno.

   ![AEM Consola de desarrollador de](./assets/2/developer-console.png)

   1. Haga clic en en __Integraciones__ pestaña
   1. Clic __Pestaña Token local y obtenga el token de desarrollo local__ botón
   1. Copie el token de acceso comenzando después de la comillas abiertas hasta antes de la comillas de cierre.
   1. Pegue el token copiado como valor para `REACT_APP_TOKEN` en el `.env` archivo.
   1. Ahora vamos a crear la aplicación ejecutando `npm ci` en la línea de comandos.
   1. Ahora inicie la aplicación React y ejecutando `npm run start` en la línea de comandos.
   1. Entrada [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) un archivo denominado `context.js`  incluye el código para establecer los valores en `.env` en el contexto de la aplicación.

## Ejecute la aplicación React

1. Inicie la aplicación React ejecutando `npm run start` en la línea de comandos.

   ```
   $ npm run start
   ```

   La aplicación React se inicia y abre una ventana del explorador para que `http://localhost:3000`. Los cambios en la aplicación React se volverán a cargar automáticamente en el navegador.

## AEM Conexión a las API sin encabezado de

1. AEM Para conectar la aplicación de React a la as a Cloud Service, vamos a añadir algunas cosas a la lista de aplicaciones de React para que se puedan usar en su lugar. `App.js`. En el `React` importar, añadir `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importar `AppContext` desde el `context.js` archivo.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Ahora, en el código de la aplicación, defina una variable de contexto.

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

   Como referencia, la variable `App.js` Ahora debería ser así.

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

1. Importe el `AEMHeadless` SDK. AEM Este SDK es una biblioteca de ayuda que utiliza la aplicación para interactuar con las API sin encabezado de la aplicación de la interfaz de usuario de.

   Añada esta instrucción de importación a `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Añada lo siguiente `{ useContext, useEffect, useState }` a la` React` import.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importe el `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Dentro de `Home` componente, obtenga el `context` de la variable `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. AEM Inicialice el SDK sin encabezado de la dentro de un  `useEffect()`AEM , ya que el SDK sin encabezado de la debe cambiar cuando la variable  `context` cambios de la variable.

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
   > Hay un `context.js` archivo en `/utils` que lee elementos de la `.env` archivo. Como referencia, la variable `context.url` AEM es la dirección URL del entorno as a Cloud Service de la. El `context.endpoint` es la ruta completa al punto final creado en la lección anterior. Por último, el `context.token` es el token de desarrollador.


1. AEM Cree el estado React que expone el contenido proveniente del SDK sin encabezado de.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. AEM Conecte la aplicación a la red de. Utilice la consulta persistente creada en la lección anterior. Vamos a añadir el siguiente código dentro de la variable `useEffect` AEM después de inicializar el SDK sin encabezado de la. Realice la `useEffect` depende del  `context` como se ve a continuación.


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

1. Abra la vista Red de las herramientas del desarrollador para revisar la solicitud de GraphQL.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Herramientas de desarrollo de Chrome](./assets/2/dev-tools.png)

   AEM El SDK sin encabezado de la codifica la solicitud de GraphQL y agrega los parámetros proporcionados. Puede abrir la solicitud en el explorador.

   >[!NOTE]
   >
   > Dado que la solicitud va al entorno de creación, debe iniciar sesión en el entorno en otra pestaña del mismo explorador.


## Procesar contenido de fragmentos de contenido

1. Muestre los fragmentos de contenido en la aplicación. Devolver un `<div>` con el título del teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Debería ver el campo de título del teaser en la pantalla.

1. El último paso es añadir el teaser a la página. Se incluye un componente de teaser de React en el paquete. En primer lugar, vamos a incluir la importación. En la parte superior del `home.js` , añada la línea:

   `import Teaser from '../../components/teaser/teaser';`

   Actualice la instrucción return:

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   Ahora debería ver el teaser con el contenido incluido en el fragmento.


## Siguientes pasos

Enhorabuena. AEM AEM ¡Ha actualizado correctamente la aplicación React para integrarla con las API de sin encabezado de mediante el SDK sin encabezado

AEM A continuación, vamos a crear un componente de lista de imágenes más complejo que procese dinámicamente los fragmentos de contenido referenciados a partir de los fragmentos de contenido que se han.

[Capítulo siguiente: Creación de un componente de lista de imágenes](./3-complex-components.md)
