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
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1887'
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

**¿Qué es un proyecto Maven?** -  [Apache ](https://maven.apache.org/) Mavenis es una herramienta de gestión de software para construir proyectos. *Todas las implementaciones de Adobe Experience* Manager utilizan proyectos de Maven para crear, administrar e implementar código personalizado además de AEM.

**¿Qué es un arquetipo Maven?** - Un  [archipiélago ](https://maven.apache.org/archetype/index.html) Maven es una plantilla o un patrón para generar nuevos proyectos. El arquetipo AEM Proyecto nos permite generar un nuevo proyecto con una Área de nombres personalizada e incluir una estructura de proyecto que sigue las mejores prácticas, acelerando enormemente nuestro proyecto.

## Crear el proyecto {#create}

Hay un par de opciones para crear un proyecto multimódulo Maven para AEM. Este tutorial aprovechará el arquetipo de proyecto [Maven AEM **25**](https://github.com/adobe/aem-project-archetype). Cloud Manager también [proporciona un asistente de IU](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) para iniciar la creación de un proyecto de aplicación AEM. El proyecto subyacente generado por la interfaz de usuario del Administrador de nube tiene como resultado la misma estructura que el uso directo del arquetipo.

>[!NOTE]
>
>Este tutorial utiliza la versión **25** del arquetipo. Siempre es recomendable utilizar la versión **más reciente** del arquetipo para generar un nuevo proyecto.

La siguiente serie de pasos se realizará utilizando un terminal de línea de comandos basado en UNIX, pero debería ser similar si se utiliza un terminal de Windows.

1. Abra un terminal de línea de comandos. Compruebe que Maven está instalado:

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
   mvn -B archetype:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=25 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides.wknd" \
       -D artifactId="aem-guides-wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5.5.0+ o 6.4.8.1+, reemplace `aemVersion="cloud"` por su versión de destinatario de AEM, por ejemplo `aemVersion="6.5.5"` o `aemVersion="6.4.8.1"`

   Puede encontrar una lista completa de las propiedades disponibles para configurar un proyecto [aquí](https://github.com/adobe/aem-project-archetype#available-properties).

1. El arquetipo Maven generará la siguiente carpeta y estructura de archivos en el sistema de archivos local:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
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

## Implementar y generar el proyecto {#build}

Cree e implemente el código del proyecto en una instancia local de AEM.

1. Asegúrese de que tiene una instancia de autor de AEM que se ejecuta localmente en el puerto **4502**.
1. Desde la línea de comandos navegue hasta el directorio del proyecto `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Ejecute el siguiente comando para crear e implementar todo el proyecto para AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La compilación tardará aproximadamente un minuto y finalizará con el siguiente mensaje:

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.269 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  8.047 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [01:02 min]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  1.985 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  8.037 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  4.672 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.313 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.270 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [ 15.571 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.232 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.728 s]
   [INFO] WKND Sites Project - Project Analyser .............. SUCCESS [ 33.398 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:18 min
   [INFO] Finished at: 2021-01-31T12:33:56-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   El perfil Maven `autoInstallSinglePackage` compila los módulos individuales del proyecto e implementa un paquete único en la instancia de AEM. De forma predeterminada, este paquete se implementará en una instancia de AEM que se ejecute localmente en el puerto **4502** y con las credenciales de `admin:admin`.

1. Vaya al Administrador de paquetes en la instancia de AEM local: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Debe ver los paquetes para `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` y `aem-guides-wknd.all`.

1. Vaya a la consola Sitios: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). El sitio WKND será uno de los sitios. Incluirá una estructura de sitio con una jerarquía de Estados Unidos y de los Maestros de idioma. Esta jerarquía del sitio se basa en los valores para `language_country` y `isSingleCountryWebsite` al generar el proyecto mediante el arquetipo.

1. Abra la página **US** `>` **Inglés** seleccionando la página y haciendo clic en el botón **Editar** en la barra de menús:

   ![consola del sitio](assets/project-setup/aem-sites-console.png)

1. Ya se ha creado el contenido de inicio y hay varios componentes disponibles para agregarlos a una página. Experimente con estos componentes para hacerse una idea de la funcionalidad. En el capítulo siguiente aprenderá los conceptos básicos de un componente.

   ![Contenido de inicio de página principal](assets/project-setup/start-home-page.png)

   *Contenido de muestra generado por el arquetipo*

## Inspect el proyecto {#project-structure}

El proyecto de AEM generado está formado por módulos Maven individuales, cada uno con una función diferente. Este tutorial y la mayoría de los programas de desarrollo se centran en estos módulos:

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) : código Java, principalmente desarrolladores de back-end.
* [ui.front](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) : contiene código fuente para CSS, JavaScript, Sass y Type Script, principalmente para desarrolladores de front-end.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) : contiene definiciones de componente y cuadro de diálogo, incrusta CSS y JavaScript compilados como bibliotecas de cliente.
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) - contiene contenido estructural y configuraciones como plantillas editables, esquemas de metadatos (/content, /conf).

* **todo** : es un módulo Maven vacío que combina los módulos anteriores en un único paquete que se puede implementar en un entorno AEM.

![Maven el diagrama del proyecto](assets/project-setup/project-pom-structure.png)

Consulte la [documentación de arquetipo del proyecto de AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) para obtener más información sobre **todos** los módulos de Maven.

### Inclusión de componentes principales {#core-components}

[AEM ](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html) componentes principales son un conjunto de componentes estandarizados de Gestor de contenido web (WCM) para AEM. Estos componentes proporcionan un conjunto de referencia de una funcionalidad y están diseñados para ser diseñados, personalizados y ampliados para proyectos individuales.

AEM como entornos de Cloud Service incluyen la versión más reciente de [AEM Componentes principales](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html). Por lo tanto, los proyectos generados para AEM como Cloud Service **no** incluyen una incrustación de AEM componentes principales.

Para AEM proyectos generados con 6.5/6.4, el arquetipo incrusta automáticamente [AEM componentes principales](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) en el proyecto. Se recomienda que AEM 6.5/6.4 integre AEM componentes principales para garantizar que la versión más reciente se implemente con el proyecto. Puede encontrar más información sobre cómo se incluyen los componentes principales [en el proyecto aquí](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## Administración de control de código fuente {#source-control}

Siempre es recomendable utilizar algún tipo de control de código fuente para administrar el código en la aplicación. Este tutorial utiliza git y GitHub. Maven y/o el IDE de su elección generan varios archivos que deben ser ignorados por el SCM.

Maven creará una carpeta de destinatario cada vez que cree e instale el paquete de código. La carpeta de destinatario y el contenido deben excluirse de SCM.

Debajo de `ui.apps` observe que se crean muchos archivos `.content.xml`. Estos archivos XML asignan los tipos de nodo y las propiedades del contenido instalado en el JCR. Estos archivos son críticos y deben **no** ignorarse.

El arquetipo del proyecto AEM generará un archivo `.gitignore` de muestra que se puede utilizar como punto de partida para el cual se pueden ignorar los archivos de forma segura. El archivo se genera en `<src>/aem-guides-wknd/.gitignore`.

## Felicitaciones! {#congratulations}

Felicitaciones, acaba de crear su primer proyecto AEM!

### Próximos pasos {#next-steps}

Comprender la tecnología subyacente de un componente de sitios de Adobe Experience Manager (AEM) mediante un ejemplo sencillo `HelloWorld` con el tutorial [Conceptos básicos de componentes](component-basics.md).

## Comandos Maven avanzado (bonificación) {#advanced-maven-commands}

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
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. Vaya a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Esta es la consola Web OSGi y contiene información sobre todos los paquetes instalados en la instancia de AEM.

1. Cambie la columna de ordenación **Id** y debería ver el paquete WKND instalado y activo.

   ![Paquete principal](assets/project-setup/wknd-osgi-console.png)

1. Puede ver la ubicación &#39;física&#39; del frasco en [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Ubicación de Jar en CRXDE](assets/project-setup/jcr-bundle-location.png)

### Módulos Ui.apps y Ui.content {#apps-content-module}

El módulo muven **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** contiene todo el código de procesamiento necesario para el sitio debajo de `/apps`. Esto incluye CSS/JS que se almacenarán en un formato AEM llamado [clientlibs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/clientlibs.html). Esto también incluye secuencias de comandos [HTL](https://docs.adobe.com/content/help/es-ES/experience-manager-htl/using/overview.html) para procesar HTML dinámico. Puede pensar en el módulo **ui.apps** como un mapa de la estructura en el JCR pero en un formato que se puede almacenar en un sistema de archivos y transferir al control de código fuente. El módulo **ui.apps** sólo contiene código.

Para crear el módulo justo:

1. Desde la línea de comandos. Vaya a la carpeta `ui.apps` (debajo de `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Ejecute el siguiente comando:

   ```shell
   $ mvn clean install -PautoInstallPackage
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

El módulo **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** está estructurado del mismo modo que el módulo **ui.apps**. La única diferencia es que el módulo **ui.content** contiene lo que se conoce como contenido **mutable**. **El contenido** múltiple se refiere esencialmente a configuraciones sin código como Plantillas, Políticas o estructuras de carpetas que se almacenan en el control de código fuente, pero que se  **** pueden modificar directamente en una instancia de AEM. Esto se analizará con mucho más detalle en el capítulo sobre Páginas y plantillas.

Los mismos comandos Maven que se utilizan para generar el módulo **ui.apps** se pueden utilizar para generar el módulo **ui.content**. No dude en repetir los pasos anteriores desde la carpeta **ui.content**.