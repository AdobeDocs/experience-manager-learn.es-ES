---
title: AEM Configuración de las herramientas de desarrollo para el desarrollo as a Cloud Service de la
description: AEM Configure una máquina de desarrollo local con todas las herramientas de línea de base necesarias para desarrollar en comparación con las herramientas de desarrollo local y las herramientas de desarrollo de la base de datos de la base de datos de la base de datos de la base de datos de manera local.
feature: Developer Tools
version: Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3561
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1278'
ht-degree: 7%

---

# Configurar las herramientas de desarrollo {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Configuración de herramientas de desarrollo"
>abstract="El desarrollo de Adobe Experience Manager (AEM) requiere que se instale y configure un conjunto mínimo de herramientas de desarrollo en el equipo de desarrollo. Estas herramientas incluyen Java, Maven, CLI de Adobe I/O, IDE de desarrollo y más."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=es" text="Directrices de desarrollo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=es" text="Conceptos básicos de desarrollo"

El desarrollo de Adobe Experience Manager (AEM) requiere que se instale y configure un conjunto mínimo de herramientas de desarrollo en el equipo de desarrollo. AEM Estas herramientas admiten el desarrollo y la construcción de Proyectos de.

Tenga en cuenta que `~` se utiliza como abreviatura del Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`.

## Instalar Java

Experience Manager AEM es una aplicación Java y, por lo tanto, requiere que el SDK de Java admita el desarrollo y el SDK as a Cloud Service de la.

1. [Descargue e instale la última versión del SDK de Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Compruebe que el SDK de Java 11 de Oracle está instalado ejecutando el comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/development-tools/java.png)

## Instalar Homebrew

_El uso de Homebrew es opcional, pero se recomienda._

Homebrew es un gestor de paquetes de código abierto para macOS, Windows y Linux. Todas las herramientas de soporte se pueden instalar por separado, Homebrew proporciona una manera conveniente de instalar y actualizar una variedad de herramientas de desarrollo necesarias para el desarrollo del Experience Manager.

1. Abra el terminal
1. Compruebe si Homebrew ya está instalado ejecutando el comando: `brew --version`.
1. Si Homebrew no está instalado, instale Homebrew

>[!BEGINTABS]

>[!TAB macOS]

[Homebrew en macOS](https://brew.sh/) requiere [Xcode](https://apps.apple.com/us/app/xcode/id497799835) o [Herramientas de línea de comandos](https://developer.apple.com/download/more/), instalable mediante el comando:

```shell
$ xcode-select --install
```

>[!TAB Windows]

[Instalar Homebrew en Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB Linux]

[Instalar Homebrew en Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. Compruebe que Homebrew está instalado ejecutando el comando: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Si utiliza Homebrew, siga las __Instalar mediante Homebrew__ instrucciones en las secciones siguientes. Si es usted __no__ Con Homebrew, instale las herramientas de con los vínculos específicos del sistema operativo.

## Instalar Git

[Git](https://git-scm.com/) es el sistema de administración del control de código fuente que utiliza [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)y, por lo tanto, es necesario para el desarrollo.

>[!BEGINTABS]

>[!TAB Instalar Git mediante Homebrew]

1. Abra el terminal/símbolo del sistema
1. Ejecute el comando: `$ brew install git`
1. Compruebe que Git está instalado mediante el comando: `$ git --version`

>[!TAB Descargue e instale Git]

1. [Descargue e instale Git](https://git-scm.com/downloads)
1. Abra el terminal/símbolo del sistema
1. Compruebe que Git está instalado mediante el comando: `$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## Instalación de Node.js (y npm){#node-js}

