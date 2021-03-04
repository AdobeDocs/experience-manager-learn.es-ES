---
title: Integración de un SPA | Introducción al Editor de SPA de AEM y React
description: Comprenda cómo se puede integrar el código fuente de una aplicación de una sola página (SPA) escrita en React con un proyecto de Adobe Experience Manager (AEM). Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de webpack, para desarrollar rápidamente la SPA con la API del modelo JSON de AEM.
sub-product: sitios
feature: Editor SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2109'
ht-degree: 1%

---


# Integración de un SPA {#integrate-spa}

Comprenda cómo se puede integrar el código fuente de una aplicación de una sola página (SPA) escrita en React con un proyecto de Adobe Experience Manager (AEM). Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de webpack, para desarrollar rápidamente la SPA con la API del modelo JSON de AEM.

## Objetivo

1. Comprenda cómo el proyecto SPA está integrado con AEM con bibliotecas del lado del cliente.
2. Aprenda a utilizar un servidor de desarrollo de webpack para el desarrollo de front-end dedicado.
3. Explore el uso de un **proxy** y un archivo **de prueba** estático para desarrollar con la API del modelo JSON de AEM

## Qué va a generar

En este capítulo se agregará un componente simple `Header` a la SPA. En el proceso de creación de este `Header` componente estático se utilizarán varios enfoques para el desarrollo de SPA de AEM.

![Nuevo encabezado en AEM](./assets/integrate-spa/final-header-component.png)

*La SPA se amplía para añadir un  `Header` componente estático*

