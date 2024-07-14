---
title: SPA Integración de una | AEM SPA Introducción al Editor de y React
description: SPA Comprenda cómo el código fuente de una aplicación de una sola página () escrita en React se puede integrar en un proyecto de Adobe Experience Manager AEM (). SPA AEM Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de Webpack, para desarrollar rápidamente la contra la API del modelo JSON de la red de aplicaciones de la red de aplicaciones de JSON de la red de aplicaciones de la red (JSON) de la red de archivos de datos.
feature: SPA Editor
version: Cloud Service
jira: KT-4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
duration: 409
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1689'
ht-degree: 1%

---

# SPA Integración de la {#developer-workflow}

SPA Comprenda cómo el código fuente de una aplicación de una sola página () escrita en React se puede integrar en un proyecto de Adobe Experience Manager AEM (). SPA AEM Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de Webpack, para desarrollar rápidamente la contra la API del modelo JSON de la red de aplicaciones de la red de aplicaciones de JSON de la red de aplicaciones de la red (JSON) de la red de archivos de datos.

## Objetivo

1. SPA AEM Comprender cómo el proyecto de está integrado con las bibliotecas del lado del cliente y la forma en que lo hace.
2. Aprenda a utilizar un servidor de desarrollo de Webpack para el desarrollo front-end dedicado.
3. AEM Explore el uso de un **proxy** y un archivo **mock** estático para desarrollar con la API del modelo JSON de la.

## Qué va a generar

SPA AEM En este capítulo, realizará varios cambios pequeños en la para comprender cómo se integra con los recursos de la interfaz de usuario de.
SPA Este capítulo agregará un componente `Header` simple a la. AEM SPA En el proceso de crear este componente **static** `Header` se usan varios enfoques para el desarrollo de la.

AEM ![Nuevo encabezado en el código de tiempo de la aplicación](./assets/integrate-spa/final-header-component.png)

SPA *La se ha ampliado para agregar un componente `Header` estático*

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). SPA AEM Este capítulo es una continuación del capítulo [Crear proyecto](create-project.md); sin embargo, para continuar, todo lo que necesita es un proyecto de trabajo habilitado para la creación de proyectos que se encuentre en la fase de creación de un proyecto que funcione y que esté habilitado para la creación de proyectos.

## Enfoque de integración {#integration-approach}

AEM Se han creado dos módulos como parte del proyecto de: `ui.apps` y `ui.frontend`.

