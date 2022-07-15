---
title: Configuración rápida AEM sin encabezado para AEM as a Cloud Service
description: La configuración rápida AEM sin encabezado le permite trabajar con AEM sin encabezado mediante el contenido del proyecto de muestra del sitio WKND y una aplicación React que consume el contenido a través de AEM API de GraphQL sin encabezado.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: b4c04a9ef7d8cfdaa5675fdfe259ab9d813fb7e0
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 2%

---

# Configuración rápida AEM sin encabezado para AEM as a Cloud Service

La configuración rápida AEM sin encabezado le permite trabajar con AEM sin encabezado mediante el contenido del proyecto de muestra del sitio WKND y una aplicación React (a SPA) de muestra que consume el contenido a través de AEM API de GraphQL sin encabezado.

## Requisitos previos

Para seguir esta configuración rápida es necesario lo siguiente:

+ AEM entorno limitado as a Cloud Service (preferiblemente desarrollo)
+ Acceso a AEM as a Cloud Service y Cloud Manager
   + __Administrador AEM__ acceso a AEM as a Cloud Service
   + __Cloud Manager: administrador de implementación__ acceso a Cloud Manager
+ Las siguientes herramientas deben instalarse localmente:
   + [Node.js v10+](https://nodejs.org/en/)
   + [npm 6+](https://www.npmjs.com/)
   + [Git](https://git-scm.com/)
   + Un IDE (por ejemplo, [Código Microsoft® Visual Studio](https://code.visualstudio.com/))

## 1. Crear un repositorio Git de Cloud Manager

En primer lugar, cree un repositorio Git de Cloud Manager que se utilice para implementar el sitio WKND. El sitio WKND es un proyecto de sitio web de muestra AEM que contiene contenido (fragmentos de contenido) y un extremo de AEM de GraphQL utilizado por la aplicación React de la configuración rápida.

_Descripción general de los pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339073/?quality=12&learn=on)

1. Vaya a [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Seleccione Cloud Manager __Programa__ que contiene el entorno as a Cloud Service AEM que se utilizará para esta configuración rápida
1. Creación de un repositorio de Git para el proyecto del sitio WKND
   1. Select __Repositorios__ en la barra de navegación superior
   1. Select __Agregar repositorio__ en la barra de acciones superior
   1. Asigne un nombre al nuevo repositorio de Git: `aem-headless-quick-setup-wknd`
      + Los nombres de repositorios de Git deben ser únicos por organización de Adobe,
   1. Select __Guardar__ y espere a que se inicialice el repositorio Git

## 2. Inserte un proyecto de sitio WKND de muestra en el repositorio Git de Cloud Manager

Con el repositorio Git de Cloud Manager creado, clone el código fuente del proyecto WKND Site de GitHub y lo insertaba en el repositorio Git de Cloud Manager. Ahora Cloud Manager tiene acceso e implementa el proyecto WKND Site en el entorno as a Cloud Service AEM.

_Descripción general de los pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339074/?quality=12&learn=on)

1. Desde la línea de comandos, clone el código fuente del proyecto WKND de ejemplo desde GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Añadir el repositorio Git de Cloud Manager como un repositorio remoto
   1. Select __Repositorios__ en la barra de navegación superior
   1. Select __Acceder a información de repositorios__ desde la barra de acciones superior
   1. Ejecutar comando encontrado en __Agregar un remoto a su repositorio de Git__ desde la línea de comandos

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Inserte el código fuente del proyecto de ejemplo desde el repositorio Git local al repositorio Git de Cloud Manager

   ```shell
   $ git push adobe main:main
   ```

   Cuando se le solicite credenciales, proporcione la variable __Nombre de usuario__ y __Contraseña__ de Cloud Manager __Información del repositorio__ modal.

## 3. Implementar el sitio WKND en AEM as a Cloud Service

Con el proyecto WKND Site insertado en el repositorio Git de Cloud Manager, no se puede implementar en AEM as a Cloud Service mediante canalizaciones de Cloud Manager.

Tenga en cuenta que el proyecto WKND Site proporciona contenido de muestra que la aplicación React consume sobre AEM API de GraphQL sin encabezado.

_Descripción general de los pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339075/?quality=12&learn=on)

1. Adjuntar un __Canalización de implementación sin producción__ al nuevo repositorio de Git
   1. Select __Canalizaciones__ en la barra de navegación superior
   1. Select __Añadir canalización__ desde la barra de acciones superior
   1. En el __Configuración__ ficha
      1. Select __Canalización de implementación__ option
      1. Configure las variables __Nombre de canalización que no es de producción__ a `Dev Deployment pipeline`
      1. Select __Déclencheur de implementación > Cambios en Git__
      1. Select __Comportamiento de errores de métricas importantes > Continuar inmediatamente__
      1. Select __Continuar__
   1. En la pestaña __Código fuente__
      1. Select __Código de pila completa__ option
      1. Seleccione el __AEM entorno de desarrollo as a Cloud Service__ de la variable __Entornos de implementación aptos__ cuadro de selección
      1. Select `aem-headless-quick-setup-wknd` en el __Repositorio__ cuadro de selección
      1. Select `main` de la variable __Rama de Git__ cuadro de selección
      1. Seleccione __Guardar__
1. Ejecute el __Canalización de implementación de desarrollo__
   1. Select __Información general__ en la barra de navegación superior
   1. Busque el __Canalización de implementación de desarrollo__ en el __Canalizaciones__ sección
   1. Seleccione el __...__ a la derecha de la entrada de canalización
   1. Select __Ejecutar__ y confirme en el modal
   1. Seleccione el __...__ a la derecha de la canalización que se está ejecutando
   1. Select __Ver detalles__
1. Desde los detalles de la ejecución de la canalización, supervise el progreso hasta que se complete correctamente. La ejecución de la canalización debe tardar entre 30 y 40 minutos.

## 4. Descargue y ejecute la aplicación WKND React

Con AEM arranque as a Cloud Service con el contenido del proyecto WKND Site, descargue e inicie la aplicación WKND React de muestra que consume el contenido del sitio WKND a través de AEM API de GraphQL sin encabezado.

_Descripción general de los pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339076/?quality=12&learn=on)

1. Desde la línea de comandos, clone el código fuente de la aplicación React desde GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra la carpeta . `~/Code/aem-guides-wknd-graphql/react-app` en su IDE.
1. En el IDE, abra el archivo `.env.development`.
1. Apunte al as a Cloud Service AEM __Publicación__ URI de host del servicio desde el  `REACT_APP_HOST_URI` propiedad.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   Para encontrar el URI de host del servicio de publicación as a Cloud Service de AEM:

   1. En Cloud Manager, seleccione __Entornos__ en la barra de navegación superior
   1. Select __Desarrollo__ entorno
   1. Busque la variable __Servicio de publicación (AEM y Dispatcher)__ vínculo __Segmentos de entorno__ tabla
   1. Copie la dirección del vínculo y utilícela como URI del servicio de publicación as a Cloud Service de AEM

1. En el IDE, guarde los cambios en `.env.development`
1. Desde la línea de comandos, ejecute la aplicación React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. La aplicación React, que se ejecuta localmente, comienza en [http://localhost:3000](http://localhost:3000) y muestra una lista de aventuras, que se obtienen de AEM as a Cloud Service mediante las API de GraphQL de AEM sin encabezado.

## 5. Editar contenido en AEM

Con la aplicación WKND React de muestra que se conecta y consume contenido de las API de GraphQL AEM sin encabezado, cree contenido en el servicio de AEM Author y vea cómo la experiencia de la aplicación React se actualiza de forma conjunta.

_Descripción general de los pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339077/?quality=12&learn=on)

1. Inicie sesión en AEM servicio de creación as a Cloud Service
1. Vaya a __Assets > Archivos > WKND Compartido > Inglés > Aventuras__
1. Abra el __Ciclismo en el sur de Utah__ Carpeta
1. Seleccione el __Ciclismo en el sur de Utah__ Fragmento de contenido y seleccione __Editar__ desde la barra de acciones superior
1. Actualice algunos de los campos del fragmento de contenido, por ejemplo:
   + Título: `Cycling Utah's National Parks`
   + Longitud del viaje: `6 Days`
   + Dificultad: `Intermediate`
   + Precio: `3500`
   + Imagen principal: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Select __Guardar__ en la barra de acciones superior
1. Select __Publicación rápida__ de la barra de acciones superior __...__
1. Actualizar la aplicación React que se ejecuta en [http://localhost:3000](http://localhost:3000).
1. En la aplicación React, seleccione la aventura de ciclismo ahora actualizada y verifique los cambios de contenido realizados en el fragmento de contenido.

1. Con el mismo enfoque, en el servicio de AEM Author:
   1. Cancele la publicación de un fragmento de contenido de aventura existente y compruebe que se haya eliminado de la experiencia React App
   1. Cree y publique un nuevo fragmento de contenido de aventura y verifique que aparezca en la experiencia React App

   >[!TIP]
   >
   > Si no está familiarizado con la creación y publicación de fragmentos de contenido nuevos o con la cancelación de la publicación de fragmentos de contenido existentes, consulte la proyección de pantalla anterior.

## Felicitaciones!

Felicitaciones! ¡Ha utilizado correctamente AEM sin encabezado para activar una aplicación React!

Para comprender en detalle cómo la aplicación React consume contenido de AEM as a Cloud Service, consulte [tutorial AEM Headless](../multi-step/overview.md). El tutorial explora cómo se crean los fragmentos de contenido en AEM y cómo esta aplicación React consume su contenido como JSON.

### Siguientes pasos

+ [Iniciar el tutorial AEM sin encabezado](../multi-step/overview.md)