[Node.js](https://nodejs.org) AEM es un entorno de tiempo de ejecución de JavaScript que se utiliza para trabajar con los recursos front-end de los recursos de un proyecto de __ui.frontend__ proyecto secundario. Node.js se distribuye con [npm](https://www.npmjs.com/), es el administrador de paquetes de Node.js de facto que se utiliza para administrar las dependencias de JavaScript.

>[!BEGINTABS]

>[!TAB Instalar Node.js mediante Homebrew]

1. Abra el terminal/símbolo del sistema
1. Ejecute el comando: `$ brew install node`
1. Compruebe que Node.js está instalado mediante el comando: `$ node -v`
1. Compruebe que npm está instalado mediante el comando: `$ npm -v`

>[!TAB Descargue e instale Node.js]

1. [Descargue e instale Node.js](https://nodejs.org/es/download/)
2. Abra el terminal/símbolo del sistema
3. Compruebe que Node.js está instalado mediante el comando: `$ node -v`
4. Compruebe que npm está instalado mediante el comando: `$ npm -v`

>[!ENDTABS]

![Node.js y npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype)AEM Los proyectos basados en el cliente instalan una versión aislada de Node.js en el momento de la compilación. AEM Es recomendable mantener la versión del sistema de desarrollo local sincronizada (o cercana) con las versiones de Node.js y npm especificadas en el pom.xml de Reactor de su proyecto de Maven de la red de distribución de contenido (Maven) de la red de distribución de contenido (Maven).
>
>Consulte este ejemplo [AEM Reactor de proyecto de pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) para saber dónde ubicar las versiones de compilación de Node.js y npm.

## Instalar Maven

AEM AEM Apache Maven es la herramienta de línea de comandos de código abierto de Java que se utiliza para crear proyectos de generados a partir del tipo de archivo del proyecto Maven de la aplicación de código abierto. Todos los IDE principales ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Código de Visual Studio](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) tienen compatibilidad con Maven integrada.


>[!BEGINTABS]

>[!TAB Instalar Maven mediante Homebrew]

1. Abra el terminal/símbolo del sistema
1. Ejecute el comando: `$ brew install maven`
1. Compruebe que Maven está instalado mediante el comando: `$ mvn -v`

>[!TAB Descargue e instale Maven]

1. [Descargar Maven](https://maven.apache.org/download.cgi)
1. [Instalar Maven](https://maven.apache.org/install.html?lang=es)
1. Abra el terminal/símbolo del sistema
1. Compruebe que Maven está instalado mediante el comando: `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## Configurar CLI de Adobe I/O{#aio-cli}

El [CLI DE ADOBE I/O](https://github.com/adobe/aio-cli), o `aio`, proporciona acceso desde la línea de comandos a una variedad de servicios de Adobe, entre los que se incluyen [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) y [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). La CLI de Adobe I/O AEM desempeña un papel integral en el desarrollo de los recursos as a Cloud Service, ya que proporciona a los desarrolladores la capacidad de:

+ AEM Registros de cola de servicios de as a Cloud Service de
+ Administrar canalizaciones de Cloud Manager desde la CLI
+ Implementar en [AEM Entornos de desarrollo rápido](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### Instale la CLI de Adobe I/O

1. Asegurar [Node.js está instalado](#node-js) ya que la CLI de Adobe I/O es un módulo npm
   + Ejecutar `node --version` para confirmar
1. Ejecutar `npm install -g @adobe/aio-cli` para instalar el `aio` npm module globalmente

### Configurar el complemento Cloud Manager de CLI de Adobe I/O{#aio-cloud-manager}

El complemento Adobe I/O Cloud Manager permite que la CLI de aio interactúe con Adobe Cloud Manager a través de `aio cloudmanager` comando.

1. Ejecutar `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` para instalar el [Complemento de aio Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### Configurar la autenticación CLI de Adobe I/O

Para que la CLI de Adobe I/O se comunique con Cloud Manager, [La integración de Cloud Manager debe crearse en la consola de Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager)Deben obtenerse las credenciales y para que la autenticación se realice correctamente.

1. Iniciar sesión en [console.adobe.io](https://console.adobe.io)
1. Asegúrese de que su organización que incluye el producto Cloud Manager al que conectarse esté activa en el conmutador de organización de Adobe
1. Cree un nuevo o abra un existente [programa Adobe I/O](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Los programas de Adobe I/O Console son simplemente agrupaciones organizativas de integraciones, cree o utilice y programas existentes basados en cómo desea administrar sus integraciones
   + Si crea un nuevo proyecto, seleccione &quot;Proyecto vacío&quot; si se le solicita (en lugar de &quot;Crear a partir de la plantilla&quot;)
   + Los programas de la consola de Adobe I/O son conceptos diferentes de los programas de Cloud Manager
1. Cree una nueva integración de API de Cloud Manager con el perfil &quot;Desarrollador - Cloud Service&quot;
1. Obtenga las credenciales de la cuenta de servicio (JWT) que necesita para rellenar las CLI de Adobe I/O [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. Cargue el `config.json` en la CLI de Adobe I/O
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Cargue el `private.key` en la CLI de Adobe I/O
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Comenzar [ejecutar comandos](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) para Cloud Manager mediante la CLI de Adobe I/O.

### AEM Configuración del complemento Entorno de desarrollo rápido de la{#rde}

AEM AEM El complemento Entorno de desarrollo rápido de la aplicación permite que la CLI de aio interactúe con el entorno de desarrollo as a Cloud Service de la interfaz de usuario de [Entornos de desarrollo rápido](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) a través de `aio aem:rde` comando.

1. Ejecutar `aio plugins:install @adobe/aio-cli-plugin-aem-rde` para instalar el [AEM Complemento Entornos de desarrollo rápido](https://github.com/adobe/aio-cli-plugin-aem-rde).

### Configurar el complemento de Asset compute de CLI de Adobe I/O{#aio-asset-compute}

El complemento de Adobe I/O Cloud Manager permite que la CLI de aio genere y ejecute Assets computes a través de `aio asset-compute` comando.

1. Ejecutar `aio plugins:install @adobe/aio-cli-plugin-asset-compute` para instalar el [complemento de Asset compute aio](https://github.com/adobe/aio-cli-plugin-asset-compute).

## Configurar el IDE de desarrollo

AEM El desarrollo de la consiste principalmente en el desarrollo de Java y front-end (JavaScript, CSS, etc.) y la administración de XML. AEM Los siguientes son los IDE más populares para el desarrollo de la.

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ es un potente IDE para el desarrollo de Java. IntelliJ IDEA viene en dos sabores, una edición comunitaria gratuita y una versión Ultimate comercial (de pago). AEM La versión gratuita de la Comunidad es suficiente para el desarrollo de la, sin embargo, el Ultimate [amplía su conjunto de capacidades](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [Descargar IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Descargar la herramienta Repositorio](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Código de Visual Studio](https://code.visualstudio.com/)__ (Código VS) es una herramienta gratuita de código abierto para desarrolladores de front-end. AEM El código de Visual Studio se puede configurar para integrar la sincronización de contenido con la ayuda de una herramienta de Adobe, con la que se puede crear una interfaz de usuario de código de Visual Studio. __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code es la opción ideal para los desarrolladores de front-end que crean principalmente código front-end; JavaScript, CSS y HTML. Mientras que el código VS tiene compatibilidad con Java mediante [extensiones](https://code.visualstudio.com/docs/java/java-tutorial)Sin embargo, es posible que carezca de algunas de las funciones avanzadas que ofrecen las funciones más específicas de Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Descargar código de Visual Studio](https://code.visualstudio.com/Download)
+ [Descargar la herramienta Repositorio](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Descargar extensión de código VS de aemfed](https://aemfed.io/)
+ [AEM Descargar extensión de código VS de sincronización](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ es un IDE popular para el desarrollo de Java y admite  __[AEM Herramientas para desarrolladores de](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)__ complemento proporcionado por Adobe AEM, que proporciona una interfaz gráfica de usuario en el IDE para la creación y sincronización de contenido JCR con una instancia de local.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Descargar Eclipse](https://www.eclipse.org/ide/)
+ [Descargar herramientas de desarrollo de Eclipse](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)
