---
title: 'Introducción a AEM Sites: Configuración del proyecto'
seo-title: 'Introducción a AEM Sites: Configuración del proyecto'
description: Abarca la creación de un proyecto Maven Multi Module para administrar el código y las configuraciones de un sitio AEM.
sub-product: sitios
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 4%

---


# Configuración del proyecto {#project-setup}

Este tutorial trata la creación de un proyecto de módulo múltiple de Maven para administrar el código y las configuraciones de un sitio de Adobe Experience Manager.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Asegúrese de que tiene una instancia nueva de Adobe Experience Manager disponible localmente y de que no se han instalado paquetes de muestra/demostración adicionales (que no sean los Service Packs requeridos).

## Objetivo {#objective}

1. Obtenga información sobre cómo generar un nuevo proyecto de AEM con un arquetipo de Maven.
1. Comprender los diferentes módulos generados por el arquetipo del proyecto AEM y cómo funcionan juntos.
1. Comprender cómo se incluyen AEM componentes principales en un proyecto AEM.

## Qué va a generar {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

En este capítulo, generará un nuevo proyecto de Adobe Experience Manager usando el [AEM Arquetipo de proyecto](https://github.com/adobe/aem-project-archetype). El proyecto de AEM contiene todo el código, el contenido y las configuraciones utilizadas para una implementación de sitios. El proyecto generado en este capítulo servirá de base para la implementación del sitio WKND y se basará en futuros capítulos.

## Fondo {#background}

**¿Qué es un proyecto Maven?** -  [Apache ](https://maven.apache.org/) Mavenis es una herramienta de gestión de software para construir proyectos. *Todas las implementaciones de Adobe Experience* Manager utilizan proyectos de Maven para crear, administrar e implementar código personalizado además de AEM.

**¿Qué es un arquetipo Maven?** - Un  [archipiélago ](https://maven.apache.org/archetype/index.html) Maven es una plantilla o un patrón para generar nuevos proyectos. El arquetipo AEM Proyecto nos permite generar un nuevo proyecto con una Área de nombres personalizada e incluir una estructura de proyecto que sigue las mejores prácticas, acelerando enormemente nuestro proyecto.

## Crear el proyecto {#create}

Hay un par de opciones para crear un proyecto multimódulo Maven para AEM. Este tutorial aprovechará el arquetipo de proyecto [Maven AEM **22**](https://github.com/adobe/aem-project-archetype). Cloud Manager también [proporciona un asistente de IU](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) para iniciar la creación de un proyecto de aplicación AEM. El proyecto subyacente generado por la interfaz de usuario del Administrador de nube tiene como resultado la misma estructura que el uso directo del arquetipo.

>[!NOTE]
>
>Para seguir este tutorial, utilice la versión **22** del arquetipo. Sin embargo, siempre es recomendable utilizar la versión **más reciente** del arquetipo para generar un nuevo proyecto.

La siguiente serie de pasos se realizará utilizando un terminal de línea de comandos basado en UNIX, pero debería ser similar si se utiliza un terminal de Windows.

1. Abra un terminal de línea de comandos y compruebe que Maven se haya instalado y agregado a la ruta de la línea de comandos:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Compruebe que el perfil **adobe-public** está activo ejecutando el siguiente comando:

   ```shell
   $ mvn help:effective-settings
       ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Si **no** ve el **adobe-public** es una indicación de que no se hace referencia a la repo de Adobe en el archivo `~/.m2/settings.xml`. Revise los pasos para instalar y configurar Apache Maven en [un entorno de desarrollo local](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Vaya a un directorio en el que desee generar el proyecto de AEM. Puede ser cualquier directorio en el que desee mantener el código fuente del proyecto. Por ejemplo, un directorio llamado `code` debajo del directorio principal del usuario:

   ```shell
   $ cd ~/code
   ```

1. Pegue lo siguiente en la línea de comandos para [generar el proyecto en modo por lotes](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   $ mvn archetype:generate -B \
       -DarchetypeGroupId=com.adobe.granite.archetypes \
       -DarchetypeArtifactId=aem-project-archetype \
       -DarchetypeVersion=22 \
       -DgroupId=com.adobe.aem.guides \
       -Dversion=0.0.1-SNAPSHOT \
       -DappsFolderName=wknd \
       -DartifactId=aem-guides-wknd \
       -Dpackage=com.adobe.aem.guides.wknd \
       -DartifactName="WKND Sites Project" \
       -DcomponentGroupName=WKND \
       -DconfFolderName=wknd \
       -DcontentFolderName=wknd \
       -DcssId=wknd \
       -DisSingleCountryWebsite=n \
       -Dlanguage_country=en_us \
       -DoptionAemVersion=6.5.0 \
       -DoptionDispatcherConfig=none \
       -DoptionIncludeErrorHandler=n \
       -DoptionIncludeExamples=y \
       -DoptionIncludeFrontendModule=y \
       -DpackageGroup=wknd \
       -DsiteName="WKND Site"
   ```

   >[!NOTE]
   >
   >De forma predeterminada, la generación de un proyecto a partir del arquetipo Maven utiliza el modo interactivo. Para evitar la afinación de los valores que generamos, utilice el modo por lotes. También es posible crear el proyecto Maven AEM mediante el complemento [AEM Developer Tools para Eclipse](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/aem-eclipse.html).

   >[!CAUTION]
   >
   >Si recibe un error como el siguiente: *No se pudo ejecutar el objetivo org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate (default-cli) en el proyecto standalone-pom: El arquetipo deseado no existe*. Es una indicación de que no se hace referencia a la repo de Adobe correctamente en el archivo `~/.m2/settings.xml`. Revise los pasos anteriores y compruebe que el archivo settings.xml hace referencia a la repo de Adobe.

   La tabla siguiente lista los valores utilizados en este tutorial:

   | Nombre | Valores | Descripción |
   |-----------------------------|---------|--------------------|
   | groupId | com.adobe.aem.guides | Id. de grupo de Maven base |
   | artiactId | aem-guide-wknd | Id. de artefacto de la curva base |
   | version | 0.0.1-SNAPSHOT | Versión |
   | package | com.adobe.aem.guides.wknd | Paquete de código fuente Java |
   | appsFolderName | wknd | /apps nombre de la carpeta |
   | artiactName | Proyecto de sitios WKND | Nombre del proyecto de Maven |
   | componentGroupName | WKND | Nombre del grupo de componentes AEM |
   | contentFolderName | wknd | /content nombre de la carpeta |
   | confFolderName | wknd | /conf nombre de carpeta |
   | cssId | wknd | prefijo utilizado en css generado |
   | packageGroup | wknd | Nombre del grupo de paquetes de contenido |
   | siteName | Sitio WKND | Nombre AEM sitio |
   | optionAemVersion | 6.5.0 | Destinatario AEM versión |
   | language_country | en_us | código de idioma o país desde el que crear la estructura de contenido (por ejemplo, en_us) |
   | optionIncludeExamples | y | Incluir un sitio de ejemplo de la biblioteca de componentes |
   | optionIncludeErrorHandler | n | Incluir una página de respuesta personalizada 404 |
   | optionIncludeFrontModule | y | Incluir un módulo de front-end dedicado |
   | isSingleCountryWebsite | n | Crear una estructura de idioma-maestro en contenido de ejemplo |
   | optionDispatcherConfig | ninguno | Generar un módulo de configuración de despachante |

1. El arquetipo Maven generará la siguiente carpeta y estructura de archivos en el sistema de archivos local:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.content/
           |--- ui.frontend /
           |--- it.launcher/
           |--- it.tests/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Genere el proyecto {#build}

Ahora que hemos generado un nuevo proyecto, podemos implementar el código de proyecto en una instancia local de AEM.

1. Asegúrese de que tiene una instancia de AEM que se ejecuta localmente en el puerto **4502**.
1. Desde la línea de comandos navegue hasta el directorio del proyecto `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Ejecute el siguiente comando para crear e implementar todo el proyecto para AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   El perfil Maven `autoInstallSinglePackage` compila los módulos individuales del proyecto e implementa un paquete único en la instancia de AEM. De forma predeterminada, este paquete se implementará en una instancia de AEM que se ejecute localmente en el puerto **4502** y con las credenciales de `admin:admin`.

1. Vaya al Administrador de paquetes en la instancia de AEM local: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Debe ver tres paquetes para `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.content` y `aem-guides-wknd.all`.

   También debería ver varios paquetes para [Componentes principales de AEM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html) que están incluidos en el proyecto por el arquetipo. Esto se tratará más adelante en el tutorial.

1. Vaya a la consola Sitios: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). El sitio WKND será uno de los sitios. Incluirá una estructura de sitio con una jerarquía de Estados Unidos y de los Maestros de idioma. Esta jerarquía del sitio se basa en los valores para `language_country` y `isSingleCountryWebsite` al generar el proyecto mediante el arquetipo.

1. Abra la página **US** `>` **Inglés** seleccionando la página y haciendo clic en el botón **Editar** en la barra de menús:

   ![consola del sitio](assets/project-setup/aem-sites-console.png)

1. Ya se ha creado parte del contenido y hay varios componentes disponibles para agregarlos a una página. Experimente con estos componentes para hacerse una idea de la funcionalidad. El modo en que se configuran esta página y los componentes se analizará en detalle más adelante en el tutorial.

## Inspect el proyecto {#project-structure}

El arquetipo AEM está formado por módulos Maven individuales:

* [núcleo](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) : paquete Java que contiene toda la funcionalidad básica, como servicios OSGi, oyentes o Planificadoras, así como código Java relacionado con componentes, como servlets o filtros de solicitud.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) - contiene las partes /apps del proyecto, es decir, clientlibs de JS&amp;CSS, componentes y configuraciones de OSGi
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) : contiene contenido estructural y configuraciones como plantillas editables, esquemas de metadatos (/content, /conf)
* ui.testing: paquete Java que contiene pruebas JUnit que se ejecutan en el servidor. Este paquete no debe implementarse en producción.
* ui.launcher: contiene código de pegado que implementa el paquete ui.testing (y los paquetes dependientes) en el servidor y activa la ejecución remota de JUnit
* [ui.front](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) - (opcional) contiene los artefactos necesarios para utilizar el módulo de compilación front-end basado en Webpack.
* todo: es un módulo Maven vacío que combina los módulos anteriores en un único paquete que se puede implementar en un entorno AEM.

![Maven el diagrama del proyecto](assets/project-setup/project-pom-structure.png)

Consulte la [documentación de arquetipo del proyecto de AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) para obtener más información sobre los módulos de Maven.

## Comandos Maven avanzados {#advanced-maven-commands}

Durante el desarrollo puede que esté trabajando con sólo uno de los módulos y desee evitar la generación de todo el proyecto para ahorrar tiempo. También es posible que desee implementar directamente en una instancia de AEM Publish o quizás en una instancia de AEM que no se ejecute en el puerto 4502.

A continuación, veremos algunos perfiles Maven adicionales y comandos que puede utilizar para la buena flexibilidad durante el desarrollo.

### Módulo principal {#core-module}

El módulo **[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** contiene todo el código Java asociado al proyecto. Cuando se crea, implementa un paquete OSGi para AEM. Para crear solo este módulo:

1. Vaya a la carpeta `core` (debajo de `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Ejecute el siguiente comando:

   ```shell
   $ mvn -PautoInstallBundle clean install
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   [INFO] Finished at: 2019-12-06T13:40:21-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Vaya a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Esta es la consola Web OSGi y contiene información sobre todos los paquetes instalados en la instancia de AEM.

1. Cambie la columna de ordenación **Id** y debería ver el paquete WKND instalado y activo.

   ![Paquete principal](assets/project-setup/wknd-osgi-console.png)

1. Puede ver la ubicación &#39;física&#39; del frasco en [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar):

   ![Ubicación de Jar en CRXDE](assets/project-setup/jcr-bundle-location.png)

### Módulos Ui.apps y Ui.content {#apps-content-module}

El módulo muven **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** contiene todo el código de procesamiento necesario para el sitio debajo de `/apps`. Esto incluye CSS/JS que se almacenarán en un formato AEM llamado [clientlibs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html). Esto también incluye secuencias de comandos [HTL](https://docs.adobe.com/docs/en/htl/overview.html) para procesar HTML dinámico. Puede pensar en el módulo **ui.apps** como un mapa de la estructura en el JCR pero en un formato que se puede almacenar en un sistema de archivos y transferir al control de código fuente. El módulo **ui.apps** sólo contiene código.

Para crear el módulo justo:

1. Desde la línea de comandos. Vaya a la carpeta `ui.apps` (debajo de `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Ejecute el siguiente comando:

   ```shell
   $ mvn -PautoInstallPackage clean install
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Vaya a [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Debe ver el paquete `ui.apps` como el primer paquete instalado y debe tener una marca de tiempo más reciente que cualquiera de los demás paquetes.

   ![Paquete Ui.apps instalado](assets/project-setup/ui-apps-package.png)

1. Vuelva a la línea de comandos y ejecute el siguiente comando (dentro de la carpeta `ui.apps`):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   El perfil `autoInstallPackagePublish` está diseñado para implementar el paquete en un entorno de publicación que se ejecuta en el puerto **4503**. Se espera el error anterior si no se encuentra una instancia de AEM que se ejecuta en http://localhost:4503.

1. Finalmente, ejecute el siguiente comando para implementar el paquete `ui.apps` en el puerto **4504**:

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   De nuevo, se espera que se produzca un error de compilación si no hay disponible ninguna instancia de AEM que se ejecute en el puerto **4504**. El parámetro `aem.port` se define en el archivo POM en `aem-guides-wknd/pom.xml`.

El módulo **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** está estructurado del mismo modo que el módulo **ui.apps**. La única diferencia es que el módulo **ui.content** contiene lo que se conoce como contenido **mutable**. **El contenido** múltiple se refiere esencialmente a configuraciones sin código como Plantillas, Políticas o estructuras de carpetas que se almacenan en el control de código fuente, pero que se  **** pueden modificar directamente en una instancia de AEM. Esto se analizará con mucho más detalle en el capítulo sobre Páginas y plantillas. Por ahora, lo importante es que los mismos comandos de Maven utilizados para crear el módulo **ui.apps** se puedan utilizar para generar el módulo **ui.content**. No dude en repetir los pasos anteriores desde la carpeta **ui.content**.

### Módulo Ui.frontentender {#ui-frontend-module}

El módulo **[ui.front](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** es un módulo Maven que en realidad es un proyecto [webpack](https://webpack.js.org/). Este módulo está configurado para ser un sistema de compilación front-end dedicado que genera archivos JavaScript y CSS, que a su vez se implementan en AEM. El módulo **ui.front** permite a los programadores codificar con idiomas como [Sass](https://sass-lang.com/), [TypeScript](https://www.typescriptlang.org/), utilizar los módulos [npm](https://www.npmjs.com/) e integrar la salida directamente en AEM.

El módulo **ui.front** se tratará con mucho más detalle en el capítulo sobre bibliotecas del lado del cliente y desarrollo del front-end. Por ahora veamos cómo se integra en el proyecto.

1. Desde la línea de comandos. Vaya a la carpeta `ui.frontend` (debajo de `aem-guides-wknd`):

   ```shell
   $ cd ../ui.frontend
   ```

1. Ejecute el siguiente comando:

   ```shell
   $ mvn clean install
   ...
   [INFO] write clientlib asset txt file (type: js): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js.txt
   [INFO] copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   [INFO]
   [INFO] write clientlib asset txt file (type: css): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt
   [INFO] copy: dist/clientlib-site/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   [INFO]
   [INFO] --- maven-assembly-plugin:3.1.1:single (default) @ aem-guides-wknd.ui.frontend ---
   [INFO] Reading assembly descriptor: assembly.xml
   [INFO] Building zip: /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO]
   [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ aem-guides-wknd.ui.frontend ---
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/pom.xml to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.pom
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  13.520 s
   [INFO] Finished at: 2019-12-06T15:26:16-08:00
   ```

   Observe las líneas como `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`. Esto indica que el CSS y JS compilados se están copiando en la carpeta `ui.apps`.

1. Vista la marca de tiempo modificada para el archivo `aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`. Debe actualizarse más recientemente que los demás archivos del módulo `ui.apps`.

   A diferencia de los otros módulos que hemos observado, el módulo **ui.frontender** no se implementa directamente en AEM. En su lugar, CSS y JS se copian en el módulo **ui.apps** y, a continuación, el módulo **ui.apps** se implementa en AEM. Si observa el orden de compilación desde el primer comando Maven, verá que **ui.front** siempre se genera *antes* **ui.apps**.

   Más adelante en el tutorial veremos las características avanzadas del módulo **ui.frontendr** y del servidor de desarrollo de webpack integrado para un rápido desarrollo.

## Inclusión de componentes principales {#core-components}

El arquetipo incrusta automáticamente [Componentes principales de AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) en el proyecto. Anteriormente, al inspeccionar los paquetes implementados en AEM, se incluían varios paquetes relacionados con los componentes principales. Componentes principales es un conjunto de componentes básicos diseñado para acelerar el desarrollo de un proyecto de AEM Sites. Los componentes principales son de código abierto y están disponibles en [GitHub](https://github.com/adobe/aem-core-wcm-components). Puede encontrar más información sobre cómo se incluyen los componentes principales [en el proyecto aquí](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components).

1. Con su editor de texto favorito, abra `aem-guides-wknd/pom.xml`.

1. Buscar `core.wcm.components.version`. Esto le mostrará qué versión de los componentes principales se incluye:

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > El arquetipo del proyecto AEM incluirá una versión de AEM componentes principales, aunque estos proyectos tengan diferentes ciclos de lanzamiento, por lo que la versión incluida de los componentes principales puede no ser la última. La práctica recomendada es aprovechar siempre la versión más reciente de Componentes principales. Las nuevas funciones y correcciones de errores se actualizan con frecuencia. Puede encontrar la información más reciente de la versión [en GitHub](https://github.com/adobe/aem-core-wcm-components/releases).

1. Si se desplaza hacia abajo hasta la sección `dependencies`, debería ver las dependencias individuales de los componentes principales:

   ```xml
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.core</artifactId>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.content</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.config</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   ```

## Administración de control de código fuente {#source-control}

Siempre es recomendable utilizar algún tipo de control de código fuente para administrar el código en la aplicación. Este tutorial utiliza git y GitHub. Maven y/o el IDE de su elección generan varios archivos que deben ser ignorados por el SCM.

Maven creará una carpeta de destinatario cada vez que cree e instale el paquete de código. La carpeta de destinatario y el contenido deben excluirse de SCM.

Debajo de ui.apps, también verá muchos archivos .content.xml que se han creado. Estos archivos XML asignan los tipos de nodo y las propiedades del contenido instalado en el JCR. Estos archivos son críticos y deben **no** ignorarse.

El arquetipo del proyecto AEM generará un archivo `.gitignore` de muestra que se puede utilizar como punto de partida para el cual se pueden ignorar los archivos de forma segura. El archivo se genera en `<src>/aem-guides-wknd/.gitignore`.

## Crítica {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## Felicitaciones! {#congratulations}

Felicitaciones, acaba de crear su primer proyecto AEM!

### Próximos pasos {#next-steps}

Comprender la tecnología subyacente de un componente de sitios de Adobe Experience Manager (AEM) mediante un ejemplo sencillo `HelloWorld` con el tutorial [Conceptos básicos de componentes](component-basics.md).
