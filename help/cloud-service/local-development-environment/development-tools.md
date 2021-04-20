---
title: Configuración de las herramientas de desarrollo para el desarrollo de AEM as a Cloud Service
description: Configure una máquina de desarrollo local con todas las herramientas básicas necesarias para desarrollarse con AEM localmente.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 2%

---


# Configuración de herramientas de desarrollo

El desarrollo de Adobe Experience Manager (AEM) requiere que se instale y configure un conjunto mínimo de herramientas de desarrollo en el equipo de desarrollador. Estas herramientas admiten el desarrollo y la creación de proyectos AEM.

Tenga en cuenta que `~` se utiliza como método abreviado para el Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`.

## Instalación de Java

Experience Manager es una aplicación Java y, por lo tanto, requiere el SDK de Java para admitir el desarrollo y el SDK de AEM as a Cloud Service.

1. [Descargar e instalar la última versión del SDK para Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2F2Fr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Compruebe que el SDK de Java 11 esté instalado ejecutando el comando :
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Instalar homebrew

_El uso de Homebrew es opcional, pero recomendado._

Homebrew es un gestor de paquetes de código abierto para macOS, Windows y Linux. Todas las herramientas de soporte se pueden instalar por separado, Homebrew proporciona una forma cómoda de instalar y actualizar una variedad de herramientas de desarrollo necesarias para el desarrollo de Experience Manager.

1. Abra el terminal
1. Compruebe si Homebrew ya está instalado ejecutando el comando : `brew --version`.
1. Si Homebrew no está instalado, instale Homebrew
   + [Instalar Homebrew en macOS](https://brew.sh/)
      + El inicio en macOS requiere [Xcode](https://apps.apple.com/us/app/xcode/id497799835) o [Herramientas de línea de comandos](https://developer.apple.com/download/more/), instalables mediante el comando:
         + `xcode-select --install`
   + [Instalar Homebrew en Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Instalar homebrew en Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Verifique que Homebrew esté instalado ejecutando el comando : `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Si está usando Homebrew, siga las instrucciones de __Instalar usando Homebrew__ en las secciones siguientes. Si está __no__ usando Homebrew, instale las herramientas utilizando los vínculos específicos del sistema operativo.

## Instalar Git

