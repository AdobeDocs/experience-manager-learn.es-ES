---
title: Token de acceso de desarrollo local
description: AEM AEM Los tokens de acceso de desarrollo local se utilizan para acelerar el desarrollo de integraciones con as a Cloud Service que interactúan mediante programación con los servicios de AEM Author o Publish a través de HTTP.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# Token de acceso de desarrollo local

AEM Los desarrolladores que crean integraciones que requieren acceso programático a los recursos as a Cloud Service AEM necesitan una forma sencilla y rápida de obtener tokens de acceso temporales para facilitar las actividades de desarrollo local de los recursos de la red de distribución de recursos (). AEM AEM Para satisfacer esta necesidad, la consola de desarrollador de permite a los desarrolladores generar tokens de acceso temporales que se pueden utilizar para acceder a los recursos mediante programación.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## Generar un token de acceso de desarrollo local

![Obtención de un token de acceso de desarrollo local](assets/local-development-access-token/getting-a-local-development-access-token.png)

El token de acceso de desarrollo local proporciona acceso a los servicios de AEM Author y Publish como usuario que generó el token, junto con sus permisos. A pesar de ser un token de desarrollo, no lo comparta ni lo almacene en el control de código fuente.

1. Entrada [Adobe Admin Console](https://adminconsole.adobe.com/) asegúrese de que usted, el desarrollador, sea miembro de:
   + __Cloud Manager: Desarrollador__ AEM Perfil de producto de IMS (otorga acceso a la consola de desarrollador de)
   + O bien __AEM Administradores de__ o __AEM Usuarios de__ AEM Perfil de producto de IMS para el servicio del entorno con el que se integra el token de acceso
   + AEM El entorno as a Cloud Service de zona protegida solo requiere ser miembro de la __AEM Administradores de__ o __AEM Usuarios de__ Perfil del producto
1. Iniciar sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. AEM Abra el Programa que contiene el entorno as a Cloud Service de la con el que desea integrarse
1. Pulse el botón __elipsis__ junto al entorno en el __Entornos__ y seleccione. __Developer Console__
1. Pulse en __Integraciones__ pestaña
1. Pulse el botón __Token local__ pestaña
1. Tocar __Obtener token de desarrollo local__ botón
1. Pulse en __botón descargar__ en la esquina superior izquierda para descargar el archivo JSON que contiene `accessToken` y guarde el archivo JSON en una ubicación segura de su equipo de desarrollo.
   + AEM Este es su token de acceso de desarrollador de 24 horas al entorno as a Cloud Service de la.

![AEM Consola de desarrollador de: integraciones: obtener token de desarrollo local](./assets/local-development-access-token/developer-console.png)

## Se utilizó el token de acceso de desarrollo local{#use-local-development-access-token}

![Token de acceso de desarrollo local: aplicación externa](assets/local-development-access-token/local-development-access-token-external-application.png)

1. AEM Descargue el token de acceso de desarrollo local temporal desde la consola de desarrollador de
   + El token de acceso de desarrollo local caduca cada 24 horas, por lo que los desarrolladores deben descargar nuevos tokens de acceso diariamente
1. AEM Se está desarrollando una aplicación externa que interactúa mediante programación con la aplicación as a Cloud Service de la
1. La aplicación externa lee el token de acceso de desarrollo local
1. AEM La aplicación externa construye solicitudes HTTP para que se realicen en as a Cloud Service, agregando el token de acceso de desarrollo local como un token de portador al encabezado de autorización de las solicitudes HTTP
1. AEM as a Cloud Service recibe la solicitud HTTP, autentica la solicitud, realiza el trabajo solicitado por la solicitud HTTP y devuelve una respuesta HTTP de nuevo a la aplicación externa

### La aplicación externa de ejemplo

AEM Crearemos una aplicación JavaScript externa sencilla para ilustrar cómo acceder mediante programación a las aplicaciones as a Cloud Service a través de HTTPS mediante el token de acceso de desarrollador local. Esto ilustra cómo _cualquiera_ AEM AEM La aplicación o el sistema que se ejecuta fuera de la aplicación de, independientemente del marco de trabajo o del lenguaje, puede utilizar el token de acceso para autenticarse mediante programación en, y acceder a, as a Cloud Service. En el [sección siguiente](./service-credentials.md)Por lo tanto, actualizaremos este código de aplicación para que admita el método de generación de un token para uso en producción.

AEM Esta aplicación de ejemplo se ejecuta desde la línea de comandos y actualiza los metadatos de los recursos mediante las API HTTP de AEM Assets, utilizando el flujo siguiente:

1. Lee los parámetros de la línea de comandos (`getCommandLineParams()`)
1. AEM Obtiene el token de acceso utilizado para autenticarse en el as a Cloud Service de la (`getAccessToken(...)`)
1. AEM Muestra una lista de todos los recursos de una carpeta de recursos de especificada en un parámetro de línea de comandos (`listAssetsByFolder(...)`)
1. Actualice los metadatos de los recursos enumerados con los valores especificados en los parámetros de la línea de comandos (`updateMetadata(...)`)

AEM AEM El elemento clave en la autenticación mediante programación para utilizar el token de acceso es la adición de un encabezado de solicitud HTTP de autorización a todas las solicitudes HTTP realizadas a los, en el siguiente formato:

+ `Authorization: Bearer ACCESS_TOKEN`

## Ejecución de la aplicación externa

1. Asegúrese de que [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) está instalado en el equipo de desarrollo local, que se utiliza para ejecutar la aplicación externa
1. Descargue y descomprima el [aplicación externa de ejemplo](./assets/aem-guides_token-authentication-external-application.zip)
1. Desde la línea de comandos, en la carpeta de este proyecto, ejecute `npm install`
1. Copie el [descargó el token de acceso de desarrollo local](#download-local-development-access-token) a un archivo denominado `local_development_token.json` en la raíz del proyecto
   + Pero recuerde, ¡nunca confirme ninguna credencial a Git!
1. Abrir `index.js` y revise el código de la aplicación externa y los comentarios.

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   Revise la `fetch(..)` invocaciones de en `listAssetsByFolder(...)` y `updateMetadata(...)`, y aviso `headers` defina la `Authorization` Encabezado de solicitud HTTP con un valor de `Bearer ACCESS_TOKEN`. AEM Así es como la solicitud HTTP originada desde la aplicación externa se autentica para as a Cloud Service.

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   AEM Cualquier solicitud HTTP que se vaya a as a Cloud Service debe establecer el token de acceso al portador en el encabezado Autorización. AEM Recuerde, cada entorno as a Cloud Service de la requiere su propio token de acceso. El token de acceso de Desarrollo no funciona en Ensayo o Producción, el de Ensayo no funciona en Desarrollo o Producción, y el de Producción no funciona en Desarrollo o Ensayo.

1. Mediante la línea de comandos, desde la raíz del proyecto, ejecute la aplicación y pase los siguientes parámetros:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Se pasan los siguientes parámetros:

   + `aem`AEM : esquema y nombre de host del entorno as a Cloud Service con el que interactúa la aplicación (por ejemplo, `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: La ruta de la carpeta de recursos cuyos recursos se actualizan con el `propertyValue`; NO agregue el `/content/dam` prefijo (por ejemplo, `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: el nombre de la propiedad del recurso que se va a actualizar, relativo a `[dam:Asset]/jcr:content` (p. ej. `metadata/dc:rights`).
   + `propertyValue`: El valor para establecer la variable `propertyName` a; los valores con espacios deben encapsularse con `"` (p. ej. `"WKND Limited Use"`)
   + `file`AEM : Ruta relativa al archivo JSON descargado de la consola de desarrollador de.

   Una ejecución correcta de la aplicación genera el resultado de cada recurso actualizado:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### AEM Verificar la actualización de metadatos en el

AEM Compruebe que los metadatos se han actualizado iniciando sesión en el entorno as a Cloud Service de la (asegúrese de que se ha pasado el mismo host a la `aem` se accede al parámetro de la línea de comandos).

1. AEM Inicie sesión en el entorno as a Cloud Service de con el que interactuó la aplicación externa (utilice el mismo host proporcionado en la `aem` parámetro de línea de comandos)
1. Vaya a __Assets__ > __Archivos__
1. Navegue hasta la carpeta de recursos especificada por la variable `folder` parámetro de línea de comandos, por ejemplo __WKND__ > __Inglés__ > __Aventuras__ > __Cata de vinos de Napa__
1. Abra el __Propiedades__ para cualquier recurso (que no sea fragmento de contenido) de la carpeta
1. Toque para abrir __Avanzadas__ pestaña
1. Revise el valor de la propiedad actualizada, por ejemplo __Copyright__ que se asigna al actualizado `metadata/dc:rights` La propiedad JCR, que refleja el valor proporcionado en la variable `propertyValue` parámetro, por ejemplo __Uso limitado de WKND__

![Actualización de metadatos de uso limitado de WKND](./assets/local-development-access-token/asset-metadata.png)

## Pasos siguientes

AEM Ahora que hemos accedido mediante programación a la as a Cloud Service utilizando el token de desarrollo local, A continuación, debemos actualizar la aplicación para que se gestione mediante credenciales de servicio, de modo que esta aplicación se pueda utilizar en un contexto de producción.

+ [Cómo usar las credenciales del servicio](./service-credentials.md)
