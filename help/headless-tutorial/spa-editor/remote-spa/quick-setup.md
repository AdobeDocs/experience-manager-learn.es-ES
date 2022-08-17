---
title: Configuración rápida SPA Editor y SPA remoto
description: Aprenda a ponerse en marcha con un SPA remoto y AEM SPA Editor en 15 minutos!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 5%

---

# Configuración rápida

La configuración rápida es una guía rápida que ilustra cómo instalar y ejecutar la aplicación WKND y como SPA remoto, y crearla con AEM SPA Editor.

La configuración rápida le lleva directamente al estado final de este tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_Un vídeo explicativo de la configuración rápida_

## Requisitos previos

Este tutorial requiere lo siguiente:

+ [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Requisitos previos solo de macOS
   + [Xcode](https://developer.apple.com/xcode/) o [Herramientas de la línea de comandos de Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip o bueno](https://github.com/adobe/aem-guides-wknd/releases)
+ [código fuente aem-guides-wknd-graphql (rama: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Este tutorial supone:

+ [Código Microsoft® Visual Studio](https://visualstudio.microsoft.com/) como el IDE
+ Un directorio de trabajo de `~/Code/wknd-app`
+ Ejecución del SDK de AEM como servicio de creación en `http://localhost:4502`
+ Ejecución del SDK de AEM con el `admin` cuenta con contraseña `admin`
+ Ejecución del SPA en `http://localhost:3000`

## Inicio rápido del SDK de AEM

Descargue e instale el AEM SDK Quickstart en el puerto 4502, con `admin/admin` credenciales.

1. [Descargar el SDK de AEM más reciente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Descomprima el SDK de AEM a `~/aem-sdk`
1. Ejecutar el Jar de inicio rápido del SDK de AEM

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK se inicia y se inicia automáticamente en [http://localhost:4502](http://localhost:4502). Inicie sesión con las siguientes credenciales:

+ Nombre de usuario: `admin`
+ Contraseña: `admin`

## Descargar e instalar el paquete WKND Site

Este tutorial depende de __WKND 2.1.0+__ proyecto (para contenido).

1. [Descargue la versión más reciente de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Inicie sesión en AEM Administrador de paquetes del SDK en [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con la variable `admin` credenciales.
1. __Cargar__ el `aem-guides-wknd.all.x.x.x.zip` descargado en el paso 1
1. Toque . __Instalar__ para la entrada `aem-guides-wknd.all-x.x.x.zip`

## Descargar e instalar paquetes de SPA de aplicaciones WKND

Para realizar una configuración rápida, aquí se proporcionan AEM paquetes que contienen la configuración y el contenido de AEM final del tutorial.

1. [Descargar ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Descargar ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Inicie sesión en AEM Administrador de paquetes del SDK en [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) con la variable `admin` credenciales.
1. __Cargar__ el `wknd-app.all.x.x.x.zip` descargado en el paso 1
1. Toque . __Instalar__ para la entrada `wknd-app.all.x.x.x.zip`
1. __Cargar__ el `wknd-app.ui.content.sample.x.x.x.zip` descargado en el paso 2
1. Toque . __Instalar__ para la entrada `wknd-app.ui.content.sample.x.x.x.zip`

## Descargar la fuente de la aplicación WKND

Descargue el código fuente de la aplicación WKND de Github.com y cambie la rama que contiene los cambios a la SPA realizada en este tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Inicie la aplicación SPA

Desde la raíz del proyecto, instale las dependencias npm de SPA proyectos y ejecute la aplicación.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Si hay errores al ejecutar `npm install` pruebe los siguientes pasos:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Compruebe que la SPA se esté ejecutando en [http://localhost:3000](http://localhost:3000).

## Creación de contenido en AEM editor SPA

Antes de crear contenido, organice las ventanas del navegador de forma que AEM Author (`http://localhost:4502`) está a la izquierda y el SPA remoto (`http://localhost:3000`) se ejecuta a la derecha. Esta disposición le permite ver cómo los cambios en el contenido AEM se reflejan inmediatamente en la SPA.

1. Iniciar sesión en [AEM SDK Author service](http://localhost:4502) como `admin`
1. Vaya a __Sites > Aplicación WKND > us > es__
1. Editar __Página principal de la aplicación WKND__
1. Cambie a __Editar__ mode

### Cree el componente fijo de la vista Inicio

1. Toque en el texto __Aventuras de WKND__ para activar el componente de título fijo (codificado en la vista Inicio de SPA)
1. Toque . __llave__ en la barra de acciones del componente Título
1. Cambia el contenido del componente Título y guárdelo
1. Actualice el SPA que se está ejecutando en `http://localhost:3000` y ver que los cambios reflejados

### Creación del componente contenedor de la vista Inicio

1. Mientras sigue editando el __Página principal de la aplicación WKND__...
1. Expanda el __Barra lateral del Editor de SPA__ (a la izquierda)
1. Toque . __Componentes__ iconos
1. Añadir, cambiar o quitar componentes del componente contenedor que se encuentra debajo del logotipo de WKND y encima del componente de título fijo
1. Actualice el SPA que se está ejecutando en `http://localhost:3000` y ver que los cambios reflejados

### Creación de un componente contenedor en una ruta dinámica

1. Cambie a __Vista previa__ en SPA Editor
1. Toque en la __Campo de surf de Bali__ y vaya a su ruta dinámica
1. Agregar, cambiar o quitar componentes del componente contenedor que se encuentra encima del __Itinerario__ encabezado
1. Actualice el SPA que se está ejecutando en `http://localhost:3000` y ver que los cambios reflejados

Las páginas de AEM nuevas en la sección __Página de inicio de la aplicación WKND > Aventura__ _must_ tienen un nombre de página AEM que coincida con el nombre del fragmento de contenido de la aventura correspondiente. Esto se debe a que la ruta SPA a AEM asignación de página se basa en el último segmento de la ruta, que es el nombre del fragmento de contenido.

## Felicitaciones!

Solo tienes que probar rápidamente cómo AEM editor de SPA puede mejorar tu SPA con áreas controladas y editables. Si le interesa, consulte el resto del tutorial, pero asegúrese de empezar de nuevo, ya que en esta configuración rápida su entorno de desarrollo local está en el estado final del tutorial.
