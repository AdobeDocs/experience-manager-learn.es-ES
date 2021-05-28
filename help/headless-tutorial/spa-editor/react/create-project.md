---
title: Crear proyecto | Introducción al AEM SPA Editor y React
description: Obtenga información sobre cómo generar un proyecto de Adobe Experience Manager (AEM) Maven como punto de partida para una aplicación React integrada con el AEM SPA Editor.
sub-product: sitios
feature: SPA Editor, AEM Tipo De Archivo Del Proyecto
version: cloud-service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 2%

---


# Crear proyecto {#spa-editor-project}

Obtenga información sobre cómo generar un proyecto de Adobe Experience Manager (AEM) Maven como punto de partida para una aplicación React integrada con el AEM SPA Editor.

## Objetivo

1. Genere un proyecto habilitado para SPA Editor mediante el tipo de archivo del proyecto AEM.
2. Implemente el proyecto de inicio en una instancia local de AEM.

## Qué va a generar {#what-build}

En este capítulo, se generará un nuevo proyecto de AEM, basado en el [AEM tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype). El proyecto AEM será arrancado con un punto de partida muy simple para la SPA React.

**¿Qué es un proyecto Maven?** -  [Apache ](https://maven.apache.org/) Mavenis es una herramienta de gestión de software para construir proyectos. *Todas las implementaciones de Adobe Experience* Manager utilizan proyectos de Maven para crear, administrar e implementar código personalizado sobre AEM.

**¿Qué es un arquetipo de Maven?** - Un  [arquetipo ](https://maven.apache.org/archetype/index.html) Maven es una plantilla o patrón para generar nuevos proyectos. El arquetipo de proyecto AEM permite generar un nuevo proyecto con un área de nombres personalizada e incluir una estructura de proyecto que siga las prácticas recomendadas, lo que acelera enormemente nuestro proyecto.

## Requisitos previos

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Asegúrese de que se esté ejecutando localmente una nueva instancia de Adobe Experience Manager, iniciada en modo **author**.

## Crear el proyecto {#create}

>[!NOTE]
>
>Este tutorial utiliza la versión **27** del tipo de archivo. Siempre es recomendable utilizar la versión **más reciente** del tipo de archivo para generar un nuevo proyecto.

1. Abra un terminal de línea de comandos e introduzca el siguiente comando Maven:

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=27 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Si el objetivo es AEM 6.5.5+, reemplace `aemVersion="cloud"` por `aemVersion="6.5.5"`. Si va a ser 6.4.8+, use `aemVersion="6.4.8"`.

   Observe la propiedad `frontendModule=react`. Esto indica al tipo de archivo del proyecto AEM que arranque el proyecto con un [React code base](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) que se utilizará con el AEM SPA Editor. Para identificar el proyecto y el propósito se usan propiedades como `appTitle`, `appId`, `artifactId` y `groupId`.

   Una lista completa de las propiedades disponibles para configurar un proyecto [se puede encontrar aquí](https://github.com/adobe/aem-project-archetype#available-properties).

1. El tipo de archivo Maven del sistema de archivos local genera la siguiente carpeta y estructura de archivos:

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- all/
       |--- analyse/
       |--- core/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- it.tests/
       |--- dispatcher/
       |--- analyse/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
   ```

   Cada carpeta representa un módulo Maven individual. En este tutorial, principalmente trabajaremos con el módulo `ui.frontend` , que es la aplicación React. Puede encontrar más información sobre los módulos individuales en la [AEM documentación del tipo de archivo del proyecto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## Implementar y crear el proyecto

A continuación, compile, cree e implemente el código del proyecto en una instancia local de AEM mediante Maven.

1. Asegúrese de que una instancia de AEM se esté ejecutando localmente en el puerto **4502**.
1. Desde la línea de comandos, vaya al directorio del proyecto `aem-guides-wknd-spa.react`.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. Ejecute el siguiente comando para crear e implementar todo el proyecto para AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La compilación tardará aproximadamente un minuto y debe terminar con el siguiente mensaje:

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   El perfil de Maven `autoInstallSinglePackage` compila los módulos individuales del proyecto e implementa un paquete único en la instancia de AEM. De forma predeterminada, este paquete se implementará en una instancia de AEM que se ejecute localmente en el puerto **4502** y con las credenciales de `admin:admin`.

1. Vaya al **Administrador de paquetes** en la instancia de AEM local: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Debería ver varios paquetes con el prefijo `aem-guides-wknd-spa.react`.

   ![Paquetes SPA WKND](assets/create-project/package-manager.png)

   *Administrador de paquetes AEM*

   Todo el código personalizado necesario para el proyecto se incluye en estos paquetes e se instala en el entorno de AEM.

## Contenido de autor

A continuación, abra el SPA de inicio generado por el tipo de archivo y actualice parte del contenido.

1. Vaya a la consola **Sites**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   La SPA WKND incluye una estructura básica del sitio con un país, idioma y página de inicio. Esta jerarquía se basa en los valores predeterminados del tipo de archivo para `language_country` y `isSingleCountryWebsite`. Estos valores se pueden sobrescribir actualizando las [propiedades disponibles](https://github.com/adobe/aem-project-archetype#available-properties) al generar un proyecto.

2. Abra la página **us** > **en** > **WKND SPA React Home Page** seleccionando la página y haciendo clic en el botón **Edit** en la barra de menú:

   ![consola del sitio](./assets/create-project/open-home-page.png)

3. Ya se ha agregado un componente **Texto** a la página. Puede editar este componente como cualquier otro componente de AEM.

   ![Actualizar componente de texto](./assets/create-project/update-text-component.gif)

4. Agregue un componente **Texto** adicional a la página.

   Tenga en cuenta que la experiencia de creación es similar a la de una página de AEM Sites tradicional. Actualmente hay un número limitado de componentes disponibles para usar. Se agregará más durante el tutorial.

## Inspect: aplicación de una sola página

A continuación, compruebe que se trata de una aplicación de una sola página con las herramientas para desarrolladores del explorador.

1. En el **Editor de página**, haga clic en el botón **Información de página** > **Ver como aparece publicado**:

   ![Botón Ver tal y como aparece publicado](./assets/create-project/view-as-published.png)

   Se abrirá una nueva pestaña con el parámetro de consulta `?wcmmode=disabled` que desactiva efectivamente el editor de AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Vea la fuente de la página y observe que no se encuentra el contenido de texto **[!DNL Hello World]** o cualquiera de los demás contenidos. En su lugar, debería ver HTML como el siguiente:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` es la SPA React que se carga en la página y responsable de procesar el contenido.

   Sin embargo, *de dónde proviene el contenido?*

3. Vuelva a la pestaña : [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Abra las herramientas para desarrolladores del explorador e inspeccione el tráfico de red de la página durante una actualización. Ver las solicitudes **XHR**:

   ![Solicitudes XHR](./assets/create-project/xhr-requests.png)

   Debería haber una solicitud a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Contiene todo el contenido, con formato JSON, que dirigirá el SPA.

5. En una pestaña nueva, abra [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   La solicitud `en.model.json` representa el modelo de contenido que dirigirá la aplicación. Inspect utiliza la salida JSON y debe poder encontrar el fragmento que representa los componentes **[!UICONTROL Text]** .

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

   En el siguiente capítulo analizaremos cómo se asigna este contenido JSON de AEM componentes a SPA componentes para formar la base de la experiencia del AEM SPA editor.

   >[!NOTE]
   >
   > Puede resultar útil instalar una extensión del explorador para dar formato automáticamente a la salida JSON.

## Felicitaciones! {#congratulations}

¡Felicidades, acaba de crear su primer proyecto AEM SPA Editor!

El SPA es bastante sencillo. En los próximos capítulos se agregará más funcionalidad.

### Pasos siguientes {#next-steps}

[Integrar una SPA](integrate-spa.md) : Descubra cómo el código fuente de SPA está integrado con el proyecto de AEM y comprenda las herramientas disponibles para desarrollar rápidamente la SPA.
