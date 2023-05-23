---
title: SPA Integración de una AEM SPA | Introducción al Editor de y React
description: SPA Comprenda cómo el código fuente de una aplicación de una sola página () escrita en React se puede integrar en un proyecto de Adobe Experience Manager AEM (). SPA AEM Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de Webpack, para desarrollar rápidamente la contra la API del modelo JSON de la red de aplicaciones de la red de aplicaciones de JSON de la red de aplicaciones de la red (JSON) de la red de archivos de datos.
feature: SPA Editor
version: Cloud Service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
source-git-commit: c34c27955dbc084620ac4dd811ba4051ea83f447
workflow-type: tm+mt
source-wordcount: '1835'
ht-degree: 1%

---

# SPA Integración de la {#developer-workflow}

SPA Comprenda cómo el código fuente de una aplicación de una sola página () escrita en React se puede integrar en un proyecto de Adobe Experience Manager AEM (). SPA AEM Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de Webpack, para desarrollar rápidamente la contra la API del modelo JSON de la red de aplicaciones de la red de aplicaciones de JSON de la red de aplicaciones de la red (JSON) de la red de archivos de datos.

## Objetivo

1. SPA AEM Comprender cómo el proyecto de está integrado con las bibliotecas del lado del cliente y la forma en que lo hace.
2. Aprenda a utilizar un servidor de desarrollo de Webpack para el desarrollo front-end dedicado.
3. Explorar el uso de un **proxy** y estático **burlar de** AEM para desarrollar con la API del modelo JSON de.

## Qué va a generar

SPA AEM En este capítulo, realizará varios cambios pequeños en la para comprender cómo se integra con los recursos de la interfaz de usuario de.
Este capítulo añadirá un `Header` SPA componente a la. En el proceso de construcción de esto **estático** `Header` AEM SPA componente se utilizan varios enfoques para el desarrollo de la.

![AEM Nuevo encabezado en el](./assets/integrate-spa/final-header-component.png)

*SPA La se amplía para agregar una estática `Header` componente*

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment). Este capítulo es una continuación de la [Crear proyecto](create-project.md) SPA AEM , sin embargo, para seguir todo lo que necesita es un proyecto de trabajo que funcione y que esté habilitado para el uso de la.

## Enfoque de integración {#integration-approach}

AEM Se crearon dos módulos como parte del proyecto de: `ui.apps` y `ui.frontend`.

