---
title: Integración de aplicaciones cliente - Conceptos avanzados de AEM sin encabezado - GraphQL
description: Implemente consultas persistentes e inclúyalas en la aplicación WKND.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 1%

---

# Integración de aplicaciones de cliente

En el capítulo anterior, se han creado y actualizado las consultas persistentes mediante el Explorador de GraphiQL.

Este capítulo le explica los pasos para integrar las consultas persistentes con la aplicación cliente WKND (también conocida como aplicación WKND) mediante solicitudes de GET HTTP dentro de las existentes **Reacción de componentes**. También ofrece un desafío opcional para aplicar sus conocimientos AEM sin encabezado, experiencia en codificación para mejorar la aplicación cliente WKND.

## Requisitos previos {#prerequisites}

Este documento forma parte de un tutorial en varias partes. Asegúrese de que los capítulos anteriores se hayan completado antes de continuar con este capítulo. La aplicación cliente WKND se conecta a AEM servicio de publicación, por lo que es importante que **ha publicado lo siguiente en el servicio de publicación de AEM**.

* Configuraciones del proyecto
* Extremos de GraphQL
* Modelos de fragmento de contenido
* Fragmentos de contenido creados
* Consultas persistentes de GraphQL

La variable _Las capturas de pantalla IDE de este capítulo provienen de [Código de Visual Studio](https://code.visualstudio.com/)_

### Capítulo 1-4 Paquete de soluciones (opcional) {#solution-package}

Hay disponible un paquete de soluciones para instalar que completa los pasos de la interfaz de usuario de AEM para los capítulos 1-4. Este paquete es **no se necesita** si se han completado los capítulos anteriores.

1. Descargar [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. En AEM, vaya a **Herramientas** > **Implementación** > **Paquetes** para acceder a **Administrador de paquetes**.
1. Cargue e instale el paquete (archivo zip) descargado en el paso anterior.
1. Duplique el paquete en el servicio AEM Publish

## Objetivos {#objectives}

En este tutorial, aprenderá a integrar las solicitudes de consultas persistentes en la aplicación WKND GraphQL React de muestra utilizando la [AEM cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js).

## Clone y ejecute la aplicación cliente de ejemplo {#clone-client-app}

Para acelerar el tutorial, se proporciona una aplicación React JS de inicio.

1. Clonar el [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite el `aem-guides-wknd-graphql/advanced-tutorial/.env.development` archivo y conjunto `REACT_APP_HOST_URI` para que apunte a su servicio de publicación AEM target.

   Actualice el método de autenticación si se conecta a una instancia de autor.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![Entorno de desarrollo de aplicaciones de React](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > Las instrucciones anteriores son para conectar la aplicación React al **Servicio de AEM Publish**, sin embargo, para conectarse al **Servicio Autor de AEM** obtenga un token de desarrollo local para el entorno as a Cloud Service de AEM de destino.
   >
   > También es posible conectar la aplicación a un [instancia de autor local mediante el SDK de AEMaaCS](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) mediante autenticación básica.


1. Abra un terminal y ejecute los comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Se debe cargar una nueva ventana del explorador en [http://localhost:3000](http://localhost:3000)


1. Toque **Camping** > **Mochila yosemite** para ver los detalles de la aventura de Yosemite Backpackaging.

   ![Pantalla Yosemite](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Abra las herramientas para desarrolladores del explorador e inspeccione las `XHR` solicitud

   ![POST GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Debería ver `GET` solicitudes al extremo de GraphQL con el nombre de configuración del proyecto (`wknd-shared`), nombre de consulta persistente (`adventure-by-slug`), nombre de variable (`slug`), valor (`yosemite-backpacking`) y codificaciones de caracteres especiales.

>[!IMPORTANT]
>
>    Si se pregunta por qué la solicitud de API de GraphQL se realiza con la variable `http://localhost:3000` y NO en el dominio del servicio de publicación de AEM, revise [Debajo Del Capó](../multi-step/graphql-and-react-app.md#under-the-hood) de Tutorial básico.


## Revisar el código

En el [Tutorial básico: Cree una aplicación React que utilice API de AEM GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) paso que hemos revisado y mejorado pocos archivos clave para obtener experiencia práctica. Antes de mejorar la aplicación WKND, revise los archivos clave.

* [Revisar el objeto AEMHeadless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [Implementar para ejecutar consultas persistentes AEM GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Consulte `Adventures` Componente React

La vista principal de la aplicación WKND React es la lista de todas las aventuras y puede filtrar estas aventuras en función del tipo de actividad _Camping, Ciclismo_. Esta vista se representa mediante la función `Adventures` componente. A continuación se muestran los detalles principales de la implementación:

* La variable `src/components/Adventures.js` llamadas `useAllAdventures(adventureActivity)` gancho y aquí `adventureActivity` es el tipo de actividad.

* La variable `useAllAdventures(adventureActivity)` el vínculo se define en la variable `src/api/usePersistedQueries.js` archivo. Basado en `adventureActivity` determina a qué consulta persistente llamar. Si no es un valor nulo, llama a `wknd-shared/adventures-by-activity`, si no obtiene todas las aventuras disponibles `wknd-shared/adventures-all`.

* El gancho utiliza el principal `fetchPersistedQuery(..)` que delega la ejecución de la consulta a `AEMHeadless` via `aemHeadlessClient.js`.

* El vínculo también solo devuelve los datos relevantes de la respuesta de AEM GraphQL en `response.data?.adventureList?.items`, permitiendo que `Adventures` Reaccione los componentes de vista para no tener en cuenta las estructuras JSON principales.

* Una vez ejecutada correctamente la consulta, la variable `AdventureListItem(..)` procesar función desde `Adventures.js` agrega un elemento de HTML para mostrar la variable _Imagen, duración del viaje, precio y título_ información.

### Consulte `AdventureDetail` Componente React

La variable `AdventureDetail` El componente React representa los detalles de la aventura. A continuación se muestran los detalles principales de la implementación:

* La variable `src/components/AdventureDetail.js` llamadas `useAdventureBySlug(slug)` gancho y aquí `slug` es parámetro de consulta.

* Como en el caso anterior, la variable `useAdventureBySlug(slug)` el vínculo se define en la variable `src/api/usePersistedQueries.js` archivo. Llama `wknd-shared/adventure-by-slug` consulta persistente delegando en `AEMHeadless` via `aemHeadlessClient.js`.

* Una vez ejecutada correctamente la consulta, la variable `AdventureDetailRender(..)` procesar función desde `AdventureDetail.js` agrega un elemento HTML para mostrar los detalles de Aventura.


## Mejorar el código

### Uso `adventure-details-by-slug` consulta persistente

En el capítulo anterior, creamos la variable `adventure-details-by-slug` consulta persistente, proporciona información adicional de aventura como _ubicación, instructorTeam y administrador_. Vamos a reemplazar `adventure-by-slug` con `adventure-details-by-slug` consulta persistente para procesar esta información adicional.

1. Abra `src/api/usePersistedQueries.js`.

1. Localizar la función `useAdventureBySlug()` y actualizar consulta como

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### Mostrar información adicional

1. Para mostrar información adicional sobre aventuras, abra `src/components/AdventureDetail.js`

1. Localizar la función `AdventureDetailRender(..)` y actualizar la función de retorno como

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. Defina también las funciones de renderización correspondientes:

   **LocationInfo**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **Lugar de residencia**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **InstructorTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **Administrador**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### Definir nuevos estilos

1. Apertura `src/components/AdventureDetail.scss` y agregar las siguientes definiciones de clase

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>Los archivos actualizados están disponibles en **AEM Guías WKND - GraphQL** proyecto, consulte [Tutorial avanzado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) para obtener más información.


Después de completar las mejoras anteriores, la aplicación WKND se verá a continuación y se mostrarán las herramientas para desarrolladores del navegador `adventure-details-by-slug` llamada de consulta persistente.

![APLICACIÓN WKND mejorada](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Desafío para mejorar (opcional)

La vista principal de la aplicación WKND React permite filtrar estas aventuras en función del tipo de actividad como _Camping, Ciclismo_. Sin embargo, el equipo empresarial de WKND quiere tener un _Ubicación_ capacidad de filtrado basada en . Los requisitos son

* En la vista principal de la aplicación WKND, en la esquina superior izquierda o derecha, agregue _Ubicación_ icono de filtrado.
* Hacer clic _Ubicación_ el icono de filtrado debe mostrar la lista de ubicaciones.
* Al hacer clic en una opción de ubicación deseada en la lista, solo se deben mostrar las aventuras coincidentes.
* Si solo hay una aventura que coincida, se muestra la vista Detalles de aventura .

## Felicitaciones

¡Enhorabuena! Ya ha completado la integración y la implementación de las consultas persistentes en la aplicación WKND de muestra.
