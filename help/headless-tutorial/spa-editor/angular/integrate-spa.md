---
title: SPA Integración de una | AEM SPA Introducción al Editor y al Angular de la
description: SPA Comprenda cómo el código fuente de una aplicación de una sola página () escrita en Angular se puede integrar con un proyecto de Adobe Experience Manager AEM (). Aprenda a utilizar herramientas front-end modernas, como la herramienta CLI de Angular SPA AEM, para desarrollar rápidamente la contra la API del modelo JSON de la versión de datos de la aplicación de código de tiempo de ejecución (JSON) de la versión de JSON de la versión de.
feature: SPA Editor
version: Cloud Service
jira: KT-5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: e9386885-86de-4e43-933c-2f0a2c04a2f2
duration: 536
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2045'
ht-degree: 0%

---

# SPA Integración de una {#integrate-spa}

SPA Comprenda cómo el código fuente de una aplicación de una sola página () escrita en Angular se puede integrar con un proyecto de Adobe Experience Manager AEM (). SPA AEM Aprenda a utilizar herramientas front-end modernas, como un servidor de desarrollo de Webpack, para desarrollar rápidamente la contra la API del modelo JSON de la red de aplicaciones de la red de aplicaciones de JSON de la red de aplicaciones de la red (JSON) de la red de archivos de datos.

## Objetivo

1. SPA AEM Comprender cómo el proyecto de está integrado con las bibliotecas del lado del cliente y la forma en que lo hace.
2. Aprenda a utilizar un servidor de desarrollo local para el desarrollo front-end dedicado.
3. AEM Explore el uso de un **proxy** y un archivo **mock** estático para desarrollar con la API del modelo JSON de la

## Qué va a generar

SPA Este capítulo agregará un componente `Header` simple a la. AEM SPA En el proceso de generar este componente `Header` estático se utilizan varios enfoques para el desarrollo de la.

AEM ![Nuevo encabezado en el código de tiempo de la aplicación](./assets/integrate-spa/final-header-component.png)

