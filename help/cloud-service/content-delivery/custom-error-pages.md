---
title: Páginas de error personalizadas
description: Obtenga información sobre cómo implementar páginas de error personalizadas para el sitio web alojado en AEM as a Cloud Service.
version: Cloud Service
feature: Brand Experiences, Configuring, Developing
topic: Content Management, Development
role: Developer
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-24T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
source-git-commit: d11b07441d8c46ce9a352e4c623ddc1781b9b9be
workflow-type: tm+mt
source-wordcount: '1355'
ht-degree: 0%

---


# Páginas de error personalizadas

Obtenga información sobre cómo implementar páginas de error personalizadas para el sitio web alojado en AEM as a Cloud Service.

En este tutorial, aprenderá lo siguiente:

- Páginas de error predeterminadas
- Páginas de error servidas desde
   - AEM Tipo de servicio de: creación, publicación, previsualización
   - CDN administrado por Adobe
- Opciones para personalizar páginas de error
   - Directiva Apache ErrorDocument
   - AEM ACS Commons - Controlador de página de error
   - Páginas de error de CDN

## Páginas de error predeterminadas

Revisemos cuándo se muestran las páginas de error, las páginas de error predeterminadas y desde dónde se proporcionan.

Las páginas de error se muestran cuando:

- la página no existe (404)
- no está autorizado para acceder a una página (403)
- error del servidor (500) debido a problemas de código o porque el servidor no está disponible.

AEM as a Cloud Service proporciona _páginas de error predeterminadas_ para los escenarios anteriores. Es una página genérica y no coincide con su marca.

AEM La página de error predeterminada _se proporciona_ desde el _tipo de servicio_(autor, publicación, vista previa) o desde la _CDN administrada por Adobe_. Consulte la tabla siguiente para obtener más detalles.

| Página de error servida desde | Detalles |
|---------------------|:-----------------------:|
| AEM Tipo de servicio de: creación, publicación, previsualización | AEM AEM Cuando la solicitud de página la proporciona el tipo de servicio de la, la página de error se proporciona desde el tipo de servicio de la. |
| CDN administrado por Adobe | Cuando la CDN administrada por el Adobe AEM _no puede alcanzar el tipo de servicio de_ (servidor de origen), la página de error se proporciona desde la CDN administrada por el Adobe. **Es un evento improbable, pero vale la pena mencionarlo.** |

AEM Sin embargo, puede _personalizar tanto el tipo de servicio como las páginas de error de CDN administradas por Adobe_ para que coincidan con su marca y proporcionen una mejor experiencia de usuario.

## Opciones para personalizar páginas de error

Las siguientes opciones están disponibles para personalizar las páginas de error:

| Aplicable a | Nombre de opción | Descripción |
|---------------------|:-----------------------:|:-----------------------:|
| AEM Tipos de servicio de publicación y previsualización de | Directiva ErrorDocument | Utilice la directiva [ErrorDocument](https://httpd.apache.org/docs/2.4/custom-error.html) en el archivo de configuración de Apache para especificar la ruta a la página de error personalizada. AEM Solo se aplica a los tipos de servicio de publicación: publicación y previsualización. |
| AEM Tipos de servicio de: creación, publicación y previsualización | AEM Controlador de página de error de ACS Commons | AEM AEM Use el [Controlador de página de error de ACS Commons ](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html) para personalizar el error en todos los tipos de servicio de la. |
| CDN administrado por Adobe | Páginas de error de CDN | Utilice las páginas de error de CDN para personalizar las páginas de error cuando la CDN gestionada por Adobe AEM no pueda alcanzar el tipo de servicio (servidor de origen) de la red de distribución de contenido (CDN) de la red de distribución de contenido (CDN)). |


## Requisitos previos

AEM En este tutorial, aprenderá a personalizar las páginas de error utilizando la directiva _ErrorDocument_, el _Controlador de páginas de error de ACS Commons_ y las opciones _Páginas de error de CDN_. Para seguir este tutorial, necesita lo siguiente:

