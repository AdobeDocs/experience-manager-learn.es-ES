---
title: AEM AEM Configuración rápida sin encabezado para la configuración as a Cloud Service de la
description: AEM AEM AEM La configuración rápida sin encabezado le permite ponerse en contacto con el sin encabezado mediante el contenido del proyecto de muestra del sitio WKND y con una aplicación de React que consume el contenido a través de las API de GraphQL sin encabezado, lo que le ayuda a utilizar el contenido sin encabezado.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# AEM AEM Configuración rápida sin encabezado para la configuración as a Cloud Service de la

AEM AEM SPA AEM La configuración rápida sin encabezado le permite ponerse en contacto con el contenido sin encabezado mediante el contenido del proyecto de muestra del sitio WKND y una aplicación React de muestra (una aplicación de react) que consume el contenido a través de las API de GraphQL sin encabezado, lo que le ayuda a utilizar el contenido sin encabezado.

## Requisitos previos

Se requiere lo siguiente para seguir esta configuración rápida:

+ AEM Entorno de espacio aislado as a Cloud Service (preferiblemente Desarrollo)
+ AEM Acceso a as a Cloud Service y Cloud Manager
   + __AEM Administrador de__ AEM acceso a la as a Cloud Service
   + __Cloud Manager: Administrador de implementación__ acceso a Cloud Manager
