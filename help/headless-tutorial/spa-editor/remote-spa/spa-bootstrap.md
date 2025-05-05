---
title: Bootstrap SPA SPA de la instancia remota de Editor
description: SPA AEM SPA Obtenga información sobre cómo arrancar una aplicación remota para comprobar la compatibilidad con el Editor de de datos.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 327
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---

# Bootstrap SPA SPA de la instancia remota de Editor

SPA AEM SPA Antes de poder agregar las áreas editables a la interfaz de usuario remota, se debe arrancar con el SDK de JavaScript de Editor de y algunas otras configuraciones.

## AEM SPA Instalación de dependencias npm del SDK de JS de

AEM SPA En primer lugar, revise las dependencias de npm de la de los usuarios de la aplicación para el proyecto React y, a continuación, instálelas.

+ AEM [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : proporciona la API para recuperar contenido de los recursos de la base de datos de.
+ AEM SPA [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : proporciona la API que asigna contenido de la a los componentes de la.
+ SPA [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) : proporciona una API para generar componentes de personalizados y proporciona implementaciones de uso común como el componente React `AEMPage`.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## SPA Revisar variables de entorno

SPA AEM Varias variables de entorno deben exponerse al entorno remoto para que sepa cómo interactuar con los usuarios de la interfaz de usuario de.

1. SPA Abrir el proyecto de Remote en `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` en su IDE
1. Abrir el archivo `.env.development`
1. En el archivo, preste atención específica a las claves y actualice según sea necesario:

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   SPA ![Variables de entorno de remoto](./assets/spa-bootstrap/env-variables.png)

   *Recuerde que las variables de entorno personalizadas en React deben tener el prefijo `REACT_APP_`.*

   + AEM SPA `REACT_APP_HOST_URI`: el esquema y el host del servicio de al que se conecta el remoto.
      + AEM AEM Este valor cambia en función de si el entorno de trabajo (local, de desarrollo, de fase o de producción) y el tipo de servicio (de autor o de Publish) son los que se utilizan para la creación de la aplicación (o de producción), y el tipo de servicio (de autor o de producción), respectivamente.
   + AEM `REACT_APP_USE_PROXY`: esto evita los problemas de CORS durante el desarrollo, ya que indica al servidor de desarrollo de react que realice solicitudes de acceso de proxy, como `/content, /graphql, .model.json`, mediante el módulo `http-proxy-middleware`.
   + AEM `REACT_APP_AUTH_METHOD`: método de autenticación para solicitudes atendidas por el servicio, las opciones son &quot;service-token&quot;, &quot;dev-token&quot;, &quot;basic&quot; o dejar en blanco para el caso de uso sin autenticación
      + AEM Necesario para su uso con el autor de la
      + AEM Posiblemente se requiera para su uso con Publish (si el contenido está protegido).
      + AEM El desarrollo con el SDK de admite cuentas locales a través de la autenticación básica. Este es el método que se utiliza en este tutorial.
      + Al integrarse con AEM as a Cloud Service, utilice [tokens de acceso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=es)
   + AEM SPA AEM `REACT_APP_BASIC_AUTH_USER`: el __nombre de usuario__ de la para la autenticación al recuperar el contenido de la recuperación.
   + AEM SPA AEM `REACT_APP_BASIC_AUTH_PASS`: la __contraseña__ de la para la autenticación durante la recuperación de contenido de la.

## Integración de la API de ModelManager

AEM SPA AEM Con las dependencias npm disponibles para la aplicación, inicialice la inicialización de `ModelManager` en el elemento `index.js` del proyecto antes de que se invoque a `ReactDOM.render(...)`.

AEM [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) es responsable de conectarse a los recursos para recuperar el contenido editable de los que se ha hecho clic en el botón de acceso a la página de inicio de la página de inicio de sesión.

1. SPA Abra el proyecto de remoto en el IDE
1. Abrir el archivo `src/index.js`
1. Agregue la importación `ModelManager` e inicialícela antes de la invocación de `root.render(..)`,

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

El archivo `src/index.js` debe tener el siguiente aspecto:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## SPA Configuración de un proxy interno de la

SPA SPA AEM Al crear un elemento editable, es mejor configurar un proxy interno [en el servidor de correo electrónico ](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), que esté configurado para enrutar las solicitudes adecuadas a la dirección de correo electrónico Esto se hace usando el módulo [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm, que ya está instalado en la aplicación WKND GraphQL base.

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

   El archivo `setupProxy.spa-editor.auth.basic.js` debe tener el siguiente aspecto:

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Esta configuración proxy hace dos cosas principales:

   1. SPA AEM Solicitudes específicas de proxy realizadas a los clientes de la red (`http://localhost:3000`) para el servidor de correo electrónico de la red (`http://localhost:4502`) para el servidor de correo electrónico de la red de correo electrónico de la red de correo electrónico
      + AEM Solo procesa solicitudes cuyas rutas coinciden con patrones que indican que deben ser atendidas por el usuario, tal como se define en `toAEM(path, req)`.
      + SPA AEM Vuelve a escribir rutas de acceso de los recursos de la parte contraria, tal como se define en `pathRewriteToAEM(path, req)`.
   1. AEM Agrega encabezados CORS a todas las solicitudes para permitir el acceso al contenido de la, tal como se define en `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + AEM SPA Si no se añade, se producen errores de CORS al cargar contenido de la.

1. Abrir el archivo `src/setupProxy.js`
1. Revise la línea que señala al archivo de configuración del proxy `setupProxy.spa-editor.auth.basic`:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

SPA Tenga en cuenta que cualquier cambio realizado en `src/setupProxy.js` o en sus archivos a los que se hace referencia requiere un reinicio de la.

## SPA Recurso estático

SPA SPA Los recursos de la estática, como el logotipo de WKND y la carga de gráficos, deben tener actualizadas sus URL de origen para forzarlos a cargarse desde el host remoto de la. SPA SPA AEM SPA Si se deja en relativo, cuando el se carga en Editor de para la creación, estas URL utilizan de forma predeterminada el host de la aplicación en lugar de la aplicación de la aplicación, lo que da como resultado 404 solicitudes, tal como se muestra en la imagen siguiente.

![Recursos estáticos rotos](./assets/spa-bootstrap/broken-static-resource.png)

SPA SPA Para resolver este problema, haga que un recurso estático alojado por el usuario remoto utilice rutas absolutas que incluyan el origen de la remota.

1. SPA Abra el proyecto de la en su IDE
1. SPA SPA Abra el archivo de variables de entorno `src/.env.development` y agregue una variable para el URI público de la:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Al implementar en AEM as a Cloud Service, debe hacer lo mismo con los `.env` archivos correspondientes._

1. Abrir el archivo `src/App.js`
1. SPA SPA Importar el URI público de la variables de entorno

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. SPA Agregue a `REACT_APP_PUBLIC_URI` el prefijo del logotipo de WKND `<img src=.../>` para forzar la resolución contra el.

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

1. Y para las __dos instancias__ del botón Atrás en `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

Los archivos `App.js`, `Loading.js` y `AdventureDetails.js` deben tener el siguiente aspecto:

![Recursos estáticos](./assets/spa-bootstrap/static-resources.png)

## AEM Cuadrícula adaptable

SPA SPA AEM SPA Para admitir el modo de diseño de Editor de para áreas editables en el recurso, debemos integrar CSS de cuadrícula adaptable de la aplicación de forma adaptable de la aplicación de forma en el menú de la página de la página de la página de. SPA No se preocupe: este sistema de cuadrícula solo es aplicable a los contenedores editables y puede utilizar el sistema de cuadrícula que elija para controlar el diseño del resto de la.

AEM SPA Añada los archivos SCSS de cuadrícula interactiva de la.

1. SPA Abra el proyecto de la en su IDE
1. Descargar y copiar los dos archivos siguientes en `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + AEM Generador SCSS de cuadrícula interactiva de la
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + SPA Invoca `_grid.scss` mediante los puntos de interrupción específicos del (escritorio y móvil) y las columnas (12).
1. Abrir `src/App.scss` e importar `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

Los archivos `_grid.scss` y `_grid-init.scss` deben tener el siguiente aspecto:

AEM ![SCSS de cuadrícula adaptable de](./assets/spa-bootstrap/aem-responsive-grid.png)

SPA AEM AEM Ahora la incluye el CSS necesario para admitir el modo de diseño de la aplicación de la aplicación para los componentes añadidos a un contenedor de.

## Clases de utilidad

Copie las siguientes clases de utilidades en el proyecto de aplicación React.

+ [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
+ [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
+ [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
+ [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

SPA ![Clases de utilidad remota de la aplicación](./assets/spa-bootstrap/utility-classes.png)

## SPA Iniciar la creación de la

SPA AEM SPA Ahora que la está preparada para la integración con, vamos a ejecutar la y ver qué aspecto tiene.

1. SPA En la línea de comandos, navegue hasta la raíz del proyecto de
1. SPA Inicie la con los comandos normales (si aún no lo ha hecho).

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. SPA Examine la en [http://localhost:3000](http://localhost:3000). ¡Todo debería quedar bien!

SPA ![en ejecución en http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## SPA AEM SPA Abra la en el Editor de

SPA AEM SPA Con la en ejecución en [http://localhost:3000](http://localhost:3000), vamos a abrirla con el Editor de la. SPA SPA AEM Todavía no se puede editar nada en el, esto solo valida el en el que se ha realizado la acción.

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Sitios > Aplicación WKND > us > en__
1. SPA Seleccione la __página de inicio de la aplicación WKND__ y pulse __Editar__, y aparecerá el cuadro de diálogo de la aplicación.

   ![Editar página principal de la aplicación WKND](./assets/spa-bootstrap/edit-home.png)

1. Cambiar a __vista previa__ mediante el conmutador de modo en la parte superior derecha
1. SPA Haga clic en torno a la

   SPA ![en ejecución en http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Enhorabuena.

SPA AEM SPA ¡Ha arrancado el control remoto para que sea compatible con el editor de! Ahora ya sabe cómo:

+ AEM SPA SPA Añadir las dependencias npm del SDK de JS de Editor de al proyecto de
+ SPA Configuración de las variables de entorno de la
+ SPA Integración de la API de ModelManager con el
+ SPA AEM Configure un proxy interno para la, de modo que enrute las solicitudes de contenido apropiadas para que se realicen las solicitudes de acceso a la red de distribución de contenido ()
+ SPA SPA Solucionar problemas con recursos de la estática que se resuelven en el contexto del Editor de
+ AEM AEM Agregar CSS de cuadrícula adaptable de para admitir la creación de diseños en contenedores editables que se puedan usar en el diseño

## Siguientes pasos

AEM SPA Ahora que hemos logrado una línea de base de compatibilidad con el Editor de la, podemos empezar a introducir áreas editables. SPA Primero se busca cómo colocar un [componente editable fijo](./spa-fixed-component.md) en el cuadro de diálogo de la interfaz de usuario de la página de la página de inicio de la página de la página de.
