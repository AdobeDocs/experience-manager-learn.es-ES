---
title: Proyecto de SPA Editor | Introducción al Editor de SPA de AEM y React
description: Aprenda a utilizar un proyecto de Maven de Adobe Experience Manager (AEM) como punto de partida para una aplicación React integrada con el Editor de SPA de AEM.
sub-product: sitios
feature: Editor de SPA, Tipo de archivo del proyecto AEM
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1136'
ht-degree: 3%

---


# Proyecto del Editor de SPA {#spa-editor-project}

Aprenda a utilizar un proyecto de Maven de Adobe Experience Manager (AEM) como punto de partida para una aplicación React integrada con el Editor de SPA de AEM.

## Objetivo

1. Comprenda la estructura de un nuevo proyecto de AEM SPA Editor creado a partir de un tipo de archivo Maven.
2. Implemente el proyecto de inicio en una instancia local de AEM.

## Qué va a generar

En este capítulo, se implementará un nuevo proyecto de AEM, basado en el [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype). El proyecto AEM se arrancará con un punto de partida muy sencillo para la SPA React. El proyecto utilizado en este capítulo servirá de base para la aplicación de la SPA de WKND y se basará en futuros capítulos.

![Proyecto de inicio WKND SPA React](./assets/create-project/wknd-spa-react.png)

*Iniciando la jerarquía del sitio para la SPA WKND.*

## Requisitos previos

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Asegúrese de que se esté ejecutando localmente una nueva instancia de Adobe Experience Manager, iniciada en modo **author**.

## Obtener el proyecto