SPA El módulo `ui.frontend` es un proyecto [webpack](https://webpack.js.org/) que contiene todo el código fuente de la. SPA La mayoría de las pruebas y el desarrollo de la se realizan en el proyecto de Webpack. SPA Cuando se activa una compilación de producción, la generación de la se compila mediante Webpack. AEM Los artefactos compilados (CSS y Javascript) se copian en el módulo `ui.apps`, que luego se implementa en el tiempo de ejecución de la.

![arquitectura de alto nivel ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

SPA *Representación de alto nivel de la integración de la.*

Encontrará información adicional sobre la versión del front-end [aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect SPA la integración de la {#inspect-spa-integration}

SPA AEM A continuación, inspeccione el módulo `ui.frontend` para comprender el tipo de archivo del proyecto [que ha generado automáticamente el módulo de datos de tipo de archivo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. AEM En el IDE de su elección, abra el proyecto de. Este tutorial usará el [IDE de código de Visual Studio](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   AEM SPA ![VSCode - Proyecto de de WKND de de 1}](./assets/integrate-spa/vscode-ide-openproject.png)

1. Expanda e inspeccione la carpeta `ui.frontend`. Abrir el archivo `ui.frontend/package.json`

1. En `dependencies` debería ver varios elementos relacionados con `react`, incluido `react-scripts`

   `ui.frontend` es una aplicación de React basada en [Crear aplicación de React](https://create-react-app.dev/) o CRA para abreviar. La versión `react-scripts` indica qué versión de CRA se utiliza.

1. También hay varias dependencias con el prefijo `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   AEM SPA SPA AEM Los módulos anteriores conforman el [SDK de JS de Editor de](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) y proporcionan la funcionalidad para que sea posible asignar componentes de la a componentes de la aplicación.

   AEM AEM También se incluyen [Componentes de WCM - React Core implementation](https://github.com/adobe/aem-react-core-wcm-components-base) y [Componentes de WCM - Editor de spa - React Core implementation](https://github.com/adobe/aem-react-core-wcm-components-spa). AEM Se trata de un conjunto de componentes de la interfaz de usuario reutilizables que se asignan a componentes de la interfaz de usuario listos para usar. Están diseñadas para utilizarse tal cual y para satisfacer las necesidades de su proyecto.

1. En el archivo `package.json` hay varios `scripts` definidos:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Son scripts de compilación estándar [disponibles](https://create-react-app.dev/docs/available-scripts) mediante la aplicación Crear React.

   La única diferencia es la adición de `&& clientlib` al script `build`. SPA Esta instrucción adicional es responsable de copiar los datos compilados en el módulo `ui.apps` como una biblioteca del lado del cliente durante una compilación.

   El módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) se usa para facilitar esto.

1. Inspect el archivo `ui.frontend/clientlib.config.js`. [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) utiliza este archivo de configuración para determinar cómo generar la biblioteca de cliente.

1. Inspect el archivo `ui.frontend/pom.xml`. Este archivo transforma la carpeta `ui.frontend` en un [módulo Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). SPA El archivo `pom.xml` se ha actualizado para usar el [complemento frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) para **probar** y **compilar** el durante una compilación de Maven.

1. Inspect cambió el archivo `index.js` a las `ui.frontend/src/index.js`:

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

   SPA `index.js` es el punto de entrada de la. AEM SPA `ModelManager` lo proporciona el SDK de JS del Editor de JS de la interfaz de usuario de. Es responsable de llamar a `pageModel` (el contenido JSON) y de inyectarlo en la aplicación.

1. Inspect cambió el archivo `import-components.js` a las `ui.frontend/src/components/import-components.js`. Este archivo importa los **componentes principales de React** predeterminados y los pone a disposición del proyecto. AEM SPA En el siguiente capítulo analizaremos la asignación de contenido de la a componentes de la.

## SPA Añadir un componente de estático {#static-spa-component}

SPA AEM A continuación, añada un nuevo componente a la e implemente los cambios en una instancia local de. SPA Este es un cambio sencillo, solo para ilustrar cómo se actualiza la.

1. En el módulo `ui.frontend`, debajo de `ui.frontend/src/components`, cree una nueva carpeta denominada `Header`.
1. Cree un archivo con el nombre `Header.js` debajo de la carpeta `Header`.

   ![Carpeta de encabezado y archivo](assets/create-project/header-folder-js.png)

1. Rellene `Header.js` con lo siguiente:

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

   Arriba se encuentra un componente React estándar que generará una cadena de texto estático.

1. Abra el archivo `ui.frontend/src/App.js`. Este es el punto de entrada de la aplicación.
1. Realice las siguientes actualizaciones en `App.js` para incluir la `Header` estática:

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

1. Abra un nuevo terminal, vaya a la carpeta `ui.frontend` y ejecute el comando `npm run build`:

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

1. Navegue hasta la carpeta `ui.apps`. SPA Debajo de `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` debería ver que los archivos compilados de la `ui.frontend/build` se han copiado de la carpeta.

   ![Biblioteca de cliente generada en ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

1. Vuelva al terminal y navegue hasta la carpeta `ui.apps`. Ejecute el siguiente comando Maven:

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

   AEM Esto implementará el paquete `ui.apps` en una instancia local en ejecución de la aplicación de la aplicación de la.

1. Abra una ficha del explorador y vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). SPA Ahora debería ver el contenido del componente `Header` que se muestra en el cuadro de diálogo de elementos de la interfaz de usuario de.

   ![Implementación inicial del encabezado](./assets/integrate-spa/initial-header-implementation.png)

   Los pasos anteriores se ejecutan automáticamente al activar una compilación de Maven desde la raíz del proyecto (es decir, `mvn clean install -PautoInstallSinglePackage`). SPA AEM Ahora debería comprender los conceptos básicos de la integración entre las bibliotecas del lado del cliente de la y la de la segmentación de datos AEM Observe que todavía puede editar y agregar `Text` componentes en el elemento de seguridad de la base de datos que se encuentra debajo del componente estático `Header`.

## Servidor de desarrollo de Webpack: proxy de la API de JSON {#proxy-json}

AEM Como se ha visto en los ejercicios anteriores, llevar a cabo una compilación y sincronizar la biblioteca de cliente con una instancia local de lleva unos minutos. SPA Esto es aceptable para las pruebas finales, pero no es ideal para la mayoría del desarrollo de la.

SPA Se puede usar [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desarrollar rápidamente el. SPA AEM La está impulsada por un modelo JSON generado por el grupo de informes de. AEM En este ejercicio, el contenido JSON de una instancia en ejecución de se **procesa como proxy** en el servidor de desarrollo.

1. Vuelva al IDE y abra el archivo `ui.frontend/package.json`.

   Busque una línea como la siguiente:

   ```json
   "proxy": "http://localhost:4502",
   ```

   [Crear aplicación React](https://create-react-app.dev/docs/proxying-api-requests-in-development) proporciona un mecanismo fácil para las solicitudes de API de proxy. AEM Todas las solicitudes desconocidas se procesan como proxy a través de `localhost:4502`, el inicio rápido de la aplicación local de la.

1. Abra una ventana de terminal y vaya a la carpeta `ui.frontend`. Ejecute el comando `npm start`:

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

1. Abra una nueva ficha de explorador (si no está abierta) y vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Servidor de desarrollo de Webpack - proxy json](./assets/integrate-spa/webpack-dev-server-1.png)

   AEM Debería ver el mismo contenido que en el caso de los recursos de creación, pero sin ninguna de las capacidades de creación habilitadas.

   >[!NOTE]
   >
   > AEM AEM Debido a los requisitos de seguridad de la, deberá iniciar sesión en la instancia de local (http://localhost:4502) en el mismo explorador, pero en una pestaña diferente.

1. Vuelva al IDE y cree un archivo con el nombre `Header.css` en la carpeta `src/components/Header`.
1. Rellene `Header.css` con lo siguiente:

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

1. Vuelva a abrir `Header.js` y agregue la línea siguiente para hacer referencia a `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   Guarde los cambios.

1. Vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) para ver los cambios de estilo reflejados automáticamente.

1. Abra el archivo `Page.css` en `ui.frontend/src/components/Page`. Realice los siguientes cambios para corregir el relleno:

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. Vuelva al explorador en [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Debería ver reflejados inmediatamente los cambios en la aplicación.

   Se agregó ![estilo al encabezado](assets/integrate-spa/added-logo-localhost.png)

   AEM Puede seguir haciendo actualizaciones de contenido en los segmentos de y ver cómo se reflejan en **webpack-dev-server**, ya que estamos procesando el contenido como proxy.

1. Detenga el servidor de desarrollo de Webpack con `ctrl+c` en el terminal.

## SPA AEM Implementar actualizaciones de la en el

Los cambios realizados en `Header` actualmente solo son visibles a través de **webpack-dev-server**. SPA AEM Implemente el actualizado para ver los cambios que se han producido.

1. AEM Vaya a la raíz del proyecto (`aem-guides-wknd-spa`) e implemente el proyecto para que se ejecute mediante Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Debería ver los `Header` actualizados y los estilos aplicados.

   AEM ![Encabezado actualizado en el código de tiempo ](assets/integrate-spa/final-header-component.png)

   SPA AEM Ahora que la actualización de la se encuentra en la fase de creación, la creación puede continuar.

## Enhorabuena. {#congratulations}

SPA AEM ¡Enhorabuena, ha actualizado la y ha explorado la integración con la opción de integración de! SPA AEM Ahora sabe cómo desarrollar la con la API del modelo JSON de la mediante **webpack-dev-server**.

### Siguientes pasos {#next-steps}

SPA AEM [Asignación de componentes de a componentes de](map-components.md). Obtenga información sobre cómo asignar componentes de React a componentes de Adobe Experience Manager AEM AEM SPA () con el SDK de JS de Editor de. SPA AEM SPA AEM La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas en los componentes de la de componentes del Editor de componentes, de forma similar a la creación tradicional de los componentes de la.

## (Bonus) Servidor de desarrollo de Webpack: simulación de API de JSON {#mock-json}

Otro enfoque para el desarrollo rápido es utilizar un archivo JSON estático para actuar como modelo JSON. AEM Al &quot;burlarse&quot; del JSON, eliminamos la dependencia de una instancia local de. También permite a un desarrollador front-end actualizar el modelo JSON para probar la funcionalidad y dirigir cambios en la API JSON que luego implementaría un desarrollador back-end.

AEM La configuración inicial del JSON ficticio **requiere una instancia de local**.

1. Vuelva al IDE, vaya a `ui.frontend/public` y agregue una nueva carpeta llamada `mock-content`.
1. Cree un nuevo archivo con el nombre `mock.model.json` debajo de `ui.frontend/public/mock-content`.
1. En el explorador, vaya a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   AEM Este es el JSON exportado por el usuario que está impulsando la aplicación. Copie la salida JSON.

1. Pegue la salida JSON del paso anterior en el archivo `mock.model.json`.

   ![Archivo Json de modelo ficticio](./assets/integrate-spa/mock-model-json-created.png)

1. Abra el archivo `index.html` en `ui.frontend/public/index.html`. AEM Actualice la propiedad de metadatos para el modelo de página de la página de datos para que apunte a una variable `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   El uso de una variable para el valor de `cq:pagemodel_root_url` hará que sea más fácil alternar entre el proxy y el modelo json ficticio.

1. Abra el archivo `ui.frontend/.env.development` y realice las siguientes actualizaciones para comentar el valor anterior de `REACT_APP_PAGE_MODEL_PATH` y `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. Si se está ejecutando, detenga **webpack-dev-server**. Inicie **webpack-dev-server** desde el terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   SPA Vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) y debería ver el código con el mismo contenido utilizado en el json **proxy**.

1. Realice un pequeño cambio en el archivo `mock.model.json` creado anteriormente. Debería ver el contenido actualizado que se refleja inmediatamente en **webpack-dev-server**.

   ![actualización json del modelo ficticio](./assets/integrate-spa/webpack-mock-model.gif)

SPA La posibilidad de manipular el modelo JSON y ver los efectos en un modelo en directo puede ayudar a un desarrollador a comprender la API del modelo JSON. También permite que el desarrollo del front-end y del back-end se produzca en paralelo.

Ahora puede cambiar dónde consumir el contenido JSON alternando las entradas del archivo `env.development`:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