## Requisitos previos

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtención del código

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Implemente el código base en una instancia local de AEM mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si utiliza [AEM 6.x](overview.md#compatibility), añada el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) o extraer el código localmente cambiando a la rama `React/integrate-spa-solution`.

## Enfoque de integración {#integration-approach}

Se crearon dos módulos como parte del proyecto AEM: `ui.apps` y `ui.frontend`.

El módulo `ui.frontend` es un proyecto [webpack](https://webpack.js.org/) que contiene todo el código fuente de SPA. La mayoría del desarrollo y las pruebas de SPA se realizarán en el proyecto de webpack. Cuando se activa una compilación de producción, la SPA se crea y compila mediante un webpack. Los artefactos compilados (CSS y Javascript) se copian en el módulo `ui.apps` que luego se implementa en el tiempo de ejecución de AEM.

![arquitectura de alto nivel ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Descripción general de la integración de SPA.*

La información adicional sobre la compilación del front-end se puede [encontrar aquí](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspeccione la integración de SPA {#inspect-spa-integration}

A continuación, inspeccione el módulo `ui.frontend` para comprender el SPA que ha generado automáticamente el [tipo de archivo del proyecto AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. En el IDE que elija, abra el proyecto AEM para el SPA WKND. Este tutorial utilizará el [IDE de código de Visual Studio](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - Proyecto SPA WKND de AEM](./assets/integrate-spa/vscode-ide-openproject.png)

2. Expanda e inspeccione la carpeta `ui.frontend` . Abra el archivo `ui.frontend/package.json`

3. En `dependencies` debería ver varios relacionados con `react` incluido `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   El `ui.frontend` es una aplicación React basada en el [Create React App](https://create-react-app.dev/) o CRA para abreviar. La versión `react-scripts` indica qué versión de CRA se utiliza.

4. También hay tres dependencias con el prefijo `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   Los módulos anteriores conforman el [SDK de JS del Editor de SPA de AEM](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) y proporcionan la funcionalidad para permitir asignar componentes de SPA a componentes de AEM.

5. En el archivo `package.json` hay varios `scripts` definidos:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Son scripts de compilación estándar que la aplicación Crear reacción hace [disponible](https://create-react-app.dev/docs/available-scripts).

   La única diferencia es la adición de `&& clientlib` al script `build`. Esta instrucción adicional es responsable de copiar el SPA compilado en el módulo `ui.apps` como una biblioteca del lado del cliente durante una compilación.

   El módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) se utiliza para facilitar esto.

6. Inspeccione el archivo `ui.frontend/clientlib.config.js`. Este archivo de configuración lo utiliza [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar cómo generar la biblioteca de cliente.

7. Inspeccione el archivo `ui.frontend/pom.xml`. Este archivo transforma la carpeta `ui.frontend` en un [módulo Maven](http://maven.apache.org/guides/mini/guide-multiple-modules.html). El archivo `pom.xml` se ha actualizado para que utilice el [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) para **test** y la **compilación** de la SPA durante una compilación de Maven.

8. Inspeccione el archivo `index.js` en `ui.frontend/src/index.js`:

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

   `index.js` es el punto de entrada de la SPA. `ModelManager` es proporcionado por el SDK de JS del Editor de SPA de AEM. Es responsable de llamar e inyectar el `pageModel` (el contenido JSON) en la aplicación.

## Añadir un componente de encabezado {#header-component}

A continuación, añada un nuevo componente a la SPA e implemente los cambios en una instancia local de AEM.

1. En el módulo `ui.frontend` , debajo de `ui.frontend/src/components` cree una nueva carpeta denominada `Header`.
2. Cree un archivo con el nombre `Header.js` debajo de la carpeta `Header`.

   ![Carpeta y archivo de encabezado](assets/integrate-spa/header-folder-js.png)

3. Rellene `Header.js` con lo siguiente:

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

4. Abra el archivo `ui.frontend/src/App.js`. Este es el punto de entrada de la aplicación.
5. Realice las siguientes actualizaciones en `App.js` para incluir el `Header` estático:

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

6. Abra un nuevo terminal y vaya a la carpeta `ui.frontend` y ejecute el comando `npm run build`:

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

7. Vaya a la carpeta `ui.apps` . Debajo de `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` debe ver que los archivos SPA compilados se han copiado de la carpeta`ui.frontend/build`.

   ![Biblioteca de cliente generada en ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

8. Vuelva al terminal y vaya a la carpeta `ui.apps` . Ejecute el siguiente comando Maven:

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

   Esto implementará el paquete `ui.apps` en una instancia local en ejecución de AEM.

9. Abra una pestaña del explorador y vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Ahora debería ver el contenido del componente `Header` que se muestra en la SPA.

   ![Implementación inicial del encabezado](./assets/integrate-spa/initial-header-implementation.png)

   Los pasos 6-8 se ejecutan automáticamente al activar una compilación de Maven desde la raíz del proyecto (es decir, `mvn clean install -PautoInstallSinglePackage`). Ahora debe comprender los conceptos básicos de la integración entre las bibliotecas de SPA y AEM del lado del cliente. Observe que aún puede editar y añadir `Text` componentes en AEM debajo del componente estático `Header`.

## Servidor de desarrollo de Webpack: proxy de la API JSON {#proxy-json}

Como se ha visto en ejercicios anteriores, realizar una compilación y sincronizar la biblioteca del cliente con una instancia local de AEM tarda unos minutos. Esto es aceptable para las pruebas finales, pero no es ideal para la mayoría del desarrollo de SPA.

Se puede utilizar un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desarrollar rápidamente el SPA. La SPA está impulsada por un modelo JSON generado por AEM. En este ejercicio, el contenido JSON de una instancia en ejecución de AEM será **proxy** en el servidor de desarrollo.

1. Vuelva al IDE y abra el archivo `ui.frontend/package.json`.

   Busque una línea como la siguiente:

   ```json
   "proxy": "http://localhost:4502",
   ```

   La [Crear aplicación de React](https://create-react-app.dev/docs/proxying-api-requests-in-development) proporciona un mecanismo fácil para las solicitudes de API de proxy. Todas las solicitudes desconocidas se procesarán como proxy mediante `localhost:4502`, el inicio rápido local de AEM.

2. Abra una ventana de terminal y vaya a la carpeta `ui.frontend` . Ejecute el comando `npm start`:

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

3. Abra una nueva pestaña del explorador (si aún no está abierta) y vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Servidor de desarrollo de Webpack: json proxy](./assets/integrate-spa/webpack-dev-server-1.png)

   Debería ver el mismo contenido que en AEM, pero sin ninguna de las capacidades de creación habilitadas.

   >[!NOTE]
   >
   > Debido a los requisitos de seguridad de AEM, deberá iniciar sesión en la instancia local de AEM (http://localhost:4502) en el mismo navegador, pero en una pestaña diferente.

4. Vuelva al IDE y cree una nueva carpeta denominada `media` en `ui.frontend/src/media`.
5. Descargue y añada el siguiente logotipo WKND a la carpeta `media` :

   ![Logotipo de WKND](./assets/integrate-spa/wknd-logo-dk.png)

6. Abra `Header.js` en `ui.frontend/src/components/Header/Header.js` e importe el logotipo:

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. Realice las siguientes actualizaciones en `Header.js` para incluir el logotipo como parte del encabezado:

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   Guarde los cambios en `Header.js`.

8. Vuelva al explorador en [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Debería ver inmediatamente los cambios realizados en la aplicación.

   ![Logotipo añadido al encabezado](./assets/integrate-spa/added-logo-localhost.png)

   Puede seguir realizando actualizaciones de contenido en AEM y verlas reflejadas en **webpack-dev-server**, ya que estamos proxizando el contenido.

9. Detenga el servidor de desarrollo de webpack con `ctrl+c` en el terminal.

## Servidor de desarrollo de Webpack: API de JSON de prueba {#mock-json}

Otro enfoque para el desarrollo rápido es utilizar un archivo JSON estático para actuar como el modelo JSON. Al &quot;burlarse&quot; del JSON, se elimina la dependencia de una instancia local de AEM. También permite a un desarrollador front-end actualizar el modelo JSON para probar la funcionalidad y dirigir los cambios a la API JSON que luego sería implementada por un desarrollador back-end.

La configuración inicial del JSON de prueba **requiere una instancia local de AEM**.

1. En el explorador, vaya a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Este es el JSON exportado por AEM que dirige la aplicación. Copie la salida JSON.

2. Vuelva al IDE, vaya a `ui.frontend/public` y añada una nueva carpeta denominada `mock-content`.
3. Cree un nuevo archivo con el nombre `mock.model.json` debajo de `ui.frontend/public/mock-content`. Pegue el resultado JSON del **paso 1** aquí.

   ![Archivo Json de Modelo de Falsificación](./assets/integrate-spa/mock-model-json-created.png)

4. Abra el archivo `index.html` en `ui.frontend/public/index.html`. Actualice la propiedad de metadatos para el modelo de página de AEM para que apunte a una variable `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   El uso de una variable para el valor del `cq:pagemodel_root_url` facilitará la alternancia entre el proxy y el modelo json de prueba.

5. Abra el archivo `ui.frontend/.env.development` y realice las siguientes actualizaciones para comentar el valor anterior de `REACT_APP_PAGE_MODEL_PATH`:

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. Si se está ejecutando, detenga el **webpack-dev-server**. Inicie el **webpack-dev-server** desde el terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Vaya a [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) y debería ver el SPA con el mismo contenido utilizado en el **proxy** json.

7. Realice un pequeño cambio en el archivo `mock.model.json` creado anteriormente. Debería ver el contenido actualizado inmediatamente reflejado en el **webpack-dev-server**.

   ![mock model json update](./assets/integrate-spa/webpack-mock-model.gif)

Poder manipular el modelo JSON y ver los efectos en una SPA activa puede ayudar a un desarrollador a comprender la API del modelo JSON. También permite el desarrollo del front-end y del back-end en paralelo.

Ahora puede alternar dónde consumir el contenido JSON alternando las entradas del archivo `env.development`:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Agregar estilos con Sass

Una práctica recomendada de React es mantener cada componente modular y autónomo. Una recomendación general es evitar reutilizar el mismo nombre de clase CSS entre componentes, lo que hace que el uso de preprocesadores no sea tan potente. Este proyecto usará [Sass](https://sass-lang.com/) para algunas funciones útiles como variables. Este proyecto también seguirá las [convenciones de nomenclatura SUIT CSS](https://github.com/suitcss/suit/blob/master/doc/components.md) de forma flexible. SUIT es una variación de la notación BEM, Modificador de elementos de bloque, que se utiliza para crear reglas CSS coherentes.

1. Abra una ventana de terminal y detenga el **webpack-dev-server** si se inicia. Desde la carpeta `ui.frontend` introduzca el siguiente comando para [instalar Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet):

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   Instale `sass` como dependencia del mismo nivel:

   ```shell
   $ npm install sass --save
   ```

2. Instale `normalize-scss` para normalizar los estilos en los navegadores:

   ```shell
   $ npm install normalize-scss
   ```

3. Inicie **webpack-dev-server** para que podamos ver los estilos actualizándose en tiempo real:

   ```shell
   $ npm start
   ```

   Utilice el método Proxy of Mock para gestionar la API del modelo JSON.

4. Vuelva al IDE y debajo de `ui.frontend/src` cree una nueva carpeta denominada `styles`.
5. Cree un nuevo archivo debajo de `ui.frontend/src/styles` llamado `_variables.scss` y rellénelo con las siguientes variables:

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. Cambie el nombre de la extensión del archivo `index.css` de `ui.frontend/src/index.css` a **`index.scss`**. Sustituya el contenido por el siguiente:

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. Actualice `ui.frontend/src/index.js` para incluir el nuevo nombre `index.scss`:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. Cree un nuevo archivo con el nombre `Header.scss` debajo de `ui.frontend/src/components/Header`. Rellene el archivo con lo siguiente:

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. Incluir `Header.scss` actualizando `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. Vuelva al explorador y al **webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Encabezado con estilo: servidor de desarrollo de webpack](./assets/integrate-spa/styled-header.png)

   Ahora debería ver los estilos actualizados agregados al componente `Header`.

## Implementación de actualizaciones de SPA en AEM

Los cambios realizados en `Header` actualmente solo son visibles a través del **webpack-dev-server**. Implemente la SPA actualizada en AEM para ver los cambios.

1. Vaya a la raíz del proyecto (`aem-guides-wknd-spa`) e implemente el proyecto en AEM mediante Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Debería ver el `Header` actualizado con el logotipo y los estilos aplicados.

   ![Encabezado actualizado en AEM](./assets/integrate-spa/final-header-component.png)

   Ahora que la SPA actualizada está en AEM, la creación puede continuar.

## Solución de problemas de error de nodo

Durante el desarrollo puede encontrar el siguiente error:

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

Esto puede ocurrir cuando la versión local de **Node.js** y **npm** son diferentes a las que utiliza el [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin). La ejecución del comando `npm rebuild node-sass` puede solucionar temporalmente el problema o quitar la carpeta `ui.frontend/node_modules` y volver a instalar.

También hay algunas maneras de abordar esto de manera más permanente.

* Asegúrese de que la versión local de npm y Node.js coincide con las versiones utilizadas por la [compilación de Maven](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* Añada el siguiente paso de ejecución al `ui.frontend/pom.xml` antes del paso `npm run build`:

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## Felicitaciones! {#congratulations}

¡Felicidades, ha actualizado la SPA y ha explorado la integración con AEM! Ahora conoce dos enfoques diferentes para desarrollar la SPA con la API del modelo JSON de AEM mediante un **webpack-dev-server**.

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) o extraer el código localmente cambiando a la rama `React/integrate-spa-solution`.

### Pasos siguientes {#next-steps}

[Asignación de componentes de SPA a componentes de AEM](map-components.md) : Aprenda a asignar componentes de React a componentes de Adobe Experience Manager (AEM) con el SDK de JS del Editor de SPA de AEM. La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas de los componentes de SPA dentro del Editor de SPA de AEM, de forma similar a la creación tradicional de AEM.