Existen varias opciones para crear un proyecto multimódulo de Maven para AEM. Este tutorial utilizó el último [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) como base para el código del tutorial. Se han realizado modificaciones en el código del proyecto para que admita varias versiones de AEM. Revise [la nota sobre la compatibilidad con versiones anteriores](overview.md#compatibility).

>[!CAUTION]
>
> Se recomienda utilizar la versión **más reciente** del [arquetipo](https://github.com/adobe/aem-project-archetype) para generar un nuevo proyecto para una implementación real. Los proyectos de AEM deben dirigirse a una sola versión de AEM mediante la propiedad `aemVersion` del tipo de archivo.

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/create-project-start
   ```

2. La siguiente estructura de carpetas y archivos representa el proyecto AEM generado por el tipo de archivo Maven en el sistema de archivos local:

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

3. Se utilizaron las siguientes propiedades al generar el proyecto AEM desde el [tipo de archivo del proyecto AEM](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | Propiedad | Value |
   |-----------------|-------------------------------------|
   | aemVersion | nube |
   | appTitle | WKND SPA React |
   | appId | wknd-spa-react |
   | groupId | com.adobe.aem.guides |
   | frontendModule | react |
   | paquete | com.adobe.aem.guides.wknd.spa.react |
   | includeExamples | n |

   >[!NOTE]
   >
   > Observe la propiedad `frontendModule=react`. Esto indica al tipo de archivo del proyecto AEM que arranque el proyecto con un [React code base](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) que se utilizará con el Editor de SPA de AEM.

## Creación del proyecto

A continuación, compile, cree e implemente el código del proyecto en una instancia local de AEM mediante Maven.

1. Asegúrese de que una instancia de AEM se esté ejecutando localmente en el puerto **4502**.
2. Desde el terminal de la línea de comandos, compruebe que Maven está instalado:

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Ejecute el siguiente comando Maven desde el directorio `aem-guides-wknd-spa` para crear e implementar el proyecto en AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si utiliza [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   Los múltiples módulos del proyecto deben compilarse e implementarse en AEM.

   ```plain
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary for wknd-spa-react 1.0.0-SNAPSHOT:
    [INFO] 
    [INFO] wknd-spa-react ..................................... SUCCESS [  0.523 s]
    [INFO] WKND SPA React - Core .............................. SUCCESS [  8.069 s]
    [INFO] wknd-spa-react.ui.frontend - UI Frontend ........... SUCCESS [01:23 min]
    [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  0.830 s]
    [INFO] WKND SPA React - UI apps ........................... SUCCESS [  4.654 s]
    [INFO] WKND SPA React - UI content ........................ SUCCESS [  1.607 s]
    [INFO] WKND SPA React - All ............................... SUCCESS [  0.384 s]
    [INFO] WKND SPA React - Integration Tests Bundles ......... SUCCESS [  0.770 s]
    [INFO] WKND SPA React - Integration Tests Launcher ........ SUCCESS [  1.407 s]
    [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.055 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  01:44 min
   ```

   El perfil de Maven ***autoInstallSinglePackage*** compila los módulos individuales del proyecto e implementa un paquete único en la instancia de AEM. De forma predeterminada, este paquete se implementará en una instancia de AEM que se ejecute localmente en el puerto **4502** y con las credenciales de **admin:admin**.

4. Vaya al **[!UICONTROL Administrador de paquetes]** en la instancia local de AEM: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Debería ver tres paquetes para `wknd-spa-react.all`, `wknd-spa-react.ui.apps` y `wknd-spa-react.ui.content`.

   ![Paquetes WKND SPA](./assets/create-project/package-manager.png)

   *Administrador de paquetes AEM*

   Todo el código personalizado necesario para el proyecto se incluye en estos paquetes e se instala en el tiempo de ejecución de AEM.

6. También debería ver varios paquetes para `spa.project.core` y `core.wcm.components`. El tipo de archivo incluye automáticamente estas dependencias. Puede encontrar más información sobre los [componentes principales de AEM aquí](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html).

   `spa.project.core` es una dependencia necesaria para generar la API del modelo JSON que espera el Editor de SPA.

## Contenido de autor

A continuación, abra el SPA de inicio generado por el tipo de archivo y actualice parte del contenido.

1. Vaya a la consola **[!UICONTROL Sites]**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   La WKND SPA incluye una estructura básica del sitio con un país, idioma y página de inicio. Esta jerarquía se basa en los valores predeterminados del tipo de archivo para `language_country` y `isSingleCountryWebsite`. Estos valores se pueden sobrescribir actualizando las [propiedades disponibles](https://github.com/adobe/aem-project-archetype#available-properties) al generar un proyecto.

2. Abra la página **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA React Home Page]** seleccionando la página y haciendo clic en el botón **[!UICONTROL Editar]** en la barra de menús:

   ![consola del sitio](./assets/create-project/open-home-page.png)

3. Ya se ha agregado un componente **[!UICONTROL Texto]** a la página. Puede editar este componente como cualquier otro componente en AEM.

   ![Actualizar componente de texto](./assets/create-project/update-text-component.gif)

4. Agregue un componente **[!UICONTROL Texto]** adicional a la página.

   Tenga en cuenta que la experiencia de creación es similar a la de una página de AEM Sites tradicional. Actualmente hay un número limitado de componentes disponibles para usar. Se agregará más durante el tutorial.

## Inspeccionar la aplicación de una sola página

A continuación, compruebe que se trata de una aplicación de una sola página con las herramientas para desarrolladores del explorador.

1. En el **[!UICONTROL Editor de página]**, haga clic en el botón **[!UICONTROL Información de página]** > **[!UICONTROL Ver como aparece publicado]**:

   ![Botón Ver tal y como aparece publicado](./assets/create-project/view-as-published.png)

   Se abrirá una nueva pestaña con el parámetro de consulta `?wcmmode=disabled` que desactiva efectivamente el editor de AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Vea la fuente de la página y observe que no se encuentra el contenido de texto **[!DNL Hello World]** o cualquiera de los demás contenidos. En su lugar, debería ver HTML como el siguiente:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` es el SPA de React que se carga en la página y responsable de procesar el contenido.

   Sin embargo, *de dónde proviene el contenido?*

3. Vuelva a la pestaña : [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Abra las herramientas para desarrolladores del explorador e inspeccione el tráfico de red de la página durante una actualización. Ver las solicitudes **XHR**:

   ![Solicitudes XHR](./assets/create-project/xhr-requests.png)

   Debería haber una solicitud a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Contiene todo el contenido, con formato JSON, que dirigirá el SPA.

5. En una pestaña nueva, abra [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   La solicitud `en.model.json` representa el modelo de contenido que dirigirá la aplicación. Inspeccione la salida JSON y debería poder encontrar el fragmento que representa los componentes **[!UICONTROL Text]**.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   En el siguiente capítulo analizaremos cómo se asigna este contenido JSON de los componentes de AEM a los componentes de SPA para formar la base de la experiencia del Editor de SPA de AEM.

   >[!NOTE]
   >
   > Puede resultar útil instalar una extensión del explorador para dar formato automáticamente a la salida JSON.

## Felicitaciones! {#congratulations}

¡Felicidades, acaba de crear su primer proyecto de AEM SPA Editor!

El SPA es bastante sencillo. En los próximos capítulos se agregará más funcionalidad.

### Pasos siguientes {#next-steps}

[Integración de la SPA](integrate-spa.md) : Descubra cómo el código fuente de la SPA está integrado con el proyecto AEM y comprenda las herramientas disponibles para desarrollar rápidamente la SPA.
