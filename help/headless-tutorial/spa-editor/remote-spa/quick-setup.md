---
title: Configuración rápida de Editor de SPA y SPA remota
description: Aprenda a ponerse en marcha con un SPA remoto y un editor de SPA de AEM en 15 minutos.
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
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 10%

---

# Configuración rápida

{{spa-editor-deprecation}}

La configuración rápida es una guía rápida que ilustra cómo instalar y ejecutar la aplicación WKND y como SPA remota, y crearla con el Editor de SPA de AEM.

La configuración rápida le lleva directamente al estado final de este tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Un recorrido en vídeo de la configuración rápida_

## Requisitos previos

Este tutorial requiere lo siguiente:

+ El [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=es)
+ [Node.js, versión 18](https://nodejs.org/es/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Requisitos previos solo de macOS
   + [Xcode](https://developer.apple.com/xcode/) o [herramientas de línea de comandos de Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip o superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [código fuente aem-guides-wknd-graphql (rama: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Este tutorial utiliza lo siguiente:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) como IDE
+ Un directorio de trabajo de `~/Code/wknd-app`
+ Ejecución del SDK de AEM como un servicio de Author en `http://localhost:4502`
+ Ejecución del SDK de AEM con la cuenta local `admin` con la contraseña `admin`
+ Ejecución de la SPA en `http://localhost:3000`

## Inicio rápido de AEM SDK

Descargue e instale AEM SDK Quickstart en el puerto 4502, con las credenciales predeterminadas `admin/admin`.

1. [Descargar la última versión de AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. Descomprima AEM SDK en `~/aem-sdk`
1. Ejecute el Jar de inicio rápido de AEM SDK

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK se inicia automáticamente en [http://localhost:4502](http://localhost:4502). Inicie sesión con las siguientes credenciales:

+ Nombre de usuario: `admin`
+ Contraseña: `admin`

## Descargar e instalar el paquete del sitio WKND

Este tutorial depende del proyecto __WKND 2.1.0+&#39;s__ (para contenido).

1. [Descargar la versión más reciente de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Inicie sesión en el Administrador de paquetes de AEM SDK en [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con las credenciales de `admin`.
1. __Cargar__ `aem-guides-wknd.all.x.x.x.zip` descargado en el paso 1
1. Pulse el botón __Instalar__ para la entrada `aem-guides-wknd.all-x.x.x.zip`

## Descargar e instalar paquetes de la SPA de la aplicación WKND

Para realizar una configuración rápida, aquí se proporcionan paquetes de AEM que contienen la configuración y el contenido finales de AEM del tutorial.

1. [Descargar ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Descargar ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
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

Antes de crear contenido, organice las ventanas del explorador de modo que AEM Author (`http://localhost:4502`) esté a la izquierda y el SPA remoto (`http://localhost:3000`) se ejecute a la derecha. Esta disposición le permite ver cómo los cambios en el contenido procedente de AEM se reflejan inmediatamente en la SPA.

1. Inicie sesión en [AEM SDK Author service](http://localhost:4502) como `admin`
1. Vaya a __Sitios > Aplicación WKND > us > en__
1. Editar __página principal de la aplicación WKND__
1. Cambiar a modo __Editar__

### Crear el componente fijo de la vista Inicio

1. Pulse el texto __WKND Adventures__ para activar el componente Título fijo (codificado en la vista Inicio de la SPA)
1. Pulse el icono __llave inglesa__ en la barra de acciones del componente Título
1. Cambia el contenido del componente Título y lo guarda
1. Actualice la SPA que se ejecuta el `http://localhost:3000` y vea que los cambios se reflejan

### Crear el componente contenedor de la vista Inicio

1. Mientras sigue editando la __página principal de la aplicación WKND__...
1. Expandir la __barra lateral del editor de SPA__ (a la izquierda)
1. Pulse los iconos __Componentes__
1. Agregar, cambiar o quitar componentes del componente contenedor que se encuentra debajo del logotipo de WKND y encima del componente de título fijo
1. Actualice la SPA que se ejecuta el `http://localhost:3000` y vea que los cambios se reflejan

### Crear un componente de contenedor en una ruta dinámica

1. Cambiar al modo __Vista previa__ en el Editor de SPA
1. Toca la tarjeta __Bali Surf Camp__ y navega a su ruta dinámica
1. Agregar, cambiar o quitar componentes del componente contenedor que se encuentra sobre el encabezado __Itinerario__
1. Actualice la SPA que se ejecuta el `http://localhost:3000` y vea que los cambios se reflejan

Las nuevas páginas de AEM bajo la __página de inicio de la aplicación WKND > Aventura__ _deben_ tener una página de AEM que coincida con el nombre del fragmento de contenido de la aventura correspondiente. Esto se debe a que la asignación de ruta de SPA a página de AEM se basa en el último segmento de la ruta, que es el nombre del fragmento de contenido.

## Enhorabuena.

Acaba de probar rápidamente cómo AEM SPA Editor puede mejorar su SPA con áreas controladas y editables. Si está interesado, consulte el resto del tutorial, pero asegúrese de empezar de nuevo, ya que en esta configuración rápida su entorno de desarrollo local ahora está en estado final del tutorial.
