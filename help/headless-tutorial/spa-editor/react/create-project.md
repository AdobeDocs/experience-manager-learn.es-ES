---
title: Crear proyecto | AEM SPA Introducción al Editor de y React
description: Obtenga información sobre cómo generar un proyecto Maven de Adobe Experience Manager AEM AEM SPA () como punto de partida para una aplicación de React integrada con el Editor de.
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
jira: KT-413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
duration: 306
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 1%

---

# Crear proyecto {#spa-editor-project}

Obtenga información sobre cómo generar un proyecto Maven de Adobe Experience Manager AEM AEM SPA () como punto de partida para una aplicación de React integrada con el Editor de.

## Objetivo

1. SPA AEM Genere un proyecto habilitado para Editor de mediante el Arquetipo de proyecto de AppMeasurement.
2. AEM Implemente el proyecto de inicio en una instancia local de.

## Qué va a generar {#what-build}

AEM En este capítulo, se genera un nuevo proyecto de basado en la variable [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype). AEM SPA El proyecto de la está arrancado con un punto de partida muy sencillo para el React.

**¿Qué es un proyecto Maven?** - [Apache Maven](https://maven.apache.org/) es una herramienta de administración de software para crear proyectos. *Todos los Adobe Experience Manager* AEM Las implementaciones de utilizan proyectos de Maven para generar, administrar e implementar código personalizado sobre las implementaciones de.

**¿Qué es un arquetipo de Maven?** - A [Arquetipo de Maven](https://maven.apache.org/archetype/index.html) es una plantilla o un patrón para generar nuevos proyectos. AEM El tipo de archivo del proyecto de nos permite generar un nuevo proyecto con un área de nombres personalizada e incluir una estructura de proyecto que siga las prácticas recomendadas, lo que acelera en gran medida nuestro proyecto.

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment). Asegúrese de que una nueva instancia de Adobe Experience Manager, iniciada en **autor** , se está ejecutando localmente.

## Creación del proyecto {#create}

>[!NOTE]
>
>Este tutorial utiliza la versión de **35** del tipo de archivo.

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
   > AEM Si la segmentación es 6.5.5+, reemplazar `aemVersion="cloud"` con `aemVersion="6.5.5"`. Si el objetivo es 6.4.8+, utilice `aemVersion="6.4.8"`.

   Observe el `frontendModule=react` propiedad. AEM Esto indica al tipo de archivo del proyecto de la que arranque el proyecto con un iniciador [Base de código de React](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) AEM SPA para usar con el Editor de la. Propiedades como `appTitle`, `appId`, `artifactId`, y `groupId` se utilizan para identificar el proyecto y el propósito.

   Lista completa de propiedades disponibles para configurar un proyecto [se puede encontrar aquí](https://github.com/adobe/aem-project-archetype#available-properties).

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

   Cada carpeta representa un módulo Maven individual. En este tutorial trabajaremos principalmente con el `ui.frontend` , que es la aplicación React. Puede encontrar más información sobre los módulos individuales en la [AEM Documentación del tipo de archivo del proyecto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es).

## Implementación y compilación del proyecto

AEM A continuación, compile, genere e implemente el código del proyecto en una instancia local de mediante Maven.

1. AEM Asegúrese de que una instancia de la se esté ejecutando localmente en el puerto **4502**.
1. Desde la línea de comandos, vaya a `aem-guides-wknd-spa.react` directorio del proyecto.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. AEM Ejecute el siguiente comando para generar e implementar todo el proyecto en el que se va a realizar la ejecución del proyecto de forma:

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

   El perfil de Maven `autoInstallSinglePackage` AEM compila los módulos individuales del proyecto e implementa un paquete único en la instancia de la instancia de la. AEM De forma predeterminada, este paquete se implementa en una instancia de que se ejecuta localmente en el puerto **4502** y con las credenciales de `admin:admin`.

1. Vaya a **Administrador de paquetes** AEM en la instancia local de la: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Debería ver varios paquetes con el prefijo `aem-guides-wknd-spa.react`.

   ![SPA Paquetes de WKND](assets/create-project/package-manager.png)

   *AEM Administrador de paquetes*

   AEM Todo el código personalizado necesario para el proyecto se incluye en estos paquetes y se instala en el entorno de la.

## Contenido del autor

SPA A continuación, abra el código de inicio que generó el tipo de archivo y actualice parte del contenido.

1. Vaya a **Sites** consola: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   SPA El WKND incluye una estructura básica del sitio con un país, un idioma y una página de inicio. Esta jerarquía se basa en los valores predeterminados del tipo de archivo para `language_country` y `isSingleCountryWebsite`. Estos valores se pueden sobrescribir actualizando el [propiedades disponibles](https://github.com/adobe/aem-project-archetype#available-properties) al generar un proyecto.

2. Abra el **us** > **en** > **SPA Página de inicio de WKND React** página seleccionando la página y haciendo clic en el botón **Editar** en la barra de menús:

   ![consola del sitio](./assets/create-project/open-home-page.png)

3. A **Texto** ya se ha añadido el componente a la página. AEM Puede editar este componente como cualquier otro componente de la lista de componentes de la lista de componentes

   ![Actualizar componente de texto](./assets/create-project/update-text-component.gif)

4. Añadir un adicional **Texto** a la página.

   Tenga en cuenta que la experiencia de creación es similar a la de una página de AEM Sites tradicional. Actualmente hay un número limitado de componentes disponibles para su uso. Se agregarán más en el curso del tutorial.

## Inspect la aplicación de una sola página

A continuación, compruebe que se trata de una aplicación de una sola página con el uso de las herramientas para desarrolladores del explorador.

1. En el **Editor de página**, haga clic en **Información de página** botón > **Ver como aparece publicado**:

   ![Botón Ver como publicado](./assets/create-project/view-as-published.png)

   Se abrirá una nueva pestaña con el parámetro de consulta `?wcmmode=disabled` AEM que desactiva de forma efectiva el editor de: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Vea el origen de la página y observe que el contenido de texto **[!DNL Hello World]** o cualquier otro contenido no se encuentra. En su lugar, debería ver un HTML como el siguiente:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` SPA es la de React que se carga en la página y responsable de procesar el contenido.

   Sin embargo, *¿de dónde proviene el contenido?*

3. Vuelva a la pestaña: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Abra las herramientas para desarrolladores del explorador e inspeccione el tráfico de red de la página durante una actualización. Ver el **XHR** solicita:

   ![Solicitudes XHR](./assets/create-project/xhr-requests.png)

   Debe haber una solicitud a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). SPA Contiene todo el contenido, con formato JSON, que impulsará la creación de la.

5. En una nueva pestaña, abra [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   La solicitud `en.model.json` representa el modelo de contenido que dirigirá la aplicación. Inspect utiliza la salida JSON y debería poder encontrar el fragmento que representa el **[!UICONTROL Texto]** componente(s).

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

   AEM SPA AEM SPA En el siguiente capítulo analizaremos cómo se asigna este contenido JSON de Componentes de la a Componentes de la para formar la base de la experiencia del Editor de la.

   >[!NOTE]
   >
   > Puede resultar útil instalar una extensión de explorador para dar formato automáticamente a la salida JSON.

## Enhorabuena. {#congratulations}

AEM SPA Enhorabuena, acaba de crear su primer proyecto de editor de.

SPA La es bastante simple. En los próximos capítulos se agregará más funcionalidad.

### Pasos siguientes {#next-steps}

[SPA Integración de una](integrate-spa.md) SPA AEM SPA - Aprenda cómo se integra el código fuente de la con el Proyecto de la y comprenda las herramientas disponibles para desarrollar rápidamente el.
