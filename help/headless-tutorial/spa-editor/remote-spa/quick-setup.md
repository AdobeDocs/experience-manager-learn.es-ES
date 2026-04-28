---
title: Quick setup SPA Editor and Remote SPA
description: Learn how to get up and running with a remote SPA and AEM SPA Editor in 15 mins!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 13%

---

# Configuración rápida

{{spa-editor-deprecation}}

Quick setup is an expedited walk-through illustrating how to install and run the WKND App and as a Remote SPA, and author it using AEM SPA Editor.

Quick setup takes you directly to the end state of this tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_A video walk-through of the quick setup_

## Requisitos previos

Este tutorial requiere lo siguiente:

+ [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=es)
+ [Node.js v18](https://nodejs.org/es/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ macOS only prerequisites
   + [Xcode](https://developer.apple.com/xcode/) or [Xcode command-line tools](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip or greater](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql source code (branch: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Este tutorial utiliza lo siguiente:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) como IDE
+ Un directorio de trabajo de `~/Code/wknd-app`
+ Ejecución del SDK de AEM como un servicio de Author en `http://localhost:4502`
+ Ejecución del SDK de AEM con la cuenta local `admin` con la contraseña `admin`
+ Ejecución de la SPA en `http://localhost:3000`

## Start the AEM SDK Quickstart

Download and install the AEM SDK Quickstart on port 4502, with default `admin/admin` credentials.

1. [Download latest AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. Unzip the AEM SDK to `~/aem-sdk`
1. Run the AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK starts and automatically launches on [http://localhost:4502](http://localhost:4502). Log in using the following credentials:

+ Nombre de usuario: `admin`
+ Contraseña: `admin`

## Descargar e instalar el paquete del sitio WKND

Este tutorial depende del proyecto __WKND 2.1.0+&#39;s__ (para contenido).

1. [Descargar la última versión de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Inicie sesión en el Administrador de paquetes de AEM SDK en [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con las credenciales de `admin`.
1. __Cargar__ `aem-guides-wknd.all.x.x.x.zip` descargado en el paso 1
1. Pulse el botón __Instalar__ para la entrada `aem-guides-wknd.all-x.x.x.zip`

## Descargar e instalar paquetes de la SPA de la aplicación WKND

Para realizar una configuración rápida, aquí se proporcionan paquetes de AEM que contienen la configuración y el contenido finales de AEM del tutorial.

1. [Descargar `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Descargar `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Inicie sesión en el Administrador de paquetes de AEM SDK en [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con las credenciales de `admin`.
1. __Cargar__ `wknd-app.all.x.x.x.zip` descargado en el paso 1
1. Pulse el botón __Instalar__ para la entrada `wknd-app.all.x.x.x.zip`
1. __Cargar__ `wknd-app.ui.content.sample.x.x.x.zip` descargado en el paso 2
1. Pulse el botón __Instalar__ para la entrada `wknd-app.ui.content.sample.x.x.x.zip`

## Descargar el origen de la aplicación WKND

Descargue el código fuente de la aplicación WKND desde Github.com y cambie la rama que contiene los cambios en la SPA realizados en este tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Inicio de la aplicación SPA

Desde la raíz del proyecto, instale las dependencias npm de los proyectos SPA y ejecute la aplicación.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Si hay errores al ejecutar `npm install`, intente los siguientes pasos:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Compruebe que la SPA se esté ejecutando en [http://localhost:3000](http://localhost:3000).

## Contenido de autor en AEM SPA Editor

Antes de crear contenido, organice las ventanas del explorador de modo que AEM Author (`http://localhost:4502`) esté a la izquierda y el SPA remoto (`http://localhost:3000`) se ejecute a la derecha. This arrangement allows you to see how changes to AEM-sourced content are immediately reflected in the SPA.

1. Log in to [AEM SDK Author service](http://localhost:4502) as `admin`
1. Navigate to __Sites > WKND App > us > en__
1. Edit __WKND App Home Page__
1. Switch to __Edit__ mode

### Author the Home view&#39;s fixed component

1. Tap on the text __WKND Adventures__ to activate the fixed Title component (hardcoded into the SPA&#39;s Home view)
1. Tap the __wrench__ icon on the Title component&#39;s action bar
1. Changes the Title component&#39;s content and save
1. Refresh the SPA running on `http://localhost:3000` and see that the changes reflected

### Author the Home view&#39;s container component

1. While still editing the __WKND App Home Page__...
1. Expand the __SPA Editor&#39;s sidebar__ (on the left)
1. Tap the __Components__ icons
1. Add, change, or remove components from the container component that sits beneath the WKND logo and above the fixed Title component
1. Refresh the SPA running on `http://localhost:3000` and see that the changes reflected

### Author a container component on a dynamic route

1. Switch to __Preview__ mode in SPA Editor
1. Tap on the __Bali Surf Camp__ card and navigate to its dynamic route
1. Add, change, or remove components from the container component that sites above the __Itinerary__ heading
1. Refresh the SPA running on `http://localhost:3000` and see that the changes reflected

New AEM pages under the __WKND App Home page > Adventure__ _must_ have an AEM page name that matches the corresponding adventure&#39;s Content Fragment&#39;s name. This is because the SPA route to AEM Page mapping is based off the last segment of the route, which is the Content Fragment&#39;s name.

## Enhorabuena.

You just got quick taste of how AEM SPA Editor can enhance your SPA with controlled, editable areas! If you&#39;re interested - check out the rest of the tutorial, but make sure to start fresh, since in this quick setup your local development environment is now in  end state of the tutorial!