- AEM El [entorno de desarrollo local](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview) o el entorno de AEM as a Cloud Service. La opción _Páginas de error de CDN_ se aplica al entorno de AEM as a Cloud Service.

- AEM El [proyecto WKND ](https://github.com/adobe/aem-guides-wknd) de para personalizar las páginas de error.

## Configuración

- AEM AEM Clone e implemente el proyecto WKND de local siguiendo los pasos a continuación:

  ```
  # For local AEM development environment
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallSinglePackage -PautoInstallSinglePackagePublish
  ```

- Para el entorno de AEM as a Cloud Service AEM, implemente el proyecto WKND de ejecutando la [canalización de pila completa](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline), vea el ejemplo de la [canalización que no es de producción](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/cloud-manager/cicd-non-production-pipeline).

- Compruebe que las páginas del sitio WKND se representan correctamente.

## Directiva Apache ErrorDocument para personalizar páginas de error{#errordocument-directive}

AEM Revisemos cómo el proyecto [WKND](https://github.com/adobe/aem-guides-wknd) utiliza la directiva `ErrorDocument` de Apache para mostrar las páginas de error personalizadas.

- El módulo `ui.content.sample` contiene las [páginas de error](https://github.com/adobe/aem-guides-wknd/tree/main/ui.content.sample/src/main/content/jcr_root/content/wknd/language-masters/en/errors) de marca en `/content/wknd/language-masters/en/errors`. AEM Revíselos en su entorno [local de ](http://localhost:4502/sites.html/content/wknd/language-masters/en/errors) o de `https://author-p<ID>-e<ID>.adobeaemcloud.com/ui#/aem/sites.html/content/wknd/language-masters/en/errors` de AEM as a Cloud Service.

- El archivo `wknd.vhost` del módulo `dispatcher` contiene:
   - La directiva [ErrorDocument](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L139-L143) que señala a las [páginas de error](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/variables/custom.vars#L7-L8) anteriores.
   - El valor [DispatcherPassError](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L133) está establecido en 1, por lo que Dispatcher permite que Apache gestione todos los errores.

  ```
  ...
  # ErrorDocument directive in wknd.vhost file
  ErrorDocument 404 ${404_PAGE}
  ErrorDocument 500 ${500_PAGE}
  ErrorDocument 502 ${500_PAGE}
  ErrorDocument 503 ${500_PAGE}
  ErrorDocument 504 ${500_PAGE}
  
  ...
  # DispatcherPassError value in wknd.vhost file
  <IfModule disp_apache2.c>
      ...
      DispatcherPassError        1
  </IfModule>
  
  # Custom error pages path in custom.vars file
  Define 404_PAGE /content/wknd/us/en/errors/404.html
  Define 500_PAGE /content/wknd/us/en/errors/500.html
  ...
  ```

- Revise las páginas de error personalizadas del sitio WKND introduciendo un nombre de página o una ruta incorrectos en su entorno, por ejemplo [https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html](https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html).

## AEM Controlador de página de error de ACS Commons-error para personalizar páginas de error{#acs-aem-commons-error-page-handler}

AEM Para personalizar páginas de error mediante el controlador de páginas de error de ACS Commons, revise la sección [Cómo usar](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html#how-to-use).

## Páginas de error de CDN para personalizar las páginas de error{#cdn-error-pages}

Vamos a implementar páginas de error de CDN para personalizar las páginas de error cuando la CDN administrada por Adobe AEM no puede alcanzar el tipo de servicio (servidor de origen).

>[!IMPORTANT]
>
> Tenga en cuenta que la CDN administrada por el Adobe AEM no puede alcanzar el tipo de servicio (servidor de origen) de es un evento improbable, pero vale la pena planificarlo.


### Resumen de páginas de error de CDN

SPA La página de error de CDN la implementa como aplicación de una sola página () CDN administrada por el Adobe.

El contenido de marca específico de WKND debe generarse dinámicamente mediante el archivo JavaScript. El archivo JavaScript debe alojarse en una ubicación de acceso público. Por lo tanto, los siguientes archivos estáticos deben desarrollarse y alojarse en una ubicación de acceso público:

1. **jsUrl**: URL absoluta del archivo JavaScript para procesar el contenido de la página de error creando elementos de HTML de forma dinámica.
1. **cssUrl**: La dirección URL absoluta del archivo CSS para aplicar estilo al contenido de la página de error.
1. **icoUrl**: La dirección URL absoluta del favicon.

### Desarrollar una página de error personalizada

SPA Desarrollemos el contenido de página de error de marca específico de WKND como una aplicación de una sola página ().

Para fines de demostración, usemos [React](https://react.dev/); sin embargo, puede usar cualquier módulo o biblioteca de JavaScript.

- Cree un nuevo proyecto de React ejecutando el siguiente comando:

  ```
  $ npx create-react-app aem-cdn-error-page
  ```

- Abra el proyecto en su editor de código favorito y actualice los siguientes archivos:

   - `src/App.js`: es el componente principal que procesa el contenido de la página de error.

     ```javascript
     import logo from "./wknd-logo.png";
     import "./App.css";
     
     function App() {
       return (
         <>
           <div className="App">
             <div className="container">
             <img src={logo} className="App-logo" alt="logo" />
             </div>
           </div>
           <div className="container">
             <div className="error-code">CDN Error Page</div>
             <h1 className="error-message">Ruh-Roh! Page Not Found</h1>
             <p className="error-description">
               We're sorry, we are unable to fetch this page!
             </p>
           </div>
         </>
       );
     }
     
     export default App;
     ```

   - `src/App.css`: aplicar estilo al contenido de la página de error.

     ```css
     .App {
       text-align: left;
     }
     
     .App-logo {
       height: 14vmin;
       pointer-events: none;
     }
     
     
     body {
       margin-top: 0;
       padding: 0;
       font-family: Arial, sans-serif;
       background-color: #fff;
       color: #333;
       display: flex;
       justify-content: center;
       align-items: center;
     }
     
     .container {
       text-align: letf;
       padding-top: 10px;
     }
     
     .error-code {
       font-size: 4rem;
       font-weight: bold;
       color: #ff6b6b;
     }
     
     .error-message {
       font-size: 2.5rem;
       margin-bottom: 10px;
     }
     
     .error-description {
       font-size: 1rem;
       margin-bottom: 20px;
     }
     ```

   - Agregar el archivo `wknd-logo.png` a la carpeta `src`. Copie el [archivo](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-512.png) como `wknd-logo.png`.

   - Agregar el archivo `favicon.ico` a la carpeta `public`. Copie el [archivo](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-32.png) como `favicon.ico`.

   - Compruebe el contenido de la página de error de CDN con marca WKND ejecutando el proyecto:

     ```
     $ npm start
     ```

     Abra el explorador y vaya a `http://localhost:3000/` para ver el contenido de la página de error de CDN.

   - Cree el proyecto para generar los archivos estáticos:

     ```
     $ npm run build
     ```

     Los archivos estáticos se generan en la carpeta `build`.


También puede descargar el archivo [aem-cdn-error-page.zip](./assets/aem-cdn-error-page.zip) que contiene los archivos de proyecto de React anteriores.

A continuación, aloje los archivos estáticos anteriores en una ubicación de acceso público.

### Archivos estáticos de host necesarios para la página de error de CDN

Vamos a alojar los archivos estáticos en Azure Blob Storage. Sin embargo, puedes usar cualquier servicio de alojamiento de archivos estáticos como [Netlify](https://www.netlify.com/), [Vercel](https://vercel.com/) o [AWS S3](https://aws.amazon.com/s3/).

- Siga la documentación oficial de [Azure Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) para crear un contenedor y cargar los archivos estáticos.

  >[!IMPORTANT]
  >
  >Si utiliza otros servicios de alojamiento de archivos estáticos, siga su documentación para alojar los archivos estáticos.

- Asegúrese de que los archivos estáticos sean de acceso público. Mi configuración de cuenta de almacenamiento específica de demostración de WKND es la siguiente:

   - **Nombre de cuenta de almacenamiento**: `aemcdnerrorpageresources`
   - **Nombre de contenedor**: `static-files`

  ![Almacenamiento de blob de Azure](./assets/azure-blob-storage-settings.png)

- En el contenedor superior `static-files`, se cargan los siguientes archivos de la carpeta `build`:

   - `error.js`: se cambió el nombre del archivo `build/static/js/main.<hash>.js` a `error.js` y a [accesible públicamente](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js).
   - `error.css`: se cambió el nombre del archivo `build/static/css/main.<hash>.css` a `error.css` y a [accesible públicamente](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css).
   - `favicon.ico`: el archivo `build/favicon.ico` se ha cargado tal cual y [es de acceso público](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico).

A continuación, configure la regla CDN (errorPages) y haga referencia a los archivos estáticos anteriores.

### Configuración de la regla de CDN

Vamos a configurar la regla de CDN `errorPages` que utiliza los archivos estáticos anteriores para procesar el contenido de la página de error de CDN.

1. AEM Abra el archivo `cdn.yaml` desde la carpeta principal `config` de su proyecto de la. Por ejemplo, el archivo cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) del proyecto [WKND.

1. Agregue la siguiente regla de CDN al archivo `cdn.yaml`:

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     # The CDN Error Page configuration. 
     # The error page is displayed when the Adobe-managed CDN is unable to reach the origin server.
     # It is implemented as a Single Page Application (SPA) and WKND branded content must be generated dynamically using the JavaScript file 
     errorPages:
       spa:
         title: Adobe AEM CDN Error Page # The title of the error page
         icoUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico # The PUBLIC URL of the favicon
         cssUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css # The PUBLIC URL of the CSS file
         jsUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js # The PUBLIC URL of the JavaScript file
   ```

1. Guarde, confirme e inserte los cambios en el repositorio de flujo ascendente de Adobe.

### Implementación de la regla CDN

Finalmente, implemente la regla de CDN configurada en el entorno de AEM as a Cloud Service mediante la canalización de Cloud Manager.

1. En Cloud Manager, vaya a la sección **Canalizaciones**.

1. Cree una nueva canalización o seleccione la canalización existente que implemente solamente los archivos **Config**. Para ver los pasos detallados, consulte [Crear una canalización de configuración](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

1. Haga clic en el botón **Ejecutar** para implementar la regla de CDN.

![Implementar regla de CDN](./assets/deploy-cdn-rule-for-cdn-error-pages.png)

### Prueba de las páginas de error de CDN

Para probar las páginas de error de CDN, siga los siguientes pasos:

- Abra el explorador y vaya a la URL del entorno de Publish; añada `cdnstatus?code=404` a la URL, por ejemplo, [https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404](https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404), o acceda a través de la [URL de dominio personalizado](https://wknd.enablementadobe.com/cdnstatus?code=404)

  ![WKND - Página de error de CDN](./assets/wknd-cdn-error-page.png)

- Los códigos admitidos son: 403, 404, 406, 500 y 503.

- Compruebe la pestaña de red del explorador para ver si los archivos estáticos se cargan desde el almacenamiento del blob de Azure. El documento del HTML entregado por la CDN administrada por Adobe contiene el contenido mínimo y el archivo JavaScript crea dinámicamente el contenido de la página de error con marca.

  ![Ficha Red de página de error de CDN](./assets/wknd-cdn-error-page-network-tab.png)

## Resumen

En este tutorial, ha aprendido a implementar páginas de error personalizadas para el sitio web alojado en AEM as a Cloud Service.

También ha aprendido los pasos detallados para la opción de páginas de error de CDN para personalizar las páginas de error cuando la CDN administrada por Adobe AEM no puede alcanzar el tipo de servicio de origen (servidor de origen) de la.



