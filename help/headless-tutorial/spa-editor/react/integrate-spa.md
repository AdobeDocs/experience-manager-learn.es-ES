---
title: Integrar un SPA | Introducción al AEM SPA Editor y React
description: Comprenda cómo el código fuente de una aplicación de una sola página (SPA) escrita en React se puede integrar con un proyecto de Adobe Experience Manager (AEM). Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de webpack, para desarrollar rápidamente el SPA con la API del modelo JSON de AEM.
sub-product: sites
feature: SPA Editor
version: Cloud Service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
source-git-commit: 415ad1266925525a6b0954b4515bae3d152bb6ec
workflow-type: tm+mt
source-wordcount: '1840'
ht-degree: 1%

---

# Integrar el SPA {#developer-workflow}

Comprenda cómo el código fuente de una aplicación de una sola página (SPA) escrita en React se puede integrar con un proyecto de Adobe Experience Manager (AEM). Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de webpack, para desarrollar rápidamente el SPA con la API del modelo JSON de AEM.

## Objetivo

1. Comprender cómo el proyecto de SPA está integrado con AEM con bibliotecas del lado del cliente.
2. Aprenda a utilizar un servidor de desarrollo de webpack para el desarrollo de front-end dedicado.
3. Explorar el uso de un **proxy** y estático **mock** para desarrollarse con la API del modelo JSON de AEM.

## Qué va a generar

En este capítulo, realizará varios pequeños cambios en la SPA para comprender cómo se integra con la AEM.
Este capítulo agregará una `Header` al SPA. En el proceso de desarrollo de esto **static** `Header` se utilizarán varios enfoques para AEM desarrollo de SPA.

![Nuevo encabezado en AEM](./assets/integrate-spa/final-header-component.png)

*El SPA se amplía para agregar un `Header` componente*

## Requisitos previos

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Este capítulo es una continuación del [Crear proyecto](create-project.md) capítulo, sin embargo, para seguir todo lo que necesita es un proyecto de AEM habilitado para SPA.

## Enfoque de integración {#integration-approach}

Se crearon dos módulos como parte del proyecto AEM: `ui.apps` y `ui.frontend`.

