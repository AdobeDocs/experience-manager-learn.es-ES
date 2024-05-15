---
title: SPA SPA Configuración rápida del Editor de segmentos de configuración rápida y remota
description: SPA AEM SPA ¡Aprenda a ponerse en marcha con un editor de y un editor remoto en 15 minutos
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 2%

---

# Configuración rápida

SPA AEM SPA La configuración rápida es una guía rápida que ilustra cómo instalar y ejecutar la aplicación WKND y como aplicación remota, y cómo crearla mediante el Editor de.

La configuración rápida le lleva directamente al estado final de este tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Un recorrido en vídeo de la configuración rápida_

## Requisitos previos

Este tutorial requiere lo siguiente:

+ [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Requisitos previos solo de macOS
   + [Xcode](https://developer.apple.com/xcode/) o [Herramientas de línea de comandos de Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip o superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [código fuente aem-guides-wknd-graphql (rama: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Este tutorial supone lo siguiente:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) como IDE
+ Un directorio de trabajo de `~/Code/wknd-app`
+ AEM Ejecución del SDK de la como servicio de autor en `http://localhost:4502`
+ AEM Ejecución del SDK de la con la variable local `admin` cuenta con contraseña `admin`
+ SPA Ejecución de la `http://localhost:3000`

## AEM Inicio rápido del SDK de

AEM Descargue e instale Quickstart de SDK de en el puerto 4502 de forma predeterminada `admin/admin` credenciales.

1. [AEM Descargar el último SDK de](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. AEM Descomprima el SDK de la en `~/aem-sdk`
1. AEM Ejecutar el Jar de inicio rápido del SDK de

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM El SDK de se inicia y se inicia automáticamente en [http://localhost:4502](http://localhost:4502). Inicie sesión con las siguientes credenciales:

+ Nombre de usuario: `admin`
+ Contraseña: `admin`

## Descargar e instalar el paquete del sitio WKND

Este tutorial depende de lo siguiente __WKND 2.1.0+__ proyecto (para contenido).

1. [Descargue la versión más reciente de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. AEM Inicie sesión en el Administrador de paquetes del SDK de la en [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con el `admin` credenciales.
1. __Cargar__ el `aem-guides-wknd.all.x.x.x.zip` descargado en el paso 1
1. Pulse el botón __Instalar__ botón para la entrada `aem-guides-wknd.all-x.x.x.zip`

## SPA Descargar e instalar paquetes de WKND App

AEM AEM Para realizar una configuración rápida, aquí se proporcionan paquetes de que contienen la configuración y el contenido finales de la aplicación de aprendizaje del tutorial.

1. [Descargar ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Descargar ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. AEM Inicie sesión en el Administrador de paquetes del SDK de la en [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con el `admin` credenciales.
1. __Cargar__ el `wknd-app.all.x.x.x.zip` descargado en el paso 1
1. Pulse el botón __Instalar__ botón para la entrada `wknd-app.all.x.x.x.zip`
1. __Cargar__ el `wknd-app.ui.content.sample.x.x.x.zip` descargado en el paso 2
1. Pulse el botón __Instalar__ botón para la entrada `wknd-app.ui.content.sample.x.x.x.zip`

## Descargar el origen de la aplicación WKND

SPA Descargue el código fuente de la aplicación WKND desde Github.com y cambie la rama que contiene los cambios en la realizados en este tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## SPA Inicio de la aplicación de

SPA Desde la raíz del proyecto, instale las dependencias npm de los proyectos de la y ejecute la aplicación.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Si hay errores al ejecutar `npm install` intente los siguientes pasos:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPA Verificar que la se esté ejecutando en [http://localhost:3000](http://localhost:3000).

## AEM SPA Contenido del autor en el editor de

AEM Antes de crear contenido, organice las ventanas del navegador de modo que, a su vez, se pueda crear contenido de autor (`http://localhost:4502`SPA ) está a la izquierda, y el control remoto está en la parte de la parte de la parte de la parte de la parte de abajo (`http://localhost:3000`) se ejecuta a la derecha. AEM SPA Esta disposición le permite ver cómo los cambios en el contenido de origen de la se reflejan inmediatamente en la.

1. Iniciar sesión en [AEM Servicio de autor de SDK](http://localhost:4502) as `admin`
1. Vaya a __Sites > Aplicación WKND > us > es__
1. Editar __Página de inicio de la aplicación WKND__
1. Cambiar a __Editar__ modo

### Crear el componente fijo de la vista Inicio

1. Toque el texto __WKND Adventures__ SPA para activar el componente Título fijo (codificado en la vista Inicio de la aplicación de la vista de página de inicio de)
1. Pulse el botón __llave__ en la barra de acciones del componente Título
1. Cambia el contenido del componente Título y lo guarda
1. SPA Actualizar la ejecución de la en `http://localhost:3000` y ver que los cambios reflejados

### Crear el componente contenedor de la vista Inicio

1. Mientras se sigue editando el __Página de inicio de la aplicación WKND__...
1. Expanda el __SPA Barra lateral del editor de__ (a la izquierda)
1. Pulse el botón __Componentes__ iconos
1. Agregar, cambiar o quitar componentes del componente contenedor que se encuentra debajo del logotipo de WKND y encima del componente de título fijo
1. SPA Actualizar la ejecución de la en `http://localhost:3000` y ver que los cambios reflejados

### Crear un componente de contenedor en una ruta dinámica

1. Cambiar a __Previsualizar__ SPA modo en el editor de
1. Pulse en __Bali Surf Camp__ y vaya a su ruta dinámica
1. Agregar, cambiar o quitar componentes del componente contenedor que se coloca encima de __Itinerario__ encabezado
1. SPA Actualizar la ejecución de la en `http://localhost:3000` y ver que los cambios reflejados

AEM Nuevas páginas de la lista de permitidos __Página de inicio de la aplicación WKND > Aventura__ _debe_ AEM Tener un nombre de página de que coincida con el nombre del fragmento de contenido de la aventura correspondiente. SPA AEM Esto se debe a que la ruta de la a la asignación de página de la página se basa en el último segmento de la ruta, que es el nombre del fragmento de contenido.

## Enhorabuena.

AEM SPA SPA ¡Solo tienes una idea rápida de cómo puede mejorar tu con áreas controladas y editables de la manera en que el editor de lo hace! Si está interesado, consulte el resto del tutorial, pero asegúrese de empezar de nuevo, ya que en esta configuración rápida su entorno de desarrollo local ahora está en estado final del tutorial.
