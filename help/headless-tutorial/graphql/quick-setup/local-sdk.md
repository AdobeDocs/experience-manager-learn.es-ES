---
title: AEM AEM Configuración rápida sin encabezado mediante el SDK local de la
description: Introducción a Adobe Experience Manager AEM () y GraphQL. AEM AEM Instale el SDK de la, añada contenido de ejemplo e implemente una aplicación que consuma contenido de la aplicación mediante sus API de GraphQL mediante el uso de la API de. AEM Consulte cómo alimenta la experiencia omnicanal la.
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 2%

---

# AEM AEM Configuración rápida sin encabezado mediante el SDK local de la {#setup}

AEM AEM SPA AEM La configuración rápida sin encabezado le permite ponerse en contacto con el contenido sin encabezado mediante el contenido del proyecto de muestra del sitio WKND y una aplicación React de muestra (una aplicación de react) que consume el contenido a través de las API de GraphQL sin encabezado, lo que le ayuda a utilizar el contenido sin encabezado. Esta guía utiliza el [AEM SDK as a Cloud Service de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html).

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v18](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

## AEM 1. Instalar el SDK de la {#aem-sdk}

Esta configuración utiliza el [AEM SDK as a Cloud Service de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) AEM para explorar las API de GraphQL de la. AEM Esta sección proporciona una guía rápida para instalar el SDK de la y ejecutarlo en el modo Autor. Una guía más detallada para configurar un entorno de desarrollo local [se puede encontrar aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up).

>[!NOTE]
>
> También es posible seguir el tutorial con un [AEM entorno as a Cloud Service](./cloud-service.md). A lo largo del tutorial se incluyen notas adicionales para utilizar un entorno de nube.

1. Vaya a **[Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** y descargue la versión más reciente de **AEM SDK de**.

   ![Portal de distribución de software](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. Descomprima la descarga y copie el JAR de inicio rápido (`aem-sdk-quickstart-XXX.jar`) a una carpeta específica, por ejemplo, `~/aem-sdk/author`.
1. Cambie el nombre del archivo jar a `aem-author-p4502.jar`.

   El `author` name especifica que el JAR de inicio rápido se inicia en el modo Autor. El `p4502` especifica que Quickstart se ejecuta en el puerto 4502.

1. AEM Para instalar e iniciar la instancia de, abra un símbolo del sistema en la carpeta que contiene el archivo jar y ejecute el siguiente comando:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Proporcione una contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizarla `admin` para el desarrollo local con el fin de reducir la necesidad de reconfigurar.
1. AEM Una vez que el servicio de termine de instalarse, se abrirá una nueva ventana del explorador en [http://localhost:4502](http://localhost:4502).
1. Inicie sesión con el nombre de usuario `admin` AEM y la contraseña seleccionada durante el inicio de la aplicación (normalmente, durante la fase inicial). `admin`).

## 2. Instalar contenido de muestra {#install-sample-content}

Contenido de muestra de **Sitio de referencia de WKND** se utiliza para acelerar el tutorial. AEM La WKND es una marca ficticia de estilo de vida, que a menudo se utiliza con el entrenamiento de la.

El sitio WKND incluye las configuraciones necesarias para exponer una [Extremo de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html). En una implementación real, siga los pasos documentados para [incluir los extremos de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) en el proyecto de cliente. A [CORS](#cors-config) también se ha empaquetado como parte del sitio WKND. Se requiere una configuración de CORS para conceder acceso a una aplicación externa, más información acerca de [CORS](#cors-config) se puede encontrar a continuación.

1. AEM Descargue el último paquete de datos compilado para el sitio WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Asegúrese de descargar la versión estándar compatible con las versiones as a Cloud Service y AEM de **no** el `classic` versión.

1. Desde el **AEM Inicio de** , vaya a **Herramientas** > **Implementación** > **Paquetes**.

   ![Navegar a paquetes](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. Clic **Cargar paquete** y elija el paquete WKND descargado en el paso anterior. Haga clic en **Instalar** para instalar el paquete.

1. Desde el **AEM Inicio de** , vaya a **Assets** > **Archivos** > **WKND compartido** > **Inglés** > **Aventuras**.

   ![Vista de carpeta de aventuras](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   Esta es una carpeta de todos los activos que componen las distintas Aventuras promocionadas por la marca WKND. AEM Esto incluye tipos de medios tradicionales, como imágenes y vídeo, y medios específicos de los que se puede hacer uso, como los siguientes: **Fragmentos de contenido**.

1. Haga clic en **Esquí de descenso Wyoming** y haga clic en la **Fragmento de contenido de Downhill Skiing Wyoming** tarjeta:

   ![Tarjeta de fragmento de contenido](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. El editor de fragmentos de contenido se abre para la aventura de Downhill Skiing Wyoming.

   ![Editor de fragmentos de contenido](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   Observe que varios campos como **Título**, **Descripción**, y **Actividad** defina el fragmento.

   **Fragmentos de contenido** AEM Estas son una de las formas en que se puede administrar el contenido en los recursos de la. Los fragmentos de contenido son contenidos reutilizables y no relacionados con la presentación compuestos por elementos de datos estructurados, como texto, texto enriquecido, fechas o referencias a otros fragmentos de contenido. Los fragmentos de contenido se exploran en buenos detalles más adelante en la configuración rápida.

1. Clic **Cancelar** para cerrar el fragmento. Siéntase libre de navegar en algunas de las otras carpetas y explorar el otro contenido de Aventura.

>[!NOTE]
>
> Si utiliza un entorno de Cloud Service, consulte la documentación para saber cómo [implementar una base de código como el sitio de referencia de WKND en un entorno de Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version).

## 3. Descargar y ejecutar la aplicación WKND React {#sample-app}

AEM Uno de los objetivos de este tutorial es mostrar cómo consumir contenido de la aplicación desde una aplicación externa mediante las API de GraphQL. Este tutorial utiliza un ejemplo de la aplicación React. AEM La aplicación React es intencionadamente sencilla, para centrarse en la integración con las API de GraphQL de la red de aplicaciones (API) de la red de redes sociales de.

1. Abra un nuevo símbolo del sistema y clone la aplicación React de ejemplo desde GitHub:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Abra la aplicación React en `aem-guides-wknd-graphql/react-app` en el IDE que elija.
1. En el IDE, abra el archivo `.env.development` en `/.env.development`. Compruebe el `REACT_APP_AUTHORIZATION` La línea no está comentada y el archivo declara las siguientes variables:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Asegurar `REACT_APP_HOST_URI` AEM apunta a su SDK de local. Para mayor comodidad, este inicio rápido conecta la aplicación React con  **AEM Author**. **Autor** requieren autenticación, por lo que la aplicación utiliza el `admin` para establecer su conexión. La conexión de una aplicación al Autor de AEM es una práctica común durante el desarrollo, ya que facilita la iteración rápida en el contenido sin necesidad de publicar cambios.

   >[!NOTE]
   >
   > AEM En un escenario de producción, la aplicación se conectará a una aplicación de tipo de conexión **Publish** entorno. Esto se explica con más detalle en la _Implementación de producción_ sección.


1. Instale e inicie la aplicación React:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Una nueva ventana del explorador abre automáticamente la aplicación en [http://localhost:3000](http://localhost:3000).

   ![Reaccionar aplicación de inicio](assets/quick-setup/aem-sdk/react-app__home-view.png)

   AEM Se muestra una lista de los contenidos de aventura de la.

1. Haga clic en una de las imágenes de aventura para ver los detalles de la aventura. AEM Se hace una petición para que se le devuelva el detalle de una aventura.

   ![Vista de detalles de aventura](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. Utilice las herramientas para desarrolladores del explorador para inspeccionar el **Red** solicitudes. Ver el **XHR** solicitudes de y observe varias solicitudes de GET a `/graphql/execute.json/...`. AEM Este prefijo de ruta invoca el punto de conexión de consulta persistente, seleccionando la consulta persistente que se ejecutará con el nombre y los parámetros codificados que siguen al prefijo.

   ![Solicitud XHR de extremo de GraphQL](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## AEM 4. Edite el contenido en la

AEM Con la aplicación React en ejecución, realice una actualización del contenido en la aplicación y compruebe que el cambio se refleje en la aplicación.

1. AEM Navegar a la [http://localhost:4502](http://localhost:4502).
1. Vaya a **Assets** > **Archivos** > **WKND compartido** > **Inglés** > **Aventuras** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![Carpeta Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Haga clic en **Bali Surf Camp** Fragmento de contenido para abrir el editor de fragmentos de contenido.
1. Modifique la **Título** y el **Descripción** de la aventura.

   ![Modificar fragmento de contenido](assets/setup/modify-content-fragment-bali.png)

1. Clic **Guardar** para guardar los cambios.
1. Actualice la aplicación React en [http://localhost:3000](http://localhost:3000) para ver los cambios:

   ![Actualizado Bali Surf Camp Adventure](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. Explorar GraphiQL {#graphiql}

1. Abrir [GraphiQL](http://localhost:4502/aem/graphiql.html) navegando a **Herramientas** > **General** > **Editor de consultas de GraphQL**
1. Seleccione las consultas persistentes existentes a la izquierda y ejecútelas para ver los resultados.

   >[!NOTE]
   >
   > La herramienta GraphiQL y la API de GraphQL son [se ha explorado con más detalle más adelante en el tutorial](../multi-step/explore-graphql-api.md).

## Enhorabuena.{#congratulations}

AEM Felicidades, ahora tiene una aplicación externa que consume contenido de la con GraphQL. No dude en inspeccionar el código en la aplicación React y seguir experimentando con la modificación de fragmentos de contenido existentes.

### Pasos siguientes

* [AEM Iniciar el tutorial sin encabezado de la](../multi-step/overview.md)