La variable `ui.frontend` es un [webpack](https://webpack.js.org/) proyecto que contiene todo el código fuente de SPA. La mayoría de las pruebas y el desarrollo de SPA se realizarán en el proyecto de webpack. Cuando se activa una compilación de producción, la SPA se crea y se compila mediante un webpack. Los artefactos compilados (CSS y Javascript) se copian en la variable `ui.apps` que luego se implementa en el tiempo de ejecución de AEM.

![arquitectura de alto nivel ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Descripción general de la integración de SPA.*

La información adicional sobre la compilación del front-end puede ser [se encuentra aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Integración de Inspect con SPA {#inspect-spa-integration}

A continuación, revise la `ui.frontend` para comprender el SPA que el [AEM tipo de archivo del proyecto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. En el IDE de su elección, abra su proyecto AEM. Este tutorial utilizará la variable [Código IDE de Visual Studio](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode: AEM proyecto WKND SPA](./assets/integrate-spa/vscode-ide-openproject.png)

1. Expanda e inspeccione la variable `ui.frontend` carpeta. Abra el archivo `ui.frontend/package.json`

1. En el `dependencies` debería ver varias relacionadas con `react` incluido `react-scripts`

   La variable `ui.frontend` es una aplicación React basada en la variable [Crear aplicación React](https://create-react-app.dev/) o CRA para abreviar. La variable `react-scripts` indica qué versión de CRA se utiliza.

1. También hay varias dependencias con el prefijo `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   Los módulos anteriores forman el [AEM SDK de JS Editor SPA](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) y proporcionar la funcionalidad para que sea posible asignar SPA componentes a AEM componentes.

   También se incluyen [Componentes de WCM de AEM: implementación principal de React](https://github.com/adobe/aem-react-core-wcm-components-base) y [Componentes de WCM de AEM: editor de spa: implementación de React Core](https://github.com/adobe/aem-react-core-wcm-components-spa). Se trata de un conjunto de componentes de interfaz de usuario reutilizables que se asignan a componentes de AEM listos para usar. Están diseñadas para utilizarse tal cual y para satisfacer las necesidades del proyecto.

1. En el `package.json` hay varios `scripts` definida:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Estos son scripts de compilación estándar creados [available](https://create-react-app.dev/docs/available-scripts) por la aplicación Crear reacción.

   La única diferencia es la adición de `&& clientlib` a `build` secuencia de comandos. Esta instrucción adicional es responsable de copiar el SPA compilado en la variable `ui.apps` como biblioteca del lado del cliente durante una compilación.

   El módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) se utiliza para facilitar este proceso.

1. Inspect el archivo `ui.frontend/clientlib.config.js`. Este archivo de configuración lo utiliza [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar cómo generar la biblioteca de cliente.

1. Inspect el archivo `ui.frontend/pom.xml`. Este archivo transforma el `ui.frontend` carpeta en un [Módulo Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). La variable `pom.xml` se ha actualizado para usar la variable [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) a **prueba** y **versión** el SPA durante una compilación de Maven.

1. Inspect el archivo `index.js` at `ui.frontend/src/index.js`:

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js` es el punto de entrada del SPA. `ModelManager` es proporcionado por AEM SPA Editor JS SDK. Es responsable de llamar e inyectar el `pageModel` (el contenido JSON) en la aplicación.

1. Inspect el archivo `import-components.js` at `ui.frontend/src/components/import-components.js`. Este archivo importa el valor predeterminado **Reacción de componentes principales** y los pone a disposición del proyecto. Inspeccionaremos la asignación de AEM contenido a los componentes de SPA en el capítulo siguiente.

## Añadir un componente SPA estático {#static-spa-component}

A continuación, agregue un componente nuevo a la SPA e implemente los cambios en una instancia de AEM local. Este será un cambio sencillo, solo para ilustrar cómo se actualiza el SPA.

1. En el `ui.frontend` módulo, debajo `ui.frontend/src/components` crear una nueva carpeta con el nombre `Header`.
1. Crear un archivo con el nombre `Header.js` debajo del `Header` carpeta.

   ![Carpeta y archivo de encabezado](assets/create-project/header-folder-js.png)

1. Rellenar `Header.js` con lo siguiente:

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   Arriba hay un componente React estándar que mostrará una cadena de texto estático.

1. Abra el archivo `ui.frontend/src/App.js`. Este es el punto de entrada de la aplicación.
1. Realice las siguientes actualizaciones para `App.js` para incluir el estático `Header`:

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

1. Abra un terminal nuevo y vaya a la `ui.frontend` y ejecute el `npm run build` comando:

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

1. Vaya a la `ui.apps` carpeta. Bajo `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` debería ver que los archivos SPA compilados se han copiado de la variable`ui.frontend/build` carpeta.

   ![Biblioteca de cliente generada en ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

1. Vuelva al terminal y navegue hasta el `ui.apps` carpeta. Ejecute el siguiente comando Maven:

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   Esto implementará la variable `ui.apps` a una instancia local en ejecución de AEM.

1. Abra una pestaña del explorador y vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Ahora debería ver el contenido del `Header` componente que se muestra en el SPA.

   ![Implementación inicial del encabezado](./assets/integrate-spa/initial-header-implementation.png)

   Los pasos anteriores se ejecutan automáticamente al activar una compilación de Maven desde la raíz del proyecto (p. ej. `mvn clean install -PautoInstallSinglePackage`). Ahora debe comprender los conceptos básicos de la integración entre las bibliotecas de SPA y AEM del lado del cliente. Observe que aún puede editar y agregar `Text` componentes en AEM debajo de la etiqueta estática `Header` componente.

## Servidor de desarrollo de Webpack: proxy de la API JSON {#proxy-json}

Como se ha visto en ejercicios anteriores, realizar una compilación y sincronizar la biblioteca del cliente con una instancia local de AEM tarda unos minutos. Esto es aceptable para las pruebas finales, pero no es ideal para la mayoría del desarrollo de SPA.

A [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) puede utilizarse para desarrollar rápidamente el SPA. El SPA está impulsado por un modelo JSON generado por AEM. En este ejercicio, el contenido JSON de una instancia de AEM en ejecución será **proxy** en el servidor de desarrollo.

1. Vuelva al IDE y abra el archivo `ui.frontend/package.json`.

   Busque una línea como la siguiente:

   ```json
   "proxy": "http://localhost:4502",
   ```

   La variable [Crear aplicación React](https://create-react-app.dev/docs/proxying-api-requests-in-development) proporciona un mecanismo fácil para las solicitudes de API proxy. Todas las solicitudes desconocidas se procesarán como proxy mediante `localhost:4502`, el inicio rápido AEM local.

1. Abra una ventana de terminal y vaya a la `ui.frontend` carpeta. Ejecutar el comando `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

1. Abra una nueva pestaña del explorador (si aún no está abierta) y vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Servidor de desarrollo de Webpack: json proxy](./assets/integrate-spa/webpack-dev-server-1.png)

   Debería ver el mismo contenido que en AEM, pero sin ninguna de las capacidades de creación habilitadas.

   >[!NOTE]
   >
   > Debido a los requisitos de seguridad de AEM, deberá iniciar sesión en la instancia de AEM local (http://localhost:4502) en el mismo explorador, pero en una pestaña diferente.

1. Vuelva al IDE y cree un archivo denominado `Header.css` en el `src/components/Header` carpeta.
1. Rellene el `Header.css` con lo siguiente:

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![IDE de VSCode](assets/integrate-spa/header-css-update.png)

1. Volver a abrir `Header.js` y agregue la siguiente línea para hacer referencia a `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   Guarde los cambios.

1. Vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) para ver los cambios de estilo reflejados automáticamente.

1. Abra el archivo . `Page.css` at `ui.frontend/src/components/Page`. Realice los siguientes cambios para corregir el relleno:

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. Vuelva al explorador en [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Debería ver inmediatamente los cambios realizados en la aplicación.

   ![Estilo añadido al encabezado](assets/integrate-spa/added-logo-localhost.png)

   Puede seguir actualizando el contenido en AEM y verlo reflejado en **webpack-dev-server**, ya que estamos proxizando el contenido.

1. Detenga el servidor de desarrollo de webpack con `ctrl+c` en el terminal.

## Implementar SPA actualizaciones en AEM

Los cambios realizados en la variable `Header` actualmente solo son visibles a través de la variable **webpack-dev-server**. Implemente el SPA actualizado en AEM para ver los cambios.

1. Vaya a la raíz del proyecto (`aem-guides-wknd-spa`) e implemente el proyecto para AEM con Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Debería ver la `Header` y los estilos aplicados.

   ![Encabezado actualizado en AEM](assets/integrate-spa/final-header-component.png)

   Ahora que la SPA actualizada está en AEM, la creación puede continuar.

## Felicitaciones! {#congratulations}

Felicidades, ha actualizado la SPA y explorado la integración con AEM! Ya sabe cómo desarrollar el SPA con la API del modelo JSON de AEM mediante un **webpack-dev-server**.

### Siguientes pasos {#next-steps}

[Asignación de componentes de SPA a componentes de AEM](map-components.md) : Aprenda a asignar componentes de React a componentes de Adobe Experience Manager (AEM) con el SDK de JS de AEM Editor SPA. La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas de los componentes de SPA dentro del AEM SPA Editor, de forma similar a la creación de AEM tradicional.

## Servidor de desarrollo de Webpack (bono): API de JSON de prueba {#mock-json}

Otro enfoque para el desarrollo rápido es utilizar un archivo JSON estático para actuar como el modelo JSON. Al &quot;burlarse&quot; del JSON, eliminamos la dependencia de una instancia de AEM local. También permite a un desarrollador front-end actualizar el modelo JSON para probar la funcionalidad y dirigir los cambios a la API JSON que luego sería implementada por un desarrollador back-end.

La configuración inicial del JSON de prueba hace **requerir una instancia de AEM local**.

1. Vuelva al IDE y vaya a `ui.frontend/public` y añada una nueva carpeta denominada `mock-content`.
1. Cree un nuevo archivo con el nombre `mock.model.json` debajo de `ui.frontend/public/mock-content`.
1. En el navegador, vaya a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Este es el JSON exportado por AEM que administra la aplicación. Copie la salida JSON.

1. Pegar la salida JSON del paso anterior en el archivo `mock.model.json`.

   ![Archivo Json de Modelo de Falsificación](./assets/integrate-spa/mock-model-json-created.png)

1. Abra el archivo . `index.html` at `ui.frontend/public/index.html`. Actualice la propiedad de metadatos del modelo de página AEM para que apunte a una variable `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Uso de una variable para el valor de la variable `cq:pagemodel_root_url` hará que sea más fácil alternar entre el proxy y el modelo json de maqueta.

1. Abra el archivo . `ui.frontend/.env.development` y realice las siguientes actualizaciones para comentar el valor anterior de `REACT_APP_PAGE_MODEL_PATH` y `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. Si se está ejecutando, detenga la variable **webpack-dev-server**. Inicie el **webpack-dev-server** desde el terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) y debería ver el SPA con el mismo contenido utilizado en la variable **proxy** json.

1. Realice un pequeño cambio en la variable `mock.model.json` archivo creado anteriormente. Debería ver el contenido actualizado inmediatamente reflejado en el **webpack-dev-server**.

   ![mock model json update](./assets/integrate-spa/webpack-mock-model.gif)

Poder manipular el modelo JSON y ver los efectos en un SPA activo puede ayudar a un desarrollador a comprender la API del modelo JSON. También permite el desarrollo del front-end y del back-end en paralelo.

Ahora puede alternar dónde consumir el contenido JSON alternando las entradas en la variable `env.development` archivo:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