[](https://git-scm.com/) Proporciona el sistema de administración del control de código fuente utilizado por  [Adobe Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html) y, por lo tanto, es necesario para el desarrollo.

+ Instalación de Git con Homebrew
   1. Abra el símbolo del sistema/terminal
   1. Ejecute el comando: `brew install git`
   1. Verifique que Git esté instalado, utilizando el comando : `git --version`
+ O bien, descargue e instale Git (macOS, Linux o Windows)
   1. [Descargar e instalar Git](https://git-scm.com/downloads)
   1. Abra el símbolo del sistema/terminal
   1. Verifique que Git esté instalado, utilizando el comando : `git --version`

![Git](./assets/development-tools/git.png)

## Instalación de Node.js (y npm){#node-js}

[Node.](https://nodejs.org) jsis es un entorno de tiempo de ejecución de JavaScript que se utiliza para trabajar con los recursos front-end del subproyecto  __ui.__ frontendde un proyecto de AEM. Node.js se distribuye con [npm](https://www.npmjs.com/), es el administrador de paquetes de Node.js de facto, que se utiliza para administrar las dependencias de JavaScript.

+ Instalación de Node.js mediante Homebrew
   1. Abra el símbolo del sistema/terminal
   1. Ejecute el comando: `brew install node`
   1. Verifique que Node.js esté instalado, utilizando el comando : `node -v`
   1. Verifique que npm esté instalado, usando el comando: `npm -v`
+ O bien, descargue e instale Node.js (macOS, Linux o Windows)
   1. [Descargar e instalar Node.js](https://nodejs.org/en/download/)
   1. Abra el símbolo del sistema/terminal
   1. Verifique que Node.js esté instalado, utilizando el comando : `node -v`
   1. Verifique que npm esté instalado, usando el comando: `npm -v`

![Node.js y npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[Los proyectos AEM basados en el tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) instalan una versión aislada de Node.js en el momento de la compilación. Es bueno mantener la versión del sistema de desarrollo local sincronizada (o cercana) con las versiones Node.js y npm especificadas en el reactor pom.xml de su proyecto AEM Maven.
>
>Consulte este ejemplo [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) para saber dónde ubicar las versiones de compilación de Node.js y npm.

## Instalar Maven

Apache Maven es la herramienta de línea de comandos Java de código abierto que se utiliza para crear proyectos AEM generados a partir del tipo de archivo Maven del proyecto AEM. Todos los IDE principales ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) cuentan con soporte integrado para Maven.

+ Instalar Maven mediante Homebrew
   1. Abra el símbolo del sistema/terminal
   1. Ejecute el comando: `brew install maven`
   1. Verifique que Maven esté instalado, utilizando el comando : `mvn -v`
+ O bien, descargue e instale Maven (macOS, Linux o Windows)
   1. [Descargar Maven](https://maven.apache.org/download.cgi)
   1. [Instalar Maven](https://maven.apache.org/install.html)
   1. Abra el símbolo del sistema/terminal
   1. Verifique que Maven esté instalado, utilizando el comando : `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Configuración de la CLI de Adobe I/O{#aio-cli}

La [CLI de Adobe I/O](https://github.com/adobe/aio-cli) o `aio` proporciona acceso a la línea de comandos a una variedad de servicios de Adobe, incluidos [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) y [Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute). La CLI de Adobe I/O desempeña un papel integral en el desarrollo de AEM as a Cloud Service, ya que proporciona a los desarrolladores la capacidad de:

+ Registros de cola de AEM as a Cloud Services
+ Administrar canalizaciones de Cloud Manager desde la CLI

### Instalación de la CLI de Adobe I/O

1. Asegúrese de que [Node.js esté instalado](#node-js) ya que la CLI de Adobe I/O es un módulo npm
   + Ejecute `node --version` para confirmar
1. Ejecute `npm install -g @adobe/aio-cli` para instalar el módulo npm `aio` globalmente

### Configuración del complemento de Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

El complemento Adobe I/O Cloud Manager permite que la CLI de aio interactúe con Adobe Cloud Manager mediante el comando `aio cloudmanager`.

1. Ejecute `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` para instalar el complemento [aio Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

### Configuración del complemento Adobe I/O CLI Asset Compute{#aio-asset-compute}

El complemento Adobe I/O Cloud Manager permite que la CLI de aio genere y ejecute los trabajadores de Asset Compute mediante el comando `aio asset-compute`.

1. Ejecute `aio plugins:install @adobe/aio-cli-plugin-asset-compute` para instalar el complemento [aio Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute).

### Configuración de la autenticación CLI de Adobe I/O

Para que Adobe I/O CLI se comunique con Cloud Manager, se debe crear una integración de Cloud Manager en Adobe I/O Console y obtener credenciales para autenticarse correctamente.

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. Inicie sesión en [console.adobe.io](https://console.adobe.io)
1. Asegúrese de que su organización que incluye el producto de Cloud Manager al que se va a conectar esté activa en el conmutador de organización de Adobe
1. Cree un programa nuevo o abra un [Adobe I/O existente](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Los programas de la consola de Adobe I/O son simplemente agrupaciones organizativas de integraciones, crear o utilizar y programas existentes basados en cómo desee administrar las integraciones
   + Si crea un nuevo proyecto, seleccione &quot;Proyecto vacío&quot; si se le solicita (en lugar de Crear a partir de plantilla)
   + Los programas de la consola de Adobe I/O son conceptos diferentes de los programas de Cloud Manager
1. Cree una nueva integración de API de Cloud Manager con el perfil &quot;Desarrollador - Cloud Service&quot;
1. Obtenga las credenciales de la cuenta de servicio (JWT) para rellenar el [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) de la CLI de Adobe I/O
1. Cargue el archivo `config.json` en la CLI de Adobe I/O
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. Cargue el archivo `private.key` en la CLI de Adobe I/O
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

Inicie [la ejecución de comandos](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) para Cloud Manager a través de la CLI de Adobe I/O.

## Configuración del IDE de desarrollo

El desarrollo de AEM consiste principalmente en el desarrollo de Java y Front-end (JavaScript, CSS, etc.) y la administración de XML. Los siguientes son los IDE más populares para el desarrollo de AEM.

### IntelliJ IDEA

__[IntelliJ ](https://www.jetbrains.com/idea/)__ IDEAes un poderoso IDE para el desarrollo de Java. IntelliJ IDEA viene con dos sabores, una edición comunitaria gratuita y una versión comercial (de pago) Ultimate. La versión comunitaria gratuita es suficiente para el desarrollo de AEM, pero Ultimate [amplía su conjunto de capacidades](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [Descargar IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Descargar la herramienta Repositorio](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Código de Microsoft Visual Studio

__[Visual Studio Code](https://code.visualstudio.com/)__  (VS Code) es una herramienta gratuita de código abierto para desarrolladores de front-end. El código de Visual Studio se puede configurar para integrar la sincronización de contenido con AEM con la ayuda de una herramienta de Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code es la opción ideal para los desarrolladores de front-end que crean principalmente código de front-end; JavaScript, CSS y HTML. Aunque VS Code es compatible con Java a través de [extensiones](https://code.visualstudio.com/docs/java/java-tutorial), es posible que carezca de algunas de las funciones avanzadas proporcionadas por más específicas de Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Descargar código de Visual Studio](https://code.visualstudio.com/Download)
+ [Descargar la herramienta Repositorio](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Descargar la extensión de código VS de AEM](https://aemfed.io/)
+ [Descargar la extensión de código VS de AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse ](https://www.eclipse.org/ide/)__ IDEs es un IDE popular para el desarrollo de Java y es compatible con el complemento de   __[herramientas para desarrolladores de ](https://eclipse.adobe.com/aem/dev-tools/)__ AEM que proporciona Adobe, ya que proporciona una interfaz gráfica de usuario integrada para la creación y sincronización de contenido JCR con una instancia local de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Descargar Eclipse](https://www.eclipse.org/ide/)
+ [Descargar herramientas de desarrollo de Eclipse](https://eclipse.adobe.com/aem/dev-tools/)