El `ui.frontend` El módulo es un [webpack](https://webpack.js.org/) SPA proyecto que contiene todo el código fuente de la. SPA La mayoría de las pruebas y el desarrollo de la se realizan en el proyecto de Webpack. SPA Cuando se activa una compilación de producción, la generación de la se compila mediante Webpack. Los artefactos compilados (CSS y Javascript) se copian en la variable `ui.apps` AEM que luego se implementa en el tiempo de ejecución de la.

![arquitectura de alto nivel ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*SPA Una descripción de alto nivel de la integración de la.*

Puede obtener información adicional sobre la versión del front-end [encontrado aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect SPA la integración de la {#inspect-spa-integration}

A continuación, inspeccione el `ui.frontend` SPA para comprender la que ha generado automáticamente el módulo [AEM Arquetipo de proyecto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. AEM En el IDE de su elección, abra el proyecto de. Este tutorial utilizará el [IDE de código de Visual Studio](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![AEM SPA VSCode - Proyecto de de WKND](./assets/integrate-spa/vscode-ide-openproject.png)

1. Expanda e inspeccione el `ui.frontend` carpeta. Abra el archivo `ui.frontend/package.json`

1. En el `dependencies` debería ver varias relacionadas con `react` incluyendo `react-scripts`

   El `ui.frontend` es una aplicación de React basada en [Crear aplicación de React](https://create-react-app.dev/) o CRA para abreviar. El `react-scripts` version indica qué versión de CRA se utiliza.

1. También hay varias dependencias con el prefijo `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   Los módulos anteriores conforman el [AEM SPA SDK de JS de Editor de](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) SPA AEM y proporcionan la funcionalidad para hacer posible la asignación de componentes de la a componentes de la.

   También se incluyen [AEM Componentes de WCM: implementación principal de React](https://github.com/adobe/aem-react-core-wcm-components-base) y [AEM Componentes de WCM - Editor de spa - Implementación principal de React](https://github.com/adobe/aem-react-core-wcm-components-spa). AEM Se trata de un conjunto de componentes de la interfaz de usuario reutilizables que se asignan a componentes de la interfaz de usuario listos para usar. Están diseñadas para utilizarse tal cual y para satisfacer las necesidades de su proyecto.

1. En el `package.json` archivo hay varios `scripts` definido:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Se trata de scripts de compilación estándar realizados [disponible](https://create-react-app.dev/docs/available-scripts) mediante Crear aplicación de React.

   La única diferencia es la adición de `&& clientlib` a la `build` script. SPA Esta instrucción adicional es responsable de copiar los datos compilados en el archivo de comandos de la interfaz de usuario de la interfaz de usuario de `ui.apps` como una biblioteca del lado del cliente durante una compilación.

   El módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) se utiliza para facilitar esto.

1. Inspect el archivo `ui.frontend/clientlib.config.js`. Este archivo de configuración lo utiliza [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar cómo generar la biblioteca de cliente.

1. Inspect el archivo `ui.frontend/pom.xml`. Este archivo transforma el `ui.frontend` carpeta en una [módulo Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). El `pom.xml` se ha actualizado el archivo para utilizar el [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) hasta **prueba** y **generar** SPA la configuración de la aplicación durante una compilación de Maven.

1. Inspect el archivo `index.js` en `ui.frontend/src/index.js`:

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

   `index.js` SPA es el punto de entrada de la. `ModelManager` AEM SPA es proporcionado por el SDK de JS de Editor de. Es responsable de llamar e inyectar al `pageModel` (el contenido JSON) en la aplicación.

1. Inspect el archivo `import-components.js` en `ui.frontend/src/components/import-components.js`. Este archivo importa el de forma predeterminada **Componentes principales de React** y los pone a disposición del proyecto. AEM SPA En el siguiente capítulo analizaremos la asignación de contenido de la a componentes de la.

## SPA Añadir un componente de estático {#static-spa-component}

SPA AEM A continuación, añada un nuevo componente a la e implemente los cambios en una instancia local de. SPA Este es un cambio sencillo, solo para ilustrar cómo se actualiza la.

1. En el `ui.frontend` módulo, debajo de `ui.frontend/src/components` cree una nueva carpeta con el nombre `Header`.
1. Cree un archivo llamado `Header.js` debajo de `Header` carpeta.

   ![Encabezado de carpeta y archivo](assets/create-project/header-folder-js.png)

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

   Arriba se encuentra un componente React estándar que generará una cadena de texto estático.

1. Abra el archivo `ui.frontend/src/App.js`. Este es el punto de entrada de la aplicación.
1. Realice las siguientes actualizaciones en `App.js` para incluir la estática `Header`:

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

1. Abra un terminal nuevo y navegue hasta el `ui.frontend` y ejecute el `npm run build` comando:

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

1. Navegue hasta la carpeta `ui.apps`. Debajo `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` SPA debería ver que los archivos compilados de la se han copiado desde el`ui.frontend/build` carpeta.

   ![Biblioteca de cliente generada en ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

1. Vuelva a la terminal y navegue hasta la `ui.apps` carpeta. Ejecute el siguiente comando Maven:

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

   Esto implementará el `ui.apps` AEM a una instancia local en ejecución de la aplicación de la aplicación de la.

1. Abra una pestaña del explorador y vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Ahora debería ver el contenido de la `Header` SPA componente que se muestra en la lista de componentes de.

   ![Implementación del encabezado inicial](./assets/integrate-spa/initial-header-implementation.png)

   Los pasos anteriores se ejecutan automáticamente al activar una compilación de Maven desde la raíz del proyecto (es decir, `mvn clean install -PautoInstallSinglePackage`). SPA AEM Ahora debería comprender los conceptos básicos de la integración entre las bibliotecas del lado del cliente de la y la de la segmentación de datos Observe que aún puede editar y agregar `Text` AEM componentes en la zona de la red bajo el elemento estático `Header` componente.

## Servidor de desarrollo de Webpack: proxy de la API de JSON {#proxy-json}

AEM Como se ha visto en los ejercicios anteriores, llevar a cabo una compilación y sincronizar la biblioteca de cliente con una instancia local de lleva unos minutos. SPA Esto es aceptable para las pruebas finales, pero no es ideal para la mayoría del desarrollo de la.

A [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) SPA se puede utilizar para desarrollar rápidamente la. SPA AEM La está impulsada por un modelo JSON generado por el grupo de informes de. AEM En este ejercicio, el contenido JSON de una instancia en ejecución de es el siguiente **en proxy** en el servidor de desarrollo.

1. Vuelva al IDE y abra el archivo `ui.frontend/package.json`.

   Busque una línea como la siguiente:

   ```json
   "proxy": "http://localhost:4502",
   ```

   El [Crear aplicación de React](https://create-react-app.dev/docs/proxying-api-requests-in-development) proporciona un mecanismo fácil para las solicitudes de API de proxy. Todas las solicitudes desconocidas se procesan como proxy mediante `localhost:4502`AEM , el inicio rápido local de la.

1. Abra una ventana de terminal y vaya a `ui.frontend` carpeta. Ejecute el comando `npm start`:

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

1. Abra una nueva pestaña del explorador (si no está abierta todavía) y vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Servidor de desarrollo de Webpack: json proxy](./assets/integrate-spa/webpack-dev-server-1.png)

   AEM Debería ver el mismo contenido que en el caso de los recursos de creación, pero sin ninguna de las capacidades de creación habilitadas.

   >[!NOTE]
   >
   > AEM AEM Debido a los requisitos de seguridad de la, deberá iniciar sesión en la instancia de local (http://localhost:4502) en el mismo explorador, pero en una pestaña diferente.

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

1. Volver a abrir `Header.js` y agregue la siguiente línea a la que hacer referencia `Header.css`:

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

   ![Estilo añadido al encabezado](assets/integrate-spa/added-logo-localhost.png)

   AEM Puede seguir realizando actualizaciones de contenido en los segmentos de y ver cómo se reflejan en las actualizaciones de contenido de los segmentos de **webpack-dev-server**, ya que estamos procesando el contenido.

1. Detenga el servidor de desarrollo de Webpack con `ctrl+c` en la terminal.

## SPA AEM Implementar actualizaciones de la en el

Los cambios realizados en el `Header` actualmente solo son visibles a través de **webpack-dev-server**. SPA AEM Implemente el actualizado para ver los cambios que se han producido.

1. Vaya a la raíz del proyecto (`aem-guides-wknd-spa`AEM ) e implemente el proyecto para que se pueda usar de manera más eficaz para el uso de Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Debería ver el informe actualizado `Header` y estilos aplicados.

   ![AEM Se ha actualizado el encabezado en](assets/integrate-spa/final-header-component.png)

   SPA AEM Ahora que la actualización de la se encuentra en la fase de creación, la creación puede continuar.

## Enhorabuena. {#congratulations}

SPA AEM ¡Enhorabuena, ha actualizado la y ha explorado la integración con la opción de integración de! SPA AEM Ahora sabe cómo desarrollar la contra la API del modelo JSON de la aplicación de datos de la aplicación de datos de la aplicación de modo que utilice una **webpack-dev-server**.

### Pasos siguientes {#next-steps}

[SPA AEM Asignación de componentes de a componentes de](map-components.md) : Obtenga información sobre cómo asignar componentes de React a componentes de Adobe Experience Manager AEM AEM SPA () con el SDK de JS de Editor de. SPA AEM SPA AEM La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas en los componentes de la de componentes del Editor de componentes, de forma similar a la creación tradicional de los componentes de la.

## (Bonus) Servidor de desarrollo de Webpack: simulación de API de JSON {#mock-json}

Otro enfoque para el desarrollo rápido es utilizar un archivo JSON estático para actuar como modelo JSON. AEM Al &quot;burlarse&quot; del JSON, eliminamos la dependencia de una instancia local de. También permite a un desarrollador front-end actualizar el modelo JSON para probar la funcionalidad y dirigir cambios en la API JSON que luego implementaría un desarrollador back-end.

La configuración inicial del JSON de prueba sí **AEM requiere una instancia de local**.

1. Vuelva al IDE y navegue hasta `ui.frontend/public` y añada una nueva carpeta denominada `mock-content`.
1. Cree un nuevo archivo con el nombre `mock.model.json` debajo de `ui.frontend/public/mock-content`.
1. En el explorador, vaya a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   AEM Este es el JSON exportado por el usuario que está impulsando la aplicación. Copie la salida JSON.

1. Pegue la salida JSON del paso anterior en el archivo `mock.model.json`.

   ![Archivo Json de modelo de simulación](./assets/integrate-spa/mock-model-json-created.png)

1. Abra el archivo `index.html` en `ui.frontend/public/index.html`. AEM Actualice la propiedad de metadatos para el modelo de página de la página de datos para que apunte a una variable `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Usar una variable para el valor de `cq:pagemodel_root_url` facilitará el cambio entre el proxy y el modelo json de prueba.

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

1. Si se está ejecutando, detenga el **webpack-dev-server**. Inicie el **webpack-dev-server** desde el terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) SPA y debería ver el contenido con el mismo que se utiliza en la. **proxy** json.

1. Realice un pequeño cambio en la `mock.model.json` archivo creado anteriormente. Debería ver el contenido actualizado inmediatamente reflejado en la **webpack-dev-server**.

   ![actualización json del modelo de simulación](./assets/integrate-spa/webpack-mock-model.gif)

SPA La posibilidad de manipular el modelo JSON y ver los efectos en un modelo en directo puede ayudar a un desarrollador a comprender la API del modelo JSON. También permite que el desarrollo del front-end y del back-end se produzca en paralelo.

Ahora puede alternar dónde consumir el contenido JSON alternando las entradas en la `env.development` archivo:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
