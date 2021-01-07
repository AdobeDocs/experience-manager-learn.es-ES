---
title: Configure las herramientas de desarrollo para AEM como un desarrollo de Cloud Service
description: Configure una máquina de desarrollo local con todas las herramientas básicas necesarias para desarrollarse frente a AEM local.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
translation-type: tm+mt
source-git-commit: debf13d8e376979548bcbf55f71661d8cb8eb823
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 1%

---


# Configurar herramientas de desarrollo

El desarrollo de Adobe Experience Manager (AEM) requiere que se instale y configure un conjunto mínimo de herramientas de desarrollo en el equipo de desarrollo. Estas herramientas apoyan el desarrollo y la construcción de AEM proyectos.

Tenga en cuenta que `~` se utiliza como método abreviado para el Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`.

## Instalar Java

Experience Manager es una aplicación Java y, por lo tanto, requiere el SDK de Java para admitir el desarrollo y la AEM como un SDK de Cloud Service.

1. [Descargar e instalar la última versión del SDK de Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=lista&amp;p.offset=0&amp;p.limit=14)
1. Verifique que Java 11 SDK esté instalado ejecutando el comando:
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Instalar Homebrew

_El uso de Homebrew es opcional, pero se recomienda._

Homebrew es un administrador de paquetes de código abierto para macOS, Windows y Linux. Todas las herramientas de soporte se pueden instalar por separado, Homebrew proporciona una manera conveniente de instalar y actualizar una variedad de herramientas de desarrollo necesarias para el desarrollo de Experience Manager.

1. Abra el terminal
1. Compruebe si Homebrew ya está instalado ejecutando el comando: `brew --version`.
1. Si Homebrew no está instalado, instale Homebrew
   + [Instalar Homebrew en macOS](https://brew.sh/)
      + El menú Inicio en macOS requiere [Xcode](https://apps.apple.com/us/app/xcode/id497799835) o [Herramientas de línea de comandos](https://developer.apple.com/download/more/), instalable mediante el comando:
         + `xcode-select --install`
   + [Instalar Homebrew en Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Instalar Homebrew en Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Verifique que Homebrew esté instalado ejecutando el comando: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Si está utilizando Homebrew, siga las instrucciones de __Instalación con Homebrew__ que se encuentran en las secciones siguientes. Si __no__ utiliza Homebrew, instale las herramientas mediante los vínculos específicos del sistema operativo.

## Instalar Git

[](https://git-scm.com/) Gitis el sistema de administración de control de código fuente utilizado por  [Adobe Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html) y, por lo tanto, es necesario para el desarrollo.

+ Instalar Git con Homebrew
   1. Abra el terminal/símbolo del sistema
   1. Ejecute el comando: `brew install git`
   1. Verifique que Git esté instalado mediante el comando: `git --version`
+ O bien, descargue e instale Git (macOS, Linux o Windows)
   1. [Descargar e instalar Git](https://git-scm.com/downloads)
   1. Abra el terminal/símbolo del sistema
   1. Verifique que Git esté instalado mediante el comando: `git --version`

![Git](./assets/development-tools/git.png)

## Instalar Node.js (y npm){#node-js}

[Node.](https://nodejs.org) jsis es un entorno de tiempo de ejecución de JavaScript utilizado para trabajar con los recursos front-end del subproyecto  __ui.__ frontendde un proyecto de AEM. Node.js se distribuye con [npm](https://www.npmjs.com/), es el administrador de paquetes de Node.js de facto, que se utiliza para administrar las dependencias de JavaScript.

+ Instalación de Node.js mediante Homebrew
   1. Abra el terminal/símbolo del sistema
   1. Ejecute el comando: `brew install node`
   1. Verifique que Node.js esté instalado mediante el comando: `node -v`
   1. Verifique que npm esté instalado, usando el comando: `npm -v`
+ O bien, descargue e instale Node.js (macOS, Linux o Windows)
   1. [Descargar e instalar Node.js](https://nodejs.org/en/download/)
   1. Abra el terminal/símbolo del sistema
   1. Verifique que Node.js esté instalado mediante el comando: `node -v`
   1. Verifique que npm esté instalado, usando el comando: `npm -v`

![Node.js y npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM proyectos de AEM basados en arquetipos](https://github.com/adobe/aem-project-archetype) de proyecto instalan una versión aislada de Node.js en el momento de la compilación. Es bueno mantener la versión del sistema de desarrollo local sincronizada (o cercana) con las versiones Node.js y npm especificadas en el reactor pom.xml del proyecto AEM Maven.
>
>Consulte este ejemplo [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) para ver dónde ubicar las versiones de compilación Node.js y npm.

## Instalar Maven

Apache Maven es la herramienta de línea de comandos Java de código abierto que se utiliza para construir AEM proyectos generados a partir del AEM arquetipo Maven Project. Todos los IDE principales ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) tienen soporte Maven integrado.

+ Instalar Maven con Homebrew
   1. Abra el terminal/símbolo del sistema
   1. Ejecute el comando: `brew install maven`
   1. Verifique que Maven esté instalado mediante el comando: `mvn -v`
+ O bien, descargue e instale Maven (macOS, Linux o Windows)
   1. [Descargar Maven](https://maven.apache.org/download.cgi)
   1. [Instalar Maven](https://maven.apache.org/install.html)
   1. Abra el terminal/símbolo del sistema
   1. Verifique que Maven esté instalado mediante el comando: `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Configurar Adobe I/O CLI{#aio-cli}

La [CLI de Adobe I/O](https://github.com/adobe/aio-cli), o `aio`, proporciona acceso a la línea de comandos a una variedad de servicios de Adobe, incluidos [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) y [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). La CLI de Adobe I/O juega un papel integral en el desarrollo en AEM como Cloud Service, ya que proporciona a los desarrolladores la capacidad de:

+ Registros de seguimiento de AEM como servicios de Cloud Services
+ Administrar canalizaciones de Cloud Manager desde CLI

### Instalación de Adobe I/O CLI

1. Asegúrese de que [Node.js esté instalado](#node-js) ya que Adobe I/O CLI es un módulo npm
   + Ejecutar `node --version` para confirmar
1. Ejecute `npm install -g @adobe/aio-cli` para instalar el módulo npm `aio` de forma global

### Configure el complemento Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

El complemento Adobe I/O Cloud Manager permite que la CLI de aio interactúe con Adobe Cloud Manager mediante el comando `aio cloudmanager`.

1. Ejecute `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` para instalar el complemento [Administrador de Marketing Cloud](https://github.com/adobe/aio-cli-plugin-cloudmanager).

### Configure el complemento de Asset compute de Adobe I/O CLI{#aio-asset-compute}

El complemento Adobe I/O Cloud Manager permite que la CLI de aio genere y ejecute trabajadores de Asset compute mediante el comando `aio asset-compute`.

1. Ejecute `aio plugins:install @adobe/aio-cli-plugin-asset-compute` para instalar el complemento de [Asset compute de aio](https://github.com/adobe/aio-cli-plugin-asset-compute).

### Configuración de la autenticación de Adobe I/O CLI

Para que Adobe I/O CLI se comunique con Cloud Manager, se debe crear una integración de Cloud Manager en la consola de Adobe I/O y obtener las credenciales para autenticarse correctamente.

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. Inicie sesión en [console.adobe.io](https://console.adobe.io)
1. Asegúrese de que la organización que incluye el producto Cloud Manager al que conectarse esté activa en el conmutador de organización de Adobe
1. Cree un programa de Adobe I/O nuevo o abra uno existente [](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Los programas de la consola de Adobe I/O son simplemente agrupaciones organizativas de integraciones, creación o uso y programas existentes basados en cómo desee administrar las integraciones
   + Si crea un nuevo proyecto, seleccione &quot;Proyecto vacío&quot; si se le solicita (en comparación con Crear a partir de plantilla)
   + Los programas de la consola de Adobe I/O son conceptos diferentes de los programas de Cloud Manager
1. Cree una nueva integración de la API de Cloud Manager con el perfil &quot;Desarrollador - Cloud Service&quot;
1. Obtenga las credenciales de la cuenta de servicio (JWT) para rellenar el [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) de la CLI de Adobe I/O
1. Cargue el archivo `config.json` en la CLI de Adobe I/O
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. Cargue el archivo `private.key` en la CLI de Adobe I/O
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

Comience [la ejecución de comandos](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) para Cloud Manager mediante la CLI de Adobe I/O.

## Configure el IDE de desarrollo

AEM desarrollo consiste principalmente en desarrollo de Java y front-end (JavaScript, CSS, etc.) y administración de XML. Los siguientes son los IDE más populares para el desarrollo de AEM.

### IntelliJ IDEA

__[IntelliJ ](https://www.jetbrains.com/idea/)__ IDEAes un potente IDE para el desarrollo de Java. IntelliJ IDEA viene en dos sabores, una edición comunitaria gratuita y una versión comercial (de pago) Ultimate. La versión comunitaria gratuita es suficiente para AEM desarrollo, pero Ultimate [amplía su conjunto de capacidades](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [Descargar IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Descargar la herramienta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Código de Microsoft Visual Studio

__[Visual Studio Code](https://code.visualstudio.com/)__  (VS Code) es una herramienta gratuita de código abierto para desarrolladores de front-end. El código de Visual Studio se puede configurar para integrar la sincronización de contenido con AEM con la ayuda de una herramienta de Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

El código de Visual Studio es la opción ideal para los desarrolladores de front-end que crean principalmente código de front-end; JavaScript, CSS y HTML. Aunque VS Code tiene compatibilidad con Java mediante [extensiones](https://code.visualstudio.com/docs/java/java-tutorial), es posible que carezca de algunas de las características avanzadas proporcionadas por más específicas de Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Descargar código de Visual Studio](https://code.visualstudio.com/Download)
+ [Descargar la herramienta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Descargar la extensión de código VS de fuente de datos](https://aemfed.io/)
+ [Descargar extensión de código VS de sincronización AEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse ](https://www.eclipse.org/ide/)__ IDEs es un IDE popular para el desarrollo de Java y es compatible con el complemento   __[AEM Developer ](https://eclipse.adobe.com/aem/dev-tools/)__ Toolscomplemento proporcionado por Adobe, que proporciona una GUI en IDE para la creación y sincronización del contenido JCR con una instancia de AEM local.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Descargar Eclipse](https://www.eclipse.org/ide/)
+ [Descargar herramientas de Eclipse Dev](https://eclipse.adobe.com/aem/dev-tools/)
