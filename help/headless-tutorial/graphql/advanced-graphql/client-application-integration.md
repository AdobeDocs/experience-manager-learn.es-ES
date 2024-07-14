---
title: 'AEM Integración de aplicaciones de cliente: conceptos avanzados de la tecnología sin encabezado de la: GraphQL'
description: Implemente consultas persistentes e integre la aplicación WKND.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 241
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 1%

---

# Integración de aplicaciones cliente

En el capítulo anterior, creó y actualizó consultas persistentes mediante el Explorador de GraphiQL.

En este capítulo se explican los pasos para integrar las consultas persistentes con la aplicación cliente WKND (también conocida como aplicación WKND) mediante solicitudes de GET HTTP en los **componentes de React** existentes. AEM También ofrece un desafío opcional para aplicar sus aprendizajes sin encabezado de la, experiencia en codificación para mejorar la aplicación del cliente WKND.

## Requisitos previos {#prerequisites}

Este documento forma parte de un tutorial de varias partes. Asegúrese de que los capítulos anteriores se hayan completado antes de continuar con este capítulo. AEM AEM La aplicación cliente WKND se conecta a un servicio de publicación, por lo que es importante que **haya publicado lo siguiente en el servicio de publicación de la**.

* Configuraciones de proyecto
* Extremos de GraphQL
* Modelos de fragmento de contenido
* Fragmentos de contenido creados
* Consultas persistentes de GraphQL