SPA *La se ha ampliado para agregar un componente `Header` estático*

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtener el código

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. AEM Implemente el código base en una instancia de local mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM Si usa [6.x](overview.md#compatibility), agregue el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) o desprotegerlo localmente cambiando a la rama `Angular/integrate-spa-solution`.

## Enfoque de integración {#integration-approach}

AEM Se han creado dos módulos como parte del proyecto de: `ui.apps` y `ui.frontend`.

SPA El módulo `ui.frontend` es un proyecto [webpack](https://webpack.js.org/) que contiene todo el código fuente de la. SPA La mayoría de las pruebas y el desarrollo de la se realizan en el proyecto de Webpack. SPA Cuando se activa una compilación de producción, la generación de la se compila mediante Webpack. AEM Los artefactos compilados (CSS y Javascript) se copian en el módulo `ui.apps`, que luego se implementa en el tiempo de ejecución de la.

![arquitectura de alto nivel ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

SPA *Representación de alto nivel de la integración de la.*

Encontrará información adicional sobre la versión del front-end [aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect SPA la integración de la {#inspect-spa-integration}

SPA AEM A continuación, inspeccione el módulo `ui.frontend` para comprender el tipo de archivo del proyecto [que ha generado automáticamente el módulo de datos de tipo de archivo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. AEM SPA En el IDE de su elección, abra el Proyecto de para el WKND. Este tutorial usará el [IDE de código de Visual Studio](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   AEM SPA ![VSCode - Proyecto de de WKND de de 1}](./assets/integrate-spa/vscode-ide-openproject.png)

2. Expanda e inspeccione la carpeta `ui.frontend`. Abrir el archivo `ui.frontend/package.json`

3. En `dependencies` debería ver varios elementos relacionados con `@angular`:

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   El módulo `ui.frontend` es una [aplicación de Angular](https://angular.io) generada mediante la [herramienta CLI de Angular](https://angular.io/cli) que incluye enrutamiento.

4. También hay tres dependencias con el prefijo `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   AEM SPA SPA AEM Los módulos anteriores conforman el [SDK de JS de Editor de](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) y proporcionan la funcionalidad para que sea posible asignar componentes de la a componentes de la aplicación.

5. En el archivo `package.json` se han definido varios `scripts`:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Estas secuencias de comandos se basan en [comandos CLI de Angular AEM](https://angular.io/cli/build) comunes, pero se han modificado ligeramente para que funcionen con el proyecto de mayor tamaño de la interfaz de usuario de la interfaz de usuario (CLI).

   `start`: ejecuta la aplicación de Angular localmente mediante un servidor web local. AEM Se ha actualizado para representar el contenido de la instancia local de.

   `build`: compila la aplicación de Angular para la distribución de producción. SPA La adición de `&& clientlib` es responsable de copiar los datos compilados en el módulo `ui.apps` como una biblioteca del lado del cliente durante una compilación. El módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) se usa para facilitar esto.

   Encontrará más detalles sobre los scripts disponibles [aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspect el archivo `ui.frontend/clientlib.config.js`. [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) utiliza este archivo de configuración para determinar cómo generar la biblioteca de cliente.

7. Inspect el archivo `ui.frontend/pom.xml`. Este archivo transforma la carpeta `ui.frontend` en un [módulo Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). SPA El archivo `pom.xml` se ha actualizado para usar el [complemento frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) para **probar** y **compilar** el durante una compilación de Maven.

8. Inspect cambió el archivo `app.component.ts` a las `ui.frontend/src/app/app.component.ts`:

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   SPA `app.component.js` es el punto de entrada de la. AEM SPA `ModelManager` lo proporciona el SDK de JS del Editor de JS de la interfaz de usuario de. Es responsable de llamar a `pageModel` (el contenido JSON) y de inyectarlo en la aplicación.

## Añadir un componente de encabezado {#header-component}

SPA AEM A continuación, añada un nuevo componente a la e implemente los cambios en una instancia de local para ver la integración.

1. Abra una nueva ventana de terminal y vaya a la carpeta `ui.frontend`:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Instale [CLI de Angular](https://angular.io/cli#installing-angular-cli) globalmente. Se usa para generar componentes de Angular, así como para generar y servir la aplicación de Angular mediante el comando **ng**.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > La versión de **@angular/cli** que usa este proyecto es **9.1.7**. Se recomienda mantener las versiones de CLI de Angular sincronizadas.

3. Cree un nuevo componente `Header` ejecutando el comando `ng generate component` de CLI de Angular desde la carpeta `ui.frontend`.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Se creará un esqueleto para el nuevo componente Encabezado de Angular en `ui.frontend/src/app/components/header`.

4. Abra el proyecto `aem-guides-wknd-spa` en el IDE que desee. Navegue hasta la carpeta `ui.frontend/src/app/components/header`.

   ![Ruta del componente Encabezado en el IDE](assets/integrate-spa/header-component-path.png)

5. Abra el archivo `header.component.html` y reemplace el contenido por lo siguiente:

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Tenga en cuenta que muestra contenido estático, por lo que este componente de Angular no requiere ningún ajuste en el `header.component.ts` generado de forma predeterminada.

6. Abra el archivo **app.component.html** en `ui.frontend/src/app/app.component.html`. Agregar `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Esto incluirá el componente `header` sobre todo el contenido de la página.

7. Abra un nuevo terminal, vaya a la carpeta `ui.frontend` y ejecute el comando `npm run build`:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navegue hasta la carpeta `ui.apps`. SPA Debajo de `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` debería ver que los archivos compilados de la `ui.frontend/build` se han copiado de la carpeta.

   ![Biblioteca de cliente generada en ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

9. Vuelva al terminal y navegue hasta la carpeta `ui.apps`. Ejecute el siguiente comando Maven:

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

10. Abra una ficha del explorador y vaya a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). SPA Ahora debería ver el contenido del componente `Header` que se muestra en el cuadro de diálogo de elementos de la interfaz de usuario de.

   ![Implementación inicial del encabezado](assets/integrate-spa/initial-header-implementation.png)

   Los pasos **7-9** se ejecutan automáticamente al activar una compilación de Maven desde la raíz del proyecto (por ejemplo, `mvn clean install -PautoInstallSinglePackage`). SPA AEM Ahora debería comprender los conceptos básicos de la integración entre las bibliotecas del lado del cliente de la y la de la segmentación de datos AEM Observe que todavía puede editar y agregar `Text` componentes en la lista de componentes, aunque el componente `Header` no se puede editar.

## Servidor de desarrollo de Webpack: proxy de la API de JSON {#proxy-json}

AEM Como se ha visto en los ejercicios anteriores, llevar a cabo una compilación y sincronizar la biblioteca de cliente con una instancia local de lleva unos minutos. SPA Esto es aceptable para las pruebas finales, pero no es ideal para la mayoría del desarrollo de la.

SPA Se puede usar un [servidor de desarrollo de Webpack](https://webpack.js.org/configuration/dev-server/) para desarrollar rápidamente el. SPA AEM La está impulsada por un modelo JSON generado por el grupo de informes de. AEM En este ejercicio, el contenido JSON de una instancia en ejecución de la instancia de es **procesado como proxy** en el servidor de desarrollo configurado por el [proyecto de Angular](https://angular.io/guide/build).

1. Vuelva al IDE y abra el archivo **proxy.conf.json** en `ui.frontend/proxy.conf.json`.

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   La [aplicación de Angular](https://angular.io/guide/build#proxying-to-a-backend-server) proporciona un mecanismo fácil para las solicitudes de API de proxy. AEM Los patrones especificados en `context` se procesarán como proxy a través de `localhost:4502`, el inicio rápido de la aplicación local de la aplicación de inicio de sesión de la aplicación de inicio rápido.

2. Abra el archivo **index.html** en `ui.frontend/src/index.html`. Este es el archivo HTML raíz que utiliza el servidor dev.

   Observe que hay una entrada para `base href="/"`. La etiqueta [base](https://angular.io/guide/deployment#the-base-tag) es crítica para que la aplicación resuelva direcciones URL relativas.

   ```html
   <base href="/">
   ```

3. Abra una ventana de terminal y vaya a la carpeta `ui.frontend`. Ejecute el comando `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. Abra una nueva ficha de explorador (si no está abierta) y vaya a [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Servidor de desarrollo de Webpack - proxy json](assets/integrate-spa/webpack-dev-server-1.png)

   AEM Debería ver el mismo contenido que en el caso de los recursos de creación, pero sin ninguna de las capacidades de creación habilitadas.

5. Vuelva al IDE y cree una nueva carpeta denominada `img` en `ui.frontend/src/assets`.
6. Descargue y agregue el siguiente logotipo WKND a la carpeta `img`:

   ![Logotipo de WKND](./assets/integrate-spa/wknd-logo-dk.png)

7. Abra **header.component.html** en `ui.frontend/src/app/components/header/header.component.html` e incluya el logotipo:

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   Guarde los cambios en **header.component.html**.

8. Vuelva al explorador. Debería ver reflejados inmediatamente los cambios en la aplicación.

   ![Logotipo añadido al encabezado](assets/integrate-spa/added-logo-localhost.png)

   AEM Puede seguir haciendo actualizaciones de contenido en **** y verlas reflejadas en el **servidor de desarrollo de Webpack**, ya que el contenido se transfiere como proxy. Tenga en cuenta que los cambios de contenido solo son visibles en el **servidor de desarrollo de Webpack**.

9. Detenga el servidor web local con `ctrl+c` en el terminal.

## Servidor de desarrollo de Webpack: simulación de API de JSON {#mock-json}

Otro enfoque para el desarrollo rápido es utilizar un archivo JSON estático para actuar como modelo JSON. AEM Al &quot;burlarse&quot; del JSON, eliminamos la dependencia de una instancia local de. También permite a un desarrollador front-end actualizar el modelo JSON para probar la funcionalidad y dirigir cambios en la API JSON que luego implementaría un desarrollador back-end.

AEM La configuración inicial del JSON ficticio **requiere una instancia de local**.

1. En el explorador, vaya a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   AEM Este es el JSON exportado por el usuario que está impulsando la aplicación. Copie la salida JSON.

2. Vuelva al IDE y vaya a `ui.frontend/src` y agregue las nuevas carpetas denominadas **mocks** y **json** para que coincidan con la siguiente estructura de carpetas:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Cree un nuevo archivo llamado **en.model.json** debajo de `ui.frontend/public/mocks/json`. Pegue aquí la salida JSON del **Paso 1**.

   ![Archivo Json de modelo ficticio](assets/integrate-spa/mock-model-json-created.png)

4. Cree un nuevo archivo **proxy.mock.conf.json** debajo de `ui.frontend`. Rellene el archivo con lo siguiente:

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   Esta configuración proxy reescribirá las solicitudes que comiencen por `/content/wknd-spa-angular/us` con `/mocks/json` y sirvan al archivo JSON estático correspondiente, por ejemplo:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Abra el archivo **angular.json**. Agregue una nueva configuración de **dev** con una matriz de **assets** actualizada para hacer referencia a la carpeta **mocks** creada.

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![Carpeta de actualización de Assets de desarrollo JSON de Angular](assets/integrate-spa/dev-assets-update-folder.png)

   AEM La creación de una configuración **dev** dedicada garantiza que la carpeta **mocks** solo se use durante el desarrollo y nunca se implemente para que se ejecute en una compilación de producción.

6. En el archivo **angular.json**, actualice la configuración de **browserTarget** para usar la nueva configuración de **dev**:

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![Actualización de desarrollo de compilación de JSON de Angular](assets/integrate-spa/angular-json-build-dev-update.png)

7. Abra el archivo `ui.frontend/package.json` y agregue un nuevo comando **start:mock** para hacer referencia al archivo **proxy.mock.conf.json**.

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   Añadir un nuevo comando facilita el cambio entre las configuraciones de proxy.

8. Si se está ejecutando, detenga el **servidor de desarrollo de Webpack**. Inicie el servidor de desarrollo **webpack** con el script **start:mock**:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   SPA Vaya a [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) y debería ver el mismo contenido, pero ahora se está extrayendo el contenido del archivo JSON **mock**.

9. Realice un pequeño cambio en el archivo **en.model.json** creado anteriormente. El contenido actualizado debe reflejarse inmediatamente en el **servidor de desarrollo de Webpack**.

   ![actualización json del modelo ficticio](./assets/integrate-spa/webpack-mock-model.gif)

   SPA La posibilidad de manipular el modelo JSON y ver los efectos en un modelo en directo puede ayudar a un desarrollador a comprender la API del modelo JSON. También permite que el desarrollo del front-end y del back-end se produzca en paralelo.

## Agregar estilos con Sass

A continuación, se añaden al proyecto algunos estilos actualizados. Este proyecto agregará compatibilidad con [Sass](https://sass-lang.com/) para algunas características útiles como las variables.

1. Abra una ventana de terminal y detenga **webpack dev server** si se inició. Desde la carpeta `ui.frontend`, escriba el siguiente comando para actualizar la aplicación de Angular y procesar los archivos **.scss**.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Esto actualizará el archivo `angular.json` con una nueva entrada en la parte inferior del archivo:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Instale `normalize-scss` para normalizar los estilos en todos los exploradores:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Vuelva al IDE y, debajo de `ui.frontend/src`, cree una nueva carpeta denominada `styles`.
4. Cree un nuevo archivo debajo de `ui.frontend/src/styles` con el nombre `_variables.scss` y rellénelo con las siguientes variables:

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
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. Cambie el nombre de la extensión del archivo **styles.css** en `ui.frontend/src/styles.css` a **styles.scss**. Reemplace el contenido por lo siguiente:

   ```scss
   /* styles.scss * /
   
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
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. Actualice **angular.json** y cambie el nombre de todas las referencias a **style.css** con **styles.scss**. Debe haber 3 referencias.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Actualizar estilos de encabezado

A continuación, agregue algunos estilos específicos de marca al componente **Header** mediante Sass.

1. Inicie **webpack dev server** para ver cómo se actualizan los estilos en tiempo real:

   ```shell
   $ npm run start:mock
   ```

2. En `ui.frontend/src/app/components/header`, cambie el nombre de **header.component.css** a **header.component.scss**. Rellene el archivo con lo siguiente:

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. Actualizar **header.component.ts** para hacer referencia a **header.component.scss**:

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. Vuelva al explorador y al **servidor de desarrollo de Webpack**:

   ![Encabezado con estilo: servidor de desarrollo de Webpack](assets/integrate-spa/styled-header.png)

   Ahora debería ver los estilos actualizados agregados al componente **Header**.

## SPA AEM Implementar actualizaciones de la en el

Actualmente, los cambios realizados en **Header** solo son visibles a través del **servidor de desarrollo de Webpack**. SPA AEM Implemente el actualizado para ver los cambios que se han producido.

1. Detenga el **servidor de desarrollo de Webpack**.
2. AEM Vaya a la raíz del proyecto `/aem-guides-wknd-spa` e implemente el proyecto para que se ejecute mediante Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Debería ver el **encabezado** actualizado con el logotipo y los estilos aplicados:

   AEM ![Encabezado actualizado en el código de tiempo ](assets/integrate-spa/final-header-component.png)

   SPA AEM Ahora que la actualización de la se encuentra en la fase de creación, la creación puede continuar.

## Enhorabuena. {#congratulations}

SPA AEM ¡Enhorabuena, ha actualizado la y ha explorado la integración con la opción de integración de! SPA AEM Ahora conoce dos enfoques diferentes para desarrollar la contra la API del modelo JSON de la aplicación de la red virtual mediante un **servidor de desarrollo de Webpack**.

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) o desprotegerlo localmente cambiando a la rama `Angular/integrate-spa-solution`.

### Siguientes pasos {#next-steps}

[Asignación de componentes de Angular a componentes de](map-components.md). Obtenga información sobre cómo asignar componentes de SPA AEM a componentes de Adobe Experience Manager AEM AEM SPA () con el SDK de JS de Editor de. SPA AEM SPA AEM La asignación de componentes permite a los autores realizar actualizaciones dinámicas de los componentes de la de componentes dentro del Editor de componentes, de forma similar a la creación tradicional de los componentes de la aplicación de la creación de.
