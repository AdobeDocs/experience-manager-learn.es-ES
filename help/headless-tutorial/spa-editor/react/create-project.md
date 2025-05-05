---
title: Crear proyecto | Introducción al Editor de SPA de AEM y React
description: Obtenga información sobre cómo generar un proyecto Maven de Adobe Experience Manager (AEM) como punto de partida para una aplicación React integrada con AEM SPA Editor.
feature: SPA Editor, AEM Project Archetype
version: Experience Manager as a Cloud Service
jira: KT-413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
duration: 250
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 1%

---

# Crear proyecto {#spa-editor-project}

Obtenga información sobre cómo generar un proyecto Maven de Adobe Experience Manager (AEM) como punto de partida para una aplicación React integrada con AEM SPA Editor.

## Objetivo

1. Genere un proyecto habilitado para el Editor SPA utilizando el Arquetipo de proyecto de AEM.
2. Implemente el proyecto de inicio en una instancia local de AEM.

## Qué va a generar {#what-build}

En este capítulo, se genera un nuevo proyecto de AEM basado en el [Arquetipo de proyecto de AEM](https://github.com/adobe/aem-project-archetype). El proyecto de AEM tiene un punto de partida muy sencillo para el SPA de React.

**¿Qué es un proyecto Maven?** - [Apache Maven](https://maven.apache.org/) es una herramienta de administración de software para generar proyectos. *Todas las implementaciones de Adobe Experience Manager* utilizan proyectos Maven para generar, administrar e implementar código personalizado sobre AEM.

**¿Qué es un arquetipo de Maven?** - Un [arquetipo de Maven](https://maven.apache.org/archetype/index.html) es una plantilla o patrón para generar nuevos proyectos. El arquetipo de proyecto de AEM nos permite generar un nuevo proyecto con un área de nombres personalizada e incluir una estructura de proyecto que siga las prácticas recomendadas, lo que acelera en gran medida nuestro proyecto.

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Asegúrese de que una nueva instancia de Adobe Experience Manager, iniciada en el modo **author**, se esté ejecutando localmente.

## Creación del proyecto {#create}

>[!NOTE]
>
>Este tutorial utiliza la versión **35** del tipo de archivo.

1. Abra un terminal de línea de comandos e introduzca el siguiente comando Maven:

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=35 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Si se usa AEM 6.5.5+, reemplace `aemVersion="cloud"` por `aemVersion="6.5.5"`. Si el objetivo es 6.4.8+, utilice `aemVersion="6.4.8"`.

   Observe la propiedad `frontendModule=react`. Esto indica al tipo de archivo del proyecto de AEM que arranque el proyecto con un [código React base](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html?lang=es) inicial que se utilizará con el Editor de SPA de AEM. Propiedades como `appTitle`, `appId`, `artifactId` y `groupId` se utilizan para identificar el proyecto y el propósito.

   Encontrará una lista completa de las propiedades disponibles para configurar un proyecto [aquí](https://github.com/adobe/aem-project-archetype#available-properties).

1. El tipo de archivo Maven genera la siguiente estructura de carpetas y archivos en el sistema de archivos local:

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   Cada carpeta representa un módulo Maven individual. En este tutorial trabajaremos principalmente con el módulo `ui.frontend`, que es la aplicación React. Encontrará más detalles sobre módulos individuales en la [documentación de tipo de archivo del proyecto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es).

## Implementación y compilación del proyecto

A continuación, compile, genere e implemente el código del proyecto en una instancia local de AEM usando Maven.

1. Asegúrese de que una instancia de AEM se esté ejecutando localmente en el puerto **4502**.
1. Desde la línea de comandos, vaya al directorio del proyecto `aem-guides-wknd-spa.react`.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. Ejecute el siguiente comando para generar e implementar todo el proyecto en AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La compilación tardará aproximadamente un minuto y debe finalizar con el siguiente mensaje:

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

   El perfil de Maven `autoInstallSinglePackage` compila los módulos individuales del proyecto e implementa un paquete único en la instancia de AEM. De manera predeterminada, este paquete se implementa en una instancia de AEM que se ejecuta localmente en el puerto **4502** y con las credenciales de `admin:admin`.

1. Vaya a **Administrador de paquetes** en la instancia local de AEM: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Debería ver varios paquetes con el prefijo `aem-guides-wknd-spa.react`.

   ![Paquetes de SPA WKND](assets/create-project/package-manager.png)

   *Administrador de paquetes AEM*

   Todo el código personalizado necesario para el proyecto se incluye en estos paquetes y se instala en el entorno de AEM.

## Contenido del autor

A continuación, abra el SPA de inicio generado por el tipo de archivo y actualice parte del contenido.

1. Vaya a la consola de **Sites**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   La SPA de WKND incluye una estructura básica del sitio con un país, un idioma y una página de inicio. Esta jerarquía se basa en los valores predeterminados del tipo de archivo para `language_country` y `isSingleCountryWebsite`. Estos valores se pueden sobrescribir actualizando las [propiedades disponibles](https://github.com/adobe/aem-project-archetype#available-properties) al generar un proyecto.

2. Abra la página **us** > **en** > **WKND SPA React Home Page**; para ello, seleccione la página y haga clic en el botón **Editar** de la barra de menús:

   ![consola del sitio](./assets/create-project/open-home-page.png)

3. Ya se agregó un componente **Texto** a la página. Puede editar este componente como cualquier otro componente de AEM.

   ![Actualizar componente de texto](./assets/create-project/update-text-component.gif)

4. Agregue un componente **Text** adicional a la página.

   Tenga en cuenta que la experiencia de creación es similar a la de una página de AEM Sites tradicional. Actualmente hay un número limitado de componentes disponibles para su uso. Se agregarán más en el curso del tutorial.

## Inspeccionar la aplicación de una sola página

A continuación, compruebe que se trata de una aplicación de una sola página con el uso de las herramientas para desarrolladores del explorador.

1. En el **Editor de páginas**, haga clic en el botón **Información de la página** > **Ver como publicado**:

   ![Botón Ver como publicado](./assets/create-project/view-as-published.png)

   Se abrirá una nueva ficha con el parámetro de consulta `?wcmmode=disabled` que desactiva el editor de AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Vea el origen de la página y observe que no se encuentra el contenido de texto **[!DNL Hello World]** o cualquiera de los demás contenidos. En su lugar, debería ver HTML como se muestra a continuación:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` es el SPA de React cargado en la página y responsable de procesar el contenido.

   Sin embargo, *¿de dónde proviene el contenido?*

3. Vuelva a la ficha: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Abra las herramientas para desarrolladores del explorador e inspeccione el tráfico de red de la página durante una actualización. Ver las **solicitudes XHR**:

   ![Solicitudes XHR](./assets/create-project/xhr-requests.png)

   Debe haber una solicitud a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Contiene todo el contenido, con formato JSON, que dirigirá el SPA.

5. En una ficha nueva, abra [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   La solicitud `en.model.json` representa el modelo de contenido que controlará la aplicación. Inspeccione la salida JSON y debería poder encontrar el fragmento que representa los componentes **[!UICONTROL Texto]**.

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

   En el siguiente capítulo, analizaremos cómo se asigna este contenido JSON de los componentes de AEM a los componentes de SPA para formar la base de la experiencia del editor de SPA de AEM.

   >[!NOTE]
   >
   > Puede resultar útil instalar una extensión de explorador para dar formato automáticamente a la salida JSON.

## Enhorabuena. {#congratulations}

¡Felicidades, acaba de crear su primer proyecto de AEM SPA Editor!

El SPA es bastante sencillo. En los próximos capítulos se agregará más funcionalidad.

### Siguientes pasos {#next-steps}

[Integrar un SPA](integrate-spa.md): aprenda cómo se integra el código fuente del SPA con el proyecto de AEM y comprenda las herramientas disponibles para desarrollar rápidamente el SPA.
