---
title: Configuración de desarrollo local
description: Aprenda a configurar un entorno de desarrollo local para el editor universal con el fin de editar el contenido de una aplicación React de ejemplo.
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 189
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 47bef697-5253-493a-b9f9-b26c27d2db56
source-git-commit: 7c58c5cb6a3d99a9577206b3e5e0b8dcd55a850e
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 4%

---

# Configuración de desarrollo local

Aprenda a configurar un entorno de desarrollo local para editar el contenido de una aplicación de React con el Editor universal de AEM.

## Requisitos previos

Se requiere lo siguiente para seguir este tutorial:

- Aptitudes básicas de HTML y JavaScript.
- Las siguientes herramientas deben estar instaladas localmente:
   - [Nodo.js](https://nodejs.org/es/download/)
   - [Git](https://git-scm.com/downloads)
   - Un IDE o editor de código, como [Visual Studio Code](https://code.visualstudio.com/)
- Descargue e instale lo siguiente:
   - [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk): contiene el Jar de inicio rápido utilizado para ejecutar AEM Author y Publish localmente con fines de desarrollo.
   - [Servicio de editor universal](https://experienceleague.adobe.com/en/docs/experience-cloud/software-distribution/home): una copia local del servicio de editor universal, tiene un subconjunto de características y se puede descargar desde el portal de distribución de software.
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy): Un proxy HTTP SSL local simple que usa un certificado autofirmado para el desarrollo local. El editor universal de AEM requiere la URL HTTPS de la aplicación React para cargarla en el editor.

## Configuración local

Siga los pasos a continuación para configurar el entorno de desarrollo local:

### SDK de AEM

Para proporcionar el contenido para la aplicación WKND Teams React, instale los siguientes paquetes en el SDK local de AEM.

- [Equipos WKND - Paquete de contenido](./assets/basic-tutorial-solution.content.zip): Contiene los modelos de fragmentos de contenido, fragmentos de contenido y consultas de GraphQL persistentes.
- [Equipos WKND - Paquete de configuración](./assets/basic-tutorial-solution.ui.config.zip): Contiene las configuraciones de Intercambio de recursos de origen cruzado (CORS) y Controlador de autenticación de token. El CORS facilita las propiedades web que no son de AEM para realizar llamadas del lado del cliente basadas en explorador a las API de GraphQL de AEM y el controlador de autenticación de token se utiliza para autenticar cada solicitud a AEM.

  ![Equipos WKND - Paquetes](./assets/wknd-teams-packages.png)

### Aplicación React

Para configurar la aplicación WKND Teams React, siga los pasos a continuación:

1. Clona la aplicación [WKND Teams React](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) de la rama de solución `basic-tutorial`.

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Vaya al directorio `basic-tutorial` y ábralo en el editor de código.

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. Instale las dependencias e inicie la aplicación React.

   ```bash
   $ npm install
   $ npm start
   ```

1. Abra la aplicación WKND Teams React en el explorador en [http://localhost:3000](http://localhost:3000). Muestra una lista de los integrantes del equipo y sus detalles. El contenido de la aplicación React lo proporciona AEM SDK local mediante las API de GraphQL (`/graphql/execute.json/my-project/all-teams`), que puede comprobar mediante la ficha de red del explorador.

   ![Equipos WKND - Aplicación React](./assets/wknd-teams-react-app.png)

### Servicio de editor universal

Para configurar el servicio de editor universal **local**, siga los pasos a continuación:

1. Descargue la versión más reciente del servicio de editor universal del [Portal de distribución de software](https://experience.adobe.com/downloads).

   ![Distribución de software - Servicio de editor universal](./assets/universal-editor-service.png)

1. Extraiga el archivo zip descargado y copie el archivo `universal-editor-service.cjs` en un nuevo directorio llamado `universal-editor-service`.

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. Cree el archivo `.env` en el directorio `universal-editor-service` y agregue las siguientes variables de entorno:

   ```bash
   # The port on which the Universal Editor service runs
   UES_PORT=8000
   # Disable SSL verification
   UES_TLS_REJECT_UNAUTHORIZED=false
   ```

1. Inicie el servicio local de Universal Editor.

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

El comando anterior inicia el servicio Editor universal en el puerto `8000` y debería ver el siguiente resultado:

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### Proxy HTTP SSL local

El editor universal de AEM requiere que la aplicación React se suministre a través de HTTPS. Vamos a configurar un proxy HTTP SSL local que utilice un certificado firmado automáticamente para el desarrollo local.

Siga los pasos a continuación para configurar el proxy HTTP SSL local y servir el servicio SDK de AEM y el editor universal a través de HTTPS:

1. Instale el paquete `local-ssl-proxy` globalmente.

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. Inicie dos instancias del proxy HTTP SSL local para los siguientes servicios:

   - Proxy HTTP SSL local de AEM SDK en el puerto `8443`.
   - Proxy HTTP SSL local del servicio Editor universal en el puerto `8001`.

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### Actualizar la aplicación React para que utilice HTTPS

Para habilitar HTTPS para la aplicación WKND Teams React, siga los pasos a continuación:

1. Detenga el React presionando `Ctrl + C` en el terminal.
1. Actualice el archivo `package.json` para incluir la variable de entorno `HTTPS=true` en el script `start`.

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. Actualice `REACT_APP_HOST_URI` en el archivo `.env.development` para utilizar el protocolo HTTPS y el puerto proxy HTTP SSL local de AEM SDK.

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. Actualice el archivo `../src/proxy/setupProxy.auth.basic.js` para usar la configuración SSL relajada mediante la opción `secure: false`.

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. Inicie la aplicación React.

   ```bash
   $ npm start
   ```

## Verificar la configuración

Después de configurar el entorno de desarrollo local siguiendo los pasos anteriores, vamos a verificar la configuración.

### Verificación local

Asegúrese de que los siguientes servicios se ejecutan localmente en HTTPS; es posible que tenga que aceptar la advertencia de seguridad en el explorador para el certificado autofirmado:

1. Aplicación WKND Teams React en [https://localhost:3000](https://localhost:3000)
1. AEM SDK en [https://localhost:8443](https://localhost:8443)
1. Servicio de editor universal en [https://localhost:8001](https://localhost:8001)

### Carga de la aplicación WKND Teams React en el editor universal

Carguemos la aplicación WKND Teams React en el editor universal para verificar la configuración:

1. Abra Universal Editor https://experience.adobe.com/#/aem/editor en su explorador. Si se le solicita, inicie sesión con su Adobe ID.

1. Introduzca la URL de la aplicación WKND Teams React en el campo de entrada URL del sitio del editor universal y haga clic en `Open`.

   ![Editor universal - URL del sitio](./assets/universal-editor-site-url.png)

1. La aplicación WKND Teams React se carga en el editor universal **pero aún no puede editar el contenido**. Debe instrumentar la aplicación React para habilitar la edición de contenido con el editor universal.

   ![Editor universal: aplicación WKND Teams React](./assets/universal-editor-wknd-teams.png)


## Siguiente paso

Aprenda a [instrumentar la aplicación React para editar el contenido](./instrument-to-edit-content.md).
