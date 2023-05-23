---
title: Bootstrap SPA SPA de la instancia remota de Editor
description: SPA AEM SPA Obtenga información sobre cómo arrancar una aplicación remota para comprobar la compatibilidad con el Editor de de datos.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1200'
ht-degree: 1%

---

# Bootstrap SPA SPA de la instancia remota de Editor

SPA AEM SPA Antes de poder agregar las áreas editables al sitio remoto, debe arrancarse con el SDK de JavaScript del Editor de la y algunas otras configuraciones.

## AEM SPA Instalación de dependencias npm del SDK de JS de

AEM SPA En primer lugar, revise las dependencias de npm de la de React y, a continuación, instálelas.

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) AEM : proporciona la API para recuperar contenido de los recursos de la red de.
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) AEM SPA : proporciona la API que asigna contenido de la a los componentes de la.
+ [`@adobe/aem-react-editable-components` Versión 2](https://github.com/adobe/aem-react-editable-components) SPA : proporciona una API para crear componentes de personalizados y proporciona implementaciones de uso común como `AEMPage` Componente React.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## SPA Revisar variables de entorno

SPA AEM Varias variables de entorno deben exponerse al entorno remoto para que sepa cómo interactuar con los usuarios de la interfaz de usuario de la interfaz de usuario de.

1. SPA Abra el proyecto remoto de en `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` en su IDE
1. Abra el archivo `.env.development`
1. En el archivo, preste atención específica a las claves y actualice según sea necesario:

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![SPA Variables de entorno remoto](./assets/spa-bootstrap/env-variables.png)

   *Recuerde que las variables de entorno personalizadas en React deben ir precedidas de `REACT_APP_`.*

   + `REACT_APP_HOST_URI`AEM SPA : el esquema y el host del servicio de al que se conecta el remoto.
      + AEM AEM Este valor cambia en función de si el entorno de trabajo (local, de desarrollo, de fase o de producción) y el tipo de servicio (de autor o de publicación) son los que se utilizan para la creación y la publicación, respectivamente.
   + `REACT_APP_USE_PROXY`AEM : esto evita problemas de CORS durante el desarrollo, ya que indica al servidor de desarrollo de react que realice solicitudes de conexión de proxy, como las que se muestran a continuación `/content, /graphql, .model.json` usando `http-proxy-middleware` módulo.
   + `REACT_APP_AUTH_METHOD`AEM : método de autenticación para solicitudes atendidas, las opciones son &#39;service-token&#39;, &#39;dev-token&#39;, &#39;basic&#39; o dejar en blanco para caso de uso sin autenticación
      + Necesario para su uso con AEM Author
      + Posiblemente se requiera para su uso con AEM Publish (si el contenido está protegido)
      + AEM El desarrollo con el SDK de admite cuentas locales a través de la autenticación básica. Este es el método que se utiliza en este tutorial.
      + AEM Al integrar con as a Cloud Service, utilice [tokens de acceso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   + `REACT_APP_BASIC_AUTH_USER`AEM : la lista de __nombre de usuario__ SPA AEM por el usuario para autenticarse durante la recuperación de contenido de la.
   + `REACT_APP_BASIC_AUTH_PASS`AEM : la lista de __contraseña__ SPA AEM por el usuario para autenticarse durante la recuperación de contenido de la.

## Integración de la API de ModelManager

AEM SPA AEM Con las dependencias npm disponibles para la aplicación, inicialice el proceso de `ModelManager` en el del proyecto `index.js` antes `ReactDOM.render(...)` se invoca a.

El [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) AEM es responsable de conectarse a la para recuperar contenido editable.

1. SPA Abra el proyecto de remoto en el IDE
1. Abra el archivo `src/index.js`
1. Añadir importación `ModelManager` e inicialícelo antes de que `root.render(..)` invocación de,

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

El `src/index.js` el archivo debe tener un aspecto similar al siguiente:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## SPA Configuración de un proxy interno de la

SPA Al crear un archivo editable, es mejor configurar una variable de forma que se pueda crear una lista de elementos editables. [SPA proxy interno en el](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)AEM , que está configurado para enrutar las solicitudes adecuadas a los grupos de trabajo de los que se va a realizar la solicitud de. Esto se realiza utilizando [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm, que ya está instalado por la aplicación base WKND GraphQL.

1. SPA Abra el proyecto de remoto en el IDE
1. Abra el archivo en `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. Actualice el archivo con el siguiente código:

   ```javascript
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') || 
               path.startsWith('/graphql') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure/') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   El `setupProxy.spa-editor.auth.basic.js` el archivo debe tener un aspecto similar al siguiente:

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Esta configuración proxy hace dos cosas principales:

   1. SPA Solicitudes específicas de proxy realizadas a la aplicación de la (`http://localhost:3000`AEM ) a la `http://localhost:4502`
      + AEM Solo procesa solicitudes cuyas rutas coinciden con patrones que indican que deben ser atendidas por el usuario, tal como se define en la sección `toAEM(path, req)`.
      + SPA AEM Vuelve a escribir rutas de acceso de la a sus homólogas, tal como se define en `pathRewriteToAEM(path, req)`
   1. AEM Añade encabezados CORS a todas las solicitudes para permitir el acceso al contenido de la, tal como se define en `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + AEM SPA Si no se añade, se producen errores de CORS al cargar contenido de la.

1. Abra el archivo `src/setupProxy.js`
1. Revise la línea que señala a `setupProxy.spa-editor.auth.basic` archivo de configuración proxy:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

Tenga en cuenta que cualquier cambio en el `src/setupProxy.js` SPA o sus archivos de referencia requieren un reinicio de los archivos de la.

## SPA Recurso estático

SPA SPA Los recursos de la estática, como el logotipo de WKND y la carga de gráficos, deben tener actualizadas sus URL de origen para forzarlos a cargarse desde el host remoto de la. SPA SPA AEM SPA Si se deja en relativo, cuando el se carga en Editor de para la creación, estas URL usan de forma predeterminada el host de la en lugar del de la, lo que da como resultado 404 solicitudes, como se muestra en la imagen siguiente.

![Recursos estáticos dañados](./assets/spa-bootstrap/broken-static-resource.png)

SPA SPA Para resolver este problema, haga que un recurso estático alojado por el usuario remoto utilice rutas absolutas que incluyan el origen de la remota.

1. SPA Abra el proyecto de la en su IDE
1. SPA Abra el archivo de variables de entorno de la `src/.env.development` SPA y agregue una variable para el URI público de la:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _AEM Al implementar en el as a Cloud Service de la, debe hacer lo mismo para el correspondiente `.env` archivos._

1. Abra el archivo `src/App.js`
1. SPA SPA Importar el URI público de la variables de entorno

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Prefijo del logotipo de WKND `<img src=.../>` con `REACT_APP_PUBLIC_URI` SPA para forzar la resolución contra la.

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Haga lo mismo para cargar la imagen en `src/components/Loading.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. Y para el __dos instancias__ del botón Atrás en `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

El `App.js`, `Loading.js`, y `AdventureDetails.js` los archivos deben tener este aspecto:

![Recursos estáticos](./assets/spa-bootstrap/static-resources.png)

## AEM Cuadrícula adaptable

SPA SPA AEM SPA Para admitir el modo de diseño de Editor de la para las áreas editables, debemos integrar CSS de cuadrícula adaptable de la aplicación en la. SPA No se preocupe: este sistema de cuadrícula solo es aplicable a los contenedores editables y puede utilizar el sistema de cuadrícula que elija para controlar el diseño del resto de la.

AEM SPA Añada los archivos SCSS de cuadrícula interactiva de la.

1. SPA Abra el proyecto de la en su IDE
1. Descargue y copie los dos archivos siguientes en `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + AEM Generador SCSS de cuadrícula interactiva de la
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Invoca `_grid.scss` SPA uso de los puntos de interrupción específicos de la (escritorio y móvil) y columnas (12).
1. Abrir `src/App.scss` e importar `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

El `_grid.scss` y `_grid-init.scss` los archivos deben tener este aspecto:

![AEM SCSS de cuadrícula interactiva](./assets/spa-bootstrap/aem-responsive-grid.png)

SPA AEM AEM Ahora la incluye el CSS necesario para admitir el modo de diseño de la para los componentes añadidos a un contenedor de elementos de un contenedor de elementos de diseño.

## Clases de utilidad

Copie las siguientes clases de utilidades en el proyecto de aplicación React.

+ [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) hasta `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
+ [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) hasta `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
+ [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) hasta `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
+ [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) hasta `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![SPA Clases de utilidad remota](./assets/spa-bootstrap/utility-classes.png)

## SPA Iniciar la creación de la

SPA AEM SPA Ahora que la está preparada para la integración con, vamos a ejecutar la y ver qué aspecto tiene.

1. SPA En la línea de comandos, navegue hasta la raíz del proyecto de
1. SPA Inicie la con los comandos normales (si aún no lo ha hecho).

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. SPA Explorar la lista de [http://localhost:3000](http://localhost:3000). ¡Todo debería quedar bien!

![SPA Ejecución de la en http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## SPA AEM SPA Abra la en el Editor de

SPA Con la en marcha [http://localhost:3000](http://localhost:3000)AEM SPA , vamos a abrirlo con el Editor de la de datos de la aplicación. SPA SPA AEM Todavía no se puede editar nada en el, esto solo valida el en el que se ha realizado la acción.

1. Inicie sesión en AEM Author
1. Vaya a __Sites > Aplicación WKND > us > es__
1. Seleccione el __Página de inicio de la aplicación WKND__ y pulse __Editar__ SPA , y aparecerá el cuadro de diálogo de la.

   ![Página principal de la aplicación Editar WKND](./assets/spa-bootstrap/edit-home.png)

1. Cambiar a __Previsualizar__ uso del interruptor de modo en la parte superior derecha
1. SPA Haga clic en torno a la

   ![SPA Ejecución de la en http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Enhorabuena.

SPA AEM SPA ¡Ha arrancado el control remoto para que sea compatible con el editor de! Ahora ya sabe cómo:

+ AEM SPA SPA Añadir las dependencias npm del SDK de JS de Editor de al proyecto de
+ SPA Configuración de las variables de entorno de la
+ SPA Integración de la API de ModelManager con el
+ SPA AEM Configure un proxy interno para la, de modo que enrute las solicitudes de contenido apropiadas para que se realicen las solicitudes de acceso a la red de distribución de contenido ()
+ SPA SPA Solucionar problemas con recursos de la estática que se resuelven en el contexto del Editor de
+ AEM AEM Agregar CSS de cuadrícula interactiva para admitir la creación de diseños en contenedores editables de la

## Pasos siguientes

AEM SPA Ahora que hemos logrado una línea de base de compatibilidad con el Editor de la, podemos empezar a introducir áreas editables. Primero miramos cómo colocar un objeto [componente editable fijo](./spa-fixed-component.md) SPA en el menú de la.
