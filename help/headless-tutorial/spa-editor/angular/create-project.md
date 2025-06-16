---
title: Proyecto de SPA Editor | Introducción al Editor de SPA de AEM y Angular
description: Aprenda a utilizar un proyecto Maven de Adobe Experience Manager (AEM) como punto de partida para una aplicación de Angular integrada con AEM SPA Editor.
feature: SPA Editor, AEM Project Archetype
version: Experience Manager as a Cloud Service
jira: KT-5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 49fcd603-ab1a-4f1e-ae1f-49d3ff373439
duration: 252
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 1%

---

# Proyecto de SPA Editor {#create-project}

{{spa-editor-deprecation}}

Aprenda a utilizar un proyecto Maven de Adobe Experience Manager (AEM) como punto de partida para una aplicación de Angular integrada con AEM SPA Editor.

## Objetivo

1. Comprenda la estructura de un nuevo proyecto de AEM SPA Editor creado a partir de un arquetipo de Maven.
2. Implemente el proyecto de inicio en una instancia local de AEM.

## Qué va a generar

En este capítulo, se implementa un nuevo proyecto de AEM basado en el [Arquetipo de proyecto de AEM](https://github.com/adobe/aem-project-archetype). El proyecto de AEM tiene un punto de partida muy sencillo para la SPA de Angular. El proyecto utilizado en este capítulo servirá de base para la implementación de la SPA de WKND y se basa en capítulos futuros.

![Proyecto de inicio de Angular SPA de WKND](./assets/create-project/what-you-will-build.png)

*Un mensaje clásico de Hello World.*

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Asegúrese de que una nueva instancia de Adobe Experience Manager, iniciada en el modo **author**, se esté ejecutando localmente.

## Obtención del proyecto

Existen varias opciones para crear un proyecto de módulo múltiple de Maven para AEM. Este tutorial usó el [tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) más reciente como base para el código del tutorial. Se han realizado modificaciones en el código del proyecto para que sea compatible con varias versiones de AEM. Revise [la nota sobre la compatibilidad con versiones anteriores](overview.md#compatibility).

>[!CAUTION]
>
>Se recomienda usar la **última** versión del [tipo de archivo](https://github.com/adobe/aem-project-archetype) para generar un nuevo proyecto para una implementación real. Los proyectos de AEM deben tener como destino una sola versión de AEM utilizando la propiedad `aemVersion` del tipo de archivo.

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. La siguiente estructura de carpetas y archivos representa el proyecto de AEM generado por el arquetipo de Maven en el sistema de archivos local:

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. Se utilizaron las siguientes propiedades al generar el proyecto de AEM a partir del [arquetipo del proyecto de AEM](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | Propiedad | Valor |
   |-----------------|---------------------------------------|
   | aemVersion | nube |
   | appTitle | WKND SPA ANGULAR |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | paquete | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > Observe la propiedad `frontendModule=angular`. Esto indica al tipo de archivo del proyecto de AEM que arranque el proyecto con un [código base de Angular](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) inicial que se utilizará con el editor de SPA de AEM.

## Creación del proyecto

A continuación, compile, genere e implemente el código del proyecto en una instancia local de AEM usando Maven.

1. Asegúrese de que una instancia de AEM se esté ejecutando localmente en el puerto **4502**.
2. Desde el terminal de la línea de comandos, compruebe que Maven está instalado:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Ejecute el siguiente comando Maven desde el directorio `aem-guides-wknd-spa` para generar e implementar el proyecto en AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   Si usas [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   Los múltiples módulos del proyecto deben compilarse e implementarse en AEM.

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   El perfil de Maven ***autoInstallSinglePackage*** compila los módulos individuales del proyecto e implementa un paquete único en la instancia de AEM. De manera predeterminada, este paquete se implementa en una instancia de AEM que se ejecuta localmente en el puerto **4502** y con las credenciales de **admin:admin**.

4. Vaya a **[!UICONTROL Administrador de paquetes]** en la instancia local de AEM: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Debería ver tres paquetes para `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` y `wknd-spa-angular.ui.content`.

   ![Paquetes de SPA WKND](./assets/create-project/package-manager.png)

   Todo el código personalizado necesario para el proyecto se incluye en estos paquetes y se instala en el tiempo de ejecución de AEM.

6. También debería ver varios paquetes para `spa.project.core` y `core.wcm.components`. Son dependencias incluidas automáticamente por el tipo de archivo. Encontrará más información sobre [componentes principales de AEM aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es).

## Contenido del autor

A continuación, abra el SPA de inicio generado por el tipo de archivo y actualice parte del contenido.

1. Vaya a la consola de **[!UICONTROL Sites]**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   La SPA de WKND incluye una estructura básica del sitio con un país, un idioma y una página de inicio. Esta jerarquía se basa en los valores predeterminados del tipo de archivo para `language_country` y `isSingleCountryWebsite`. Estos valores se pueden sobrescribir actualizando las [propiedades disponibles](https://github.com/adobe/aem-project-archetype#available-properties) al generar un proyecto.

2. Para abrir la página **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]**, seleccione la página y haga clic en el botón **[!UICONTROL Editar]** de la barra de menús:

   ![consola del sitio](./assets/create-project/open-home-page.png)

3. Ya se agregó un componente **[!UICONTROL Texto]** a la página. Puede editar este componente como cualquier otro componente de AEM.

   ![Actualizar componente de texto](./assets/create-project/update-text-component.gif)

4. Agregue un componente **[!UICONTROL Text]** adicional a la página.

   Tenga en cuenta que la experiencia de creación es similar a la de una página de AEM Sites tradicional. Actualmente hay un número limitado de componentes disponibles para su uso. Se agregarán más en el curso del tutorial.

## Inspeccionar la aplicación de una sola página

A continuación, compruebe que se trata de una aplicación de una sola página con el uso de las herramientas para desarrolladores del explorador.

1. En el **[!UICONTROL Editor de páginas]**, haga clic en el menú **[!UICONTROL Información de la página]** > **[!UICONTROL Ver como publicado]**:

   ![Botón Ver como publicado](./assets/create-project/view-as-published.png)

   Se abrirá una nueva ficha con el parámetro de consulta `?wcmmode=disabled` que desactiva el editor de AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. Vea el origen de la página y observe que no se encuentra el contenido de texto **[!DNL Hello World]** o cualquiera de los demás contenidos. En su lugar, debería ver HTML como se muestra a continuación:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` es la SPA de Angular que se carga en la página y responsable de procesar el contenido.

   *¿De dónde proviene el contenido?*

3. Vuelva a la ficha: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. Abra las herramientas para desarrolladores del explorador e inspeccione el tráfico de red de la página durante una actualización. Ver las **solicitudes XHR**:

   ![Solicitudes XHR](./assets/create-project/xhr-requests.png)

   Debe haber una solicitud a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Contiene todo el contenido, con formato JSON, que dirigirá el SPA.

5. En una ficha nueva, abra [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   La solicitud `en.model.json` representa el modelo de contenido que controlará la aplicación. Inspeccione la salida JSON y debería poder encontrar el fragmento que representa los componentes **[!UICONTROL Texto]**.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   En el siguiente capítulo, analizaremos cómo se asigna el contenido JSON de los componentes de AEM a los componentes de SPA para formar la base de la experiencia del editor de SPA de AEM.

   >[!NOTE]
   >
   > Puede resultar útil instalar una extensión de explorador para dar formato automáticamente a la salida JSON.

## Enhorabuena. {#congratulations}

¡Felicidades, acaba de crear su primer proyecto de AEM SPA Editor!

Ahora mismo es bastante simple, pero en los próximos capítulos se agrega más funcionalidad.

### Siguientes pasos {#next-steps}

[Integrar el SPA](integrate-spa.md): aprenda cómo se integra el código fuente del SPA con el proyecto de AEM y comprenda las herramientas disponibles para desarrollar rápidamente el SPA.