Las _capturas de pantalla IDE de este capítulo provienen de [Visual Studio Code](https://code.visualstudio.com/)_

### Capítulo 1-4 Paquete de soluciones (opcional) {#solution-package}

AEM Hay disponible un paquete de soluciones para instalar que completa los pasos de la interfaz de usuario de la interfaz de usuario de la aplicación para los capítulos 1 a 4. Este paquete **no es necesario** si se han completado los capítulos anteriores.

1. Descargar [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. AEM En la barra de herramientas, vaya a **Herramientas** > **Implementación** > **Paquetes** para acceder al **Administrador de paquetes**.
1. Cargue e instale el paquete (archivo zip) descargado en el paso anterior.
1. AEM Replicar el paquete en el servicio de Publish de la

## Objetivos {#objectives}

En este tutorial, aprenderá a integrar las solicitudes de consultas persistentes en la aplicación WKND GraphQL AEM React de ejemplo mediante [Cliente sin encabezado para JavaScript](https://github.com/adobe/aem-headless-client-js).

## Clonar y ejecutar la aplicación cliente de ejemplo {#clone-client-app}

Para acelerar el tutorial, se proporciona una aplicación React JS de inicio.

1. Clone el repositorio [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. AEM Edite el archivo `aem-guides-wknd-graphql/advanced-tutorial/.env.development` y configure `REACT_APP_HOST_URI` para que apunte a su destino a la hora de publicar el servicio de publicación de la.

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

   ![Entorno de desarrollo de aplicaciones React](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > AEM Las instrucciones anteriores son para conectar la aplicación React al **servicio de Publish AEM**; sin embargo, para conectarse al **servicio de autor de**, obtenga un token de desarrollo local para su entorno de AEM as a Cloud Service de destino.
   >
   > También es posible conectar la aplicación a una [instancia de autor local mediante el SDK](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) de AEMaaCS con autenticación básica.


1. Abra un terminal y ejecute los comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Se debe cargar una nueva ventana del explorador en [http://localhost:3000](http://localhost:3000)


1. Pulse **Camping** > **Mochilero Yosemite** para ver los detalles de la aventura de mochilero Yosemite.

   ![Pantalla de mochilero Yosemite](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Abra las herramientas para desarrolladores del explorador e inspeccione la solicitud `XHR`

   ![GraphQL POST](assets/client-application-integration/graphql-persisted-query.png)

   Debería ver `GET` solicitudes al extremo de GraphQL con el nombre de configuración de proyecto (`wknd-shared`), el nombre de consulta persistente (`adventure-by-slug`), el nombre de variable (`slug`), el valor (`yosemite-backpacking`) y las codificaciones de caracteres especiales.

>[!IMPORTANT]
>
>    Si se está preguntando por qué se realiza la solicitud de la API de GraphQL AEM en el dominio `http://localhost:3000` y NO en el dominio de servicio de Publish, revise [En el capó](../multi-step/graphql-and-react-app.md#under-the-hood) del tutorial básico.


## Revisar el código

AEM En el [Tutorial básico - Crear una aplicación de React que usa las API de GraphQL que se utilizan en la aplicación](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object), hemos revisado y mejorado algunos archivos clave para obtener experiencia práctica. Antes de mejorar la aplicación WKND, revise los archivos clave.

* [Revisar el objeto sin encabezado AEMH](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* AEM [Implemente para ejecutar consultas persistentes de GraphQL de la versión de ejecución de datos](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Revisar componente de React `Adventures`

La vista principal de la aplicación WKND React es la lista de todas las aventuras que puedes filtrar según el tipo de actividad: _Acampar, Ciclismo_. El componente `Adventures` representa esta vista. A continuación se muestran los principales detalles de implementación:

* El vínculo `src/components/Adventures.js` llama al vínculo `useAllAdventures(adventureActivity)` y aquí el argumento `adventureActivity` es el tipo de actividad.

* El vínculo `useAllAdventures(adventureActivity)` está definido en el archivo `src/api/usePersistedQueries.js`. En función del valor `adventureActivity`, determina a qué consulta persistente llamar. Si no es un valor nulo, llama a `wknd-shared/adventures-by-activity`; de lo contrario, obtiene todas las aventuras disponibles `wknd-shared/adventures-all`.

* El vínculo utiliza la función principal `fetchPersistedQuery(..)` que delega la ejecución de la consulta a `AEMHeadless` mediante `aemHeadlessClient.js`.

* AEM El vínculo también devuelve solamente los datos relevantes de la respuesta de GraphQL de la en `response.data?.adventureList?.items`, lo que permite que los componentes de la vista React `Adventures` no sean agnósticos de las estructuras JSON principales.

* Una vez ejecutada correctamente la consulta, la función de procesamiento `AdventureListItem(..)` de `Adventures.js` agrega un elemento HTML para mostrar la información de _Imagen, duración del viaje, precio y título_.

### Revisar componente de React `AdventureDetail`

El componente React `AdventureDetail` procesa los detalles de la aventura. A continuación se muestran los principales detalles de implementación:

* El vínculo `src/components/AdventureDetail.js` llama al vínculo `useAdventureBySlug(slug)` y el argumento `slug` es un parámetro de consulta.

* Como en el caso anterior, el vínculo `useAdventureBySlug(slug)` se define en el archivo `src/api/usePersistedQueries.js`. Llama a `wknd-shared/adventure-by-slug` a una consulta persistente delegando a `AEMHeadless` a través de `aemHeadlessClient.js`.

* Una vez ejecutada correctamente la consulta, la función de procesamiento `AdventureDetailRender(..)` de `AdventureDetail.js` agrega un elemento HTML para mostrar los detalles de la aventura.


## Mejore el código

### Usar consulta persistente `adventure-details-by-slug`

En el capítulo anterior, creamos la consulta persistente `adventure-details-by-slug`, que proporciona información de aventura adicional como _ubicación, instructorTeam y administrator_. Vamos a reemplazar `adventure-by-slug` con `adventure-details-by-slug` consulta persistente para procesar esta información adicional.

1. Abra `src/api/usePersistedQueries.js`.

1. Busque la función `useAdventureBySlug()` y actualice la consulta como

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

1. Para mostrar información adicional sobre la aventura, abra `src/components/AdventureDetail.js`

1. Busque la función `AdventureDetailRender(..)` y actualice la función de retorno como

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

1. Defina también las funciones de procesamiento correspondientes:

   **InformaciónDeUbicación**

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

   **Ubicación**

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

   **EquipoInstructor**

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

### Definir estilos nuevos

1. Abra `src/components/AdventureDetail.scss` y agregue las siguientes definiciones de clase

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
>Los archivos actualizados están disponibles en el proyecto **AEM Guides WKND - GraphQL**; consulte la sección [Tutorial avanzado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial).


Después de completar las mejoras anteriores, la aplicación WKND tiene el siguiente aspecto y las herramientas para desarrolladores del explorador muestran `adventure-details-by-slug` llamada de consulta persistente.

![APLICACIÓN WKND mejorada](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Reto de mejora (opcional)

La vista principal de la aplicación WKND React te permite filtrar estas aventuras según el tipo de actividad como _Camping, Ciclismo_. Sin embargo, el equipo empresarial de WKND desea tener una capacidad de filtrado adicional basada en _Location_. Los requisitos son

* En la vista principal de la aplicación WKND, en la esquina superior izquierda o derecha, agregue el icono de filtrado _Location_.
* Si hace clic en el icono de filtrado _Ubicación_, se mostrará una lista de ubicaciones.
* Al hacer clic en una opción de ubicación de la lista, solo se deben mostrar las aventuras coincidentes.
* Si solo hay una aventura que coincida, se muestra la vista Detalles de la aventura.

## Felicitaciones

Enhorabuena. Ahora ha completado la integración y la implementación de las consultas persistentes en la aplicación WKND de ejemplo.