+ Las siguientes herramientas deben instalarse localmente:
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + Un IDE (por ejemplo, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1. Crear un repositorio de Git de Cloud Manager

En primer lugar, cree un repositorio Git de Cloud Manager que se utilice para implementar el sitio WKND. AEM El sitio WKND es un ejemplo de proyecto de sitio web que contiene contenido (fragmentos de contenido) y un punto final de GraphQL AEM que utiliza la aplicación React de configuración rápida.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Vaya a [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Seleccione Cloud Manager __Programa__ AEM que contiene el entorno as a Cloud Service de la que se utilizará para esta configuración rápida
1. Cree un repositorio Git para el proyecto del sitio WKND
   1. Seleccionar __Repositorios__ en la barra de navegación superior
   1. Seleccionar __Añadir repositorio__ en la barra de acciones superior
   1. Asigne un nombre al nuevo repositorio Git: `aem-headless-quick-setup-wknd`
      + Los nombres de los repositorios de Git deben ser únicos para cada organización de Adobe,
   1. Seleccionar __Guardar__ y espere a que se inicialice el repositorio Git

## 2. Inserte el proyecto del sitio WKND de muestra en el repositorio Git de Cloud Manager

Con el repositorio Git de Cloud Manager creado, clone el código fuente del proyecto del sitio WKND desde GitHub y lo inserta en el repositorio Git de Cloud Manager. AEM Ahora, Cloud Manager para acceder al proyecto del sitio WKND e implementarlo en el entorno as a Cloud Service de la.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. Desde la línea de comandos, clone el código fuente del proyecto WKND de muestra desde GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Añadir el repositorio Git de Cloud Manager como remoto
   1. Seleccionar __Repositorios__ en la barra de navegación superior
   1. Seleccionar __Acceder a info del repositorio__ desde la barra de acciones superior
   1. Ejecutar comando encontrado en __Añada un remoto a su repositorio de Git__ desde la línea de comandos

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Inserte el código fuente del proyecto de muestra desde el repositorio Git local al repositorio Git de Cloud Manager

   ```shell
   $ git push adobe main:main
   ```

   Cuando se le soliciten credenciales, proporcione el __Nombre de usuario__ y __Contraseña__ desde el de Cloud Manager __Información del repositorio__ modal.

## AEM as a Cloud Service 3. Implementar el sitio WKND para

AEM Con el proyecto del sitio WKND insertado en el repositorio Git de Cloud Manager, no se puede implementar en as a Cloud Service mediante canalizaciones de Cloud Manager.

AEM Tenga en cuenta que el proyecto del sitio WKND proporciona contenido de muestra que la aplicación React consume a través de las API de GraphQL sin encabezado de la red de distribución de contenido.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Adjuntar un __Canalización de implementación que no es de producción__ al nuevo repositorio de Git
   1. Seleccionar __Canalizaciones__ en la barra de navegación superior
   1. Seleccionar __Agregar canalización__ desde la barra de acciones superior
   1. En el __Configuración__ pestaña
      1. Seleccionar __Canalización de implementación__ opción
      1. Configure las variables __Nombre de la canalización que no es de producción__ hasta `Dev Deployment pipeline`
      1. Seleccionar __Déclencheur de implementación > Cambios en Git__
      1. Seleccionar __Comportamiento de los errores de las métricas importantes > Continuar inmediatamente__
      1. Seleccionar __Continuar__
   1. En el __Código fuente__ pestaña
      1. Seleccionar __Código de pila completa__ opción
      1. Seleccione el __AEM entorno de desarrollo as a Cloud Service__ desde el __Entornos de implementación aptos__ cuadro de selección
      1. Seleccionar `aem-headless-quick-setup-wknd` en el __Repositorio__ cuadro de selección
      1. Seleccionar `main` desde el __Rama Git__ cuadro de selección
      1. Seleccionar __Guardar__
1. Ejecute el __Canalización de implementación de desarrolladores__
   1. Seleccionar __Información general__ en la barra de navegación superior
   1. Busque el recién creado __Canalización de implementación de desarrollo__ en el __Canalizaciones__ sección
   1. Seleccione el __...__ a la derecha de la entrada de la canalización
   1. Seleccionar __Ejecutar__ y confirme en el modal
   1. Seleccione el __...__ a la derecha de la canalización en ejecución
   1. Seleccionar __Ver detalles__
1. Desde los detalles de ejecución de la canalización, monitorice el progreso hasta que se complete correctamente. La ejecución de la canalización debe tardar entre 30 y 40 minutos.

## 4. Descargue y ejecute la aplicación WKND React

AEM Con el contenido del proyecto WKND Site as a Cloud Service AEM, descargue e inicie la aplicación React de WKND de ejemplo que consume el contenido del sitio WKND a través de las API de GraphQL sin encabezado.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. Desde la línea de comandos, clone el código fuente de la aplicación React desde GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra la carpeta `~/Code/aem-guides-wknd-graphql/react-app` en su IDE.
1. En el IDE, abra el archivo `.env.development`.
1. AEM Apuntar a la as a Cloud Service de la __Publish__ URI de host del servicio desde  `REACT_APP_HOST_URI` propiedad.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   AEM Para encontrar el URI de host del servicio Publicación as a Cloud Service de la:

   1. En Cloud Manager, seleccione __Entornos__ en la barra de navegación superior
   1. Seleccionar __Desarrollo__ entorno
   1. Busque el __AEM Servicio de publicación (y Dispatcher)__ vincular __Segmentos de entorno__ tabla
   1. AEM Copie la dirección del vínculo y utilícela como URI del servicio de publicación as a Cloud Service de la

1. En el IDE, guarde los cambios en `.env.development`
1. Desde la línea de comandos, ejecute la aplicación React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. La aplicación React, que se ejecuta localmente, comienza el [http://localhost:3000](http://localhost:3000) AEM y muestra un listado de aventuras, que provienen de las API de GraphQL de sin encabezado, que se encuentran as a Cloud Service AEM de la manera en que se originan.

## AEM 5. Editar contenido en la

AEM Con la aplicación React de WKND de ejemplo que se conecta a las API de GraphQL AEM sin encabezado y consume contenido de ellas, cree contenido en el servicio de autor de y vea cómo se actualiza de forma conjunta la experiencia de la aplicación React.

_Screencast de pasos_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. AEM Inicie sesión en el servicio de autor as a Cloud Service de
1. Vaya a __Recursos > Archivos > WKND Compartido > Inglés > Aventuras__
1. Abra el __Ciclismo en el sur de Utah__ Carpeta
1. Seleccione el __Ciclismo en el sur de Utah__ Fragmento de contenido y seleccione __Editar__ desde la barra de acciones superior
1. Actualice algunos de los campos del fragmento de contenido, por ejemplo:
   + Título: `Cycling Utah's National Parks`
   + Duración del viaje: `6 Days`
   + Dificultad: `Intermediate`
   + Precio: `3500`
   + Imagen principal: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Seleccionar __Guardar__ en la barra de acciones superior
1. Seleccionar __Publicación rápida__ desde las barras de acciones superiores __...__
1. Actualice la aplicación de React que se ejecuta en [http://localhost:3000](http://localhost:3000).
1. En la aplicación React, seleccione la aventura de ciclismo ahora actualizada y verifique los cambios de contenido realizados en el fragmento de contenido.

1. AEM Con el mismo enfoque, en el servicio de autor de:
   1. Cancele la publicación de un fragmento de contenido de aventura existente y compruebe que se ha eliminado de la experiencia de la aplicación React
   1. Cree y publique un nuevo fragmento de contenido de aventura y verifique que aparece en la experiencia de la aplicación React

   >[!TIP]
   >
   > Si no está familiarizado con la creación y publicación de fragmentos de contenido nuevos o con la cancelación de la publicación de fragmentos de contenido existentes, vea la proyección de pantalla anterior.

## Enhorabuena.

Enhorabuena. AEM ¡Ha utilizado con éxito la tecnología sin encabezado para alimentar una aplicación de React!

AEM Para comprender en detalle cómo la aplicación React consume contenido de los as a Cloud Service de la aplicación, consulte: [AEM el tutorial sin encabezado de](../multi-step/overview.md). AEM El tutorial explora cómo se crean los fragmentos de contenido en los y cómo esta aplicación de React consume su contenido como JSON.

### Siguientes pasos

+ [AEM Iniciar el tutorial sin encabezado de la](../multi-step/overview.md)
