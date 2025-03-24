---
title: Configuración rápida de AEM sin encabezado para AEM as a Cloud Service
description: La configuración rápida de AEM sin encabezado le permite ponerse en contacto con AEM sin encabezado mediante el contenido del proyecto de muestra del sitio WKND y una aplicación React que consume el contenido a través de las API de GraphQL sin encabezado de AEM.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# Configuración rápida de AEM sin encabezado para AEM as a Cloud Service

La configuración rápida de AEM sin encabezado le permite ponerse en contacto con AEM sin encabezado mediante el contenido del proyecto de muestra del sitio WKND y una aplicación React de muestra (un SPA) que consume el contenido a través de las API de GraphQL sin encabezado de AEM.

## Requisitos previos

Se requiere lo siguiente para seguir esta configuración rápida:

+ Entorno de zona protegida de AEM as a Cloud Service (preferiblemente Desarrollo)
+ Acceso a AEM as a Cloud Service y Cloud Manager
   + Acceso de __administrador de AEM__ a AEM as a Cloud Service
   + __Cloud Manager - Administrador de implementación__ acceso a Cloud Manager
+ Las siguientes herramientas deben instalarse localmente:
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + Un IDE (por ejemplo, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1. Crear un repositorio de Git de Cloud Manager

En primer lugar, cree un repositorio Git de Cloud Manager que se utilice para implementar el sitio WKND. El sitio WKND es un proyecto de sitio web de AEM de ejemplo que contiene contenido (fragmentos de contenido) y un punto final AEM de GraphQL que utiliza la aplicación React de configuración rápida.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Vaya a [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Seleccione el __programa__ de Cloud Manager que contiene el entorno de AEM as a Cloud Service que se utilizará para esta configuración rápida
1. Cree un repositorio Git para el proyecto del sitio WKND
   1. Seleccione __Repositorios__ en la barra de navegación superior
   1. Seleccione __Agregar repositorio__ en la barra de acciones superior
   1. Asigne un nombre al nuevo repositorio Git: `aem-headless-quick-setup-wknd`
      + Los nombres de los repositorios de Git deben ser únicos para cada organización de Adobe,
   1. Seleccione __Guardar__ y espere a que se inicialice el repositorio Git

## 2. Insertar el proyecto del sitio WKND de muestra en el repositorio Git de Cloud Manager

Con el repositorio Git de Cloud Manager creado, clone el código fuente del proyecto del sitio WKND desde GitHub y lo inserta en el repositorio Git de Cloud Manager. Ahora, Cloud Manager permite acceder al proyecto del sitio WKND e implementarlo en el entorno de AEM as a Cloud Service.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. Desde la línea de comandos, clone el código fuente del proyecto WKND de muestra desde GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Añadir el repositorio Git de Cloud Manager como remoto
   1. Seleccione __Repositorios__ en la barra de navegación superior
   1. Seleccione __Acceder a la info del repositorio__ en la barra de acciones superior
   1. Se encontró el comando Execute en __Agregue un remoto a su repositorio Git__ desde la línea de comandos

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Inserte el código fuente del proyecto de ejemplo desde el repositorio Git local al repositorio Git de Cloud Manager

   ```shell
   $ git push adobe main:main
   ```

   Cuando se le soliciten credenciales, proporcione __Nombre de usuario__ y __Contraseña__ del modal __Información del repositorio__ de Cloud Manager.

## 3. Implementar el sitio WKND en AEM as a Cloud Service

Con el proyecto del sitio WKND insertado en el repositorio de Git de Cloud Manager, no se puede implementar en AEM as a Cloud Service mediante canalizaciones de Cloud Manager.

Tenga en cuenta que el proyecto del sitio WKND proporciona contenido de muestra que la aplicación React consume a través de las API de GraphQL sin encabezado de AEM.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Adjunte una __canalización de implementación que no sea de producción__ al nuevo repositorio Git
   1. Seleccione __Canalizaciones__ en la navegación superior
   1. Seleccione __Agregar canalización__ en la barra de acciones superior
   1. En la ficha __Configuración__
      1. Seleccionar la opción __Canalización de implementación__
      1. Establecer __nombre de canalización que no sea de producción__ en `Dev Deployment pipeline`
      1. Seleccione __Déclencheur de implementación > En cambios de Git__
      1. Seleccione __Comportamiento de errores de métricas importantes > Continuar inmediatamente__
      1. Seleccionar __Continuar__
   1. En la ficha __Código Source__
      1. Seleccione la opción __Código de pila completa__
      1. Seleccione el __entorno de desarrollo de AEM as a Cloud Service__ del cuadro de diálogo __Entornos de implementación aptos__
      1. Seleccione `aem-headless-quick-setup-wknd` en el cuadro de selección __Repositorio__
      1. Seleccione `main` del cuadro de selección __Rama de Git__
      1. Seleccionar __Guardar__
1. Ejecutar la __canalización de implementación de desarrolladores__
   1. Seleccione __Información general__ en la barra de navegación superior
   1. Busque la __canalización de implementación de desarrolladores__ recién creada en la sección __Canalizaciones__
   1. Seleccione __...__ a la derecha de la entrada de la canalización
   1. Seleccione __Ejecutar__ y confirme en el modal
   1. Seleccione __...__ a la derecha de la canalización en ejecución
   1. Seleccionar __Ver detalles__
1. Desde los detalles de ejecución de la canalización, monitorice el progreso hasta que se complete correctamente. La ejecución de la canalización debe tardar entre 30 y 40 minutos.

## 4. Descargue y ejecute la aplicación WKND React

Con AEM as a Cloud Service arrancado con el contenido del proyecto del sitio WKND, descargue e inicie la aplicación React de WKND de ejemplo que consume el contenido del sitio WKND a través de las API de GraphQL sin encabezado de AEM.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. Desde la línea de comandos, clone el código fuente de la aplicación React desde GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra la carpeta `~/Code/aem-guides-wknd-graphql/react-app` en su IDE.
1. En el IDE, abra el archivo `.env.development`.
1. Elija el URI de host del servicio AEM as a Cloud Service __Publish__ desde la propiedad `REACT_APP_HOST_URI`.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   Para encontrar el URI de host del servicio AEM as a Cloud Service Publish:

   1. En Cloud Manager, seleccione __Entornos__ en la barra de navegación superior
   1. Seleccione el entorno __Development__
   1. Busque la tabla __Servicio de publicación (AEM y Dispatcher)__ vínculo __Segmentos de entorno__
   1. Copie la dirección del vínculo y utilícela como URI del servicio de publicación de AEM as a Cloud Service

1. En el IDE, guarde los cambios en `.env.development`
1. Desde la línea de comandos, ejecute la aplicación React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. La aplicación React, que se ejecuta localmente, comienza en [http://localhost:3000](http://localhost:3000) y muestra una lista de aventuras, que provienen de AEM as a Cloud Service y que utilizan las API de GraphQL de AEM Headless.

## 5. Editar contenido en AEM

Con la aplicación React de WKND de ejemplo que se conecta a las API de GraphQL sin encabezado de AEM y consume contenido, cree contenido en el servicio AEM Author y vea cómo se actualiza de forma conjunta la experiencia de la aplicación React.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Iniciar sesión en el servicio de AEM as a Cloud Service Author
1. Vaya a __Assets > Archivos > WKND Compartido > Inglés > Aventuras__
1. Abrir la carpeta __Ciclismo en el sur de Utah__
1. Seleccione el fragmento de contenido __Ciclismo en el sur de Utah__ y seleccione __Editar__ en la barra de acciones superior
1. Actualice algunos de los campos del fragmento de contenido, por ejemplo:
   + Título: `Cycling Utah's National Parks`
   + Duración del viaje: `6 Days`
   + Dificultad: `Intermediate`
   + Precio: `3500`
   + Imagen principal: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Seleccione __Guardar__ en la barra de acciones superior
1. Seleccione __Publicación rápida__ en __de la barra de acciones superior...__
1. Actualice la aplicación React que se ejecuta en [http://localhost:3000](http://localhost:3000).
1. En la aplicación React, seleccione la aventura de ciclismo ahora actualizada y verifique los cambios de contenido realizados en el fragmento de contenido.

1. Con el mismo enfoque, en el servicio de AEM Author:
   1. Cancele la publicación de un fragmento de contenido de aventura existente y compruebe que se ha eliminado de la experiencia de la aplicación React
   1. Cree y publique un nuevo fragmento de contenido de aventura y verifique que aparece en la experiencia de la aplicación React

   >[!TIP]
   >
   > Si no está familiarizado con la creación y publicación de fragmentos de contenido nuevos o con la cancelación de la publicación de fragmentos de contenido existentes, vea la proyección de pantalla anterior.

## Enhorabuena.

¡Enhorabuena! ¡Ha utilizado correctamente AEM sin encabezado para impulsar una aplicación de React!

Para comprender en detalle cómo la aplicación React consume contenido de AEM as a Cloud Service, consulte [el tutorial sin encabezado de AEM](../multi-step/overview.md). El tutorial explora cómo se crearon los fragmentos de contenido en AEM y cómo esta aplicación de React consume su contenido como JSON.

### Siguientes pasos

+ [Inicio del tutorial de AEM sin encabezado](../multi-step/overview.md)
