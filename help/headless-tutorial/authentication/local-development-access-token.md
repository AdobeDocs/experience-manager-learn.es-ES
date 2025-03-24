---
title: Token de acceso de desarrollo local
description: Los tokens de acceso de desarrollo local de AEM se utilizan para acelerar el desarrollo de integraciones con AEM as a Cloud Service que interactúa mediante programación con los servicios de AEM Author o Publish a través de HTTP.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 572
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# Token de acceso de desarrollo local

Los desarrolladores que crean integraciones que requieren acceso programático a AEM as a Cloud Service necesitan una forma sencilla y rápida de obtener tokens de acceso temporales para AEM a fin de facilitar las actividades de desarrollo local. Para satisfacer esta necesidad, Developer Console de AEM permite a los desarrolladores generar tokens de acceso temporales que se pueden utilizar para acceder a AEM mediante programación.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## Generar un token de acceso de desarrollo local

![Obteniendo un token de acceso de desarrollo local](assets/local-development-access-token/getting-a-local-development-access-token.png)

El token de acceso de desarrollo local proporciona acceso a los servicios de AEM Author y Publish como usuario que ha generado el token, junto con sus permisos. A pesar de ser un token de desarrollo, no lo comparta ni lo almacene en el control de código fuente.

1. En [Adobe Admin Console](https://adminconsole.adobe.com/), asegúrese de que usted, el desarrollador, sea miembro de:
   + __Cloud Manager - Developer__ Perfil de producto de IMS (otorga acceso a AEM Developer Console)
   + El perfil de producto de IMS __Administradores de AEM__ o __Usuarios de AEM__ para el servicio del entorno de AEM con el que se integra el token de acceso
   + El entorno de espacio aislado de AEM as a Cloud Service solo requiere pertenencia a __Administradores de AEM__ o a __Usuarios de AEM__ Perfil de producto
1. Inicie sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra el programa que contiene el entorno de AEM as a Cloud Service con el que desea integrarse
1. Pulse los __puntos suspensivos__ junto al entorno en la sección __Entornos__ y seleccione __Developer Console__
1. Pulse en la ficha __Integraciones__
1. Pulse la pestaña __Token local__
1. Pulse el botón __Obtener token de desarrollo local__
1. Pulse __botón de descarga__ en la esquina superior izquierda para descargar el archivo JSON que contiene el valor `accessToken` y guarde el archivo JSON en una ubicación segura de su equipo de desarrollo.
   + Este es su token de acceso de desarrollador de 24 horas al entorno de AEM as a Cloud Service.

![AEM Developer Console - Integraciones - Obtener token de desarrollo local](./assets/local-development-access-token/developer-console.png)

## Se utilizó el token de acceso de desarrollo local{#use-local-development-access-token}

![Token De Acceso De Desarrollo Local - Aplicación Externa](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Descargue el token de acceso de desarrollo local temporal de AEM Developer Console
   + El token de acceso de desarrollo local caduca cada 24 horas, por lo que los desarrolladores deben descargar nuevos tokens de acceso diariamente
1. Se está desarrollando una aplicación externa que interactúa mediante programación con AEM as a Cloud Service
1. La aplicación externa lee el token de acceso de desarrollo local
1. La aplicación externa construye solicitudes HTTP a AEM as a Cloud Service y agrega el token de acceso de desarrollo local como token de portador al encabezado de autorización de las solicitudes HTTP
1. AEM as a Cloud Service recibe la solicitud HTTP, autentica la solicitud, realiza el trabajo solicitado por la solicitud HTTP y devuelve una respuesta HTTP de nuevo a la aplicación externa

### La aplicación externa de ejemplo

Crearemos una aplicación de JavaScript externa sencilla para ilustrar cómo acceder mediante programación a AEM as a Cloud Service a través de HTTPS mediante el token de acceso de desarrollador local. Esto ilustra cómo _cualquier aplicación o sistema de_ que se ejecute fuera de AEM, independientemente del marco de trabajo o del lenguaje, puede utilizar el token de acceso para autenticarse mediante programación en AEM as a Cloud Service y tener acceso a. En la [siguiente sección](./service-credentials.md), actualizaremos este código de aplicación para admitir el método de generación de un token para uso de producción.

Esta aplicación de ejemplo se ejecuta desde la línea de comandos y actualiza los metadatos de los recursos de AEM mediante las API HTTP de AEM Assets mediante el siguiente flujo:

1. Lee los parámetros de la línea de comandos (`getCommandLineParams()`)
1. Obtiene el token de acceso utilizado para autenticarse en AEM as a Cloud Service (`getAccessToken(...)`)
1. Enumera todos los recursos de una carpeta de recursos de AEM especificada en parámetros de línea de comandos (`listAssetsByFolder(...)`)
1. Actualizar los metadatos de los recursos enumerados con los valores especificados en los parámetros de la línea de comandos (`updateMetadata(...)`)

El elemento clave para autenticarse mediante programación en AEM mediante el token de acceso es agregar un encabezado de solicitud HTTP de autorización a todas las solicitudes HTTP realizadas a AEM, en el siguiente formato:

+ `Authorization: Bearer ACCESS_TOKEN`

## Ejecución de la aplicación externa

1. Asegúrese de que [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) esté instalado en el equipo de desarrollo local, que se usa para ejecutar la aplicación externa
1. Descargue y descomprima la [aplicación externa de ejemplo](./assets/aem-guides_token-authentication-external-application.zip)
1. Desde la línea de comandos, en la carpeta de este proyecto, ejecute `npm install`
1. Copie el [token de acceso de desarrollo local descargado](#download-local-development-access-token) en un archivo de nombre `local_development_token.json` en la raíz del proyecto
   + Pero recuerde, ¡nunca confirme ninguna credencial a Git!
1. Abra `index.js` y revise el código de la aplicación externa y los comentarios.

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

   Revise las invocaciones de `fetch(..)` en `listAssetsByFolder(...)` y `updateMetadata(...)`, y observe que `headers` define el encabezado de solicitud HTTP de `Authorization` con un valor de `Bearer ACCESS_TOKEN`. Así es como la solicitud HTTP originada desde la aplicación externa se autentica en AEM as a Cloud Service.

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

   Cualquier solicitud HTTP a AEM as a Cloud Service debe establecer el token de acceso al portador en el encabezado Autorización. Recuerde, cada entorno de AEM as a Cloud Service requiere su propio token de acceso. El token de acceso de Desarrollo no funciona en Ensayo o Producción, el de Ensayo no funciona en Desarrollo o Producción, y el de Producción no funciona en Desarrollo o Ensayo.

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

   + `aem`: esquema y nombre de host del entorno de AEM as a Cloud Service con el que interactúa la aplicación (p. ej. `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: ruta de la carpeta de recursos cuyos recursos se actualizaron con `propertyValue`; NO agregue el prefijo `/content/dam` (por ejemplo: `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: nombre de propiedad de recurso que se va a actualizar, relativo a `[dam:Asset]/jcr:content` (p. ej. `metadata/dc:rights`).
   + `propertyValue`: el valor para establecer `propertyName` en; los valores con espacios deben encapsularse con `"` (p. ej. `"WKND Limited Use"`)
   + `file`: la ruta relativa al archivo JSON descargado de AEM Developer Console.

   Una ejecución correcta de la aplicación genera el resultado de cada recurso actualizado:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Verificar la actualización de metadatos en AEM

Compruebe que los metadatos se hayan actualizado iniciando sesión en el entorno de AEM as a Cloud Service (asegúrese de que se tiene acceso al mismo host pasado al parámetro de línea de comandos `aem`).

1. Inicie sesión en el entorno de AEM as a Cloud Service con el que interactuó la aplicación externa (utilice el mismo host proporcionado en el parámetro de línea de comandos `aem`)
1. Vaya a __Assets__ > __Archivos__
1. Navegue por la carpeta de recursos especificada por el parámetro de línea de comandos `folder`, por ejemplo __WKND__ > __Inglés__ > __Aventuras__ > __Cata de vinos de Napa__
1. Abra __Propiedades__ para cualquier recurso (que no sea fragmento de contenido) de la carpeta
1. Pulse la pestaña __Avanzado__
1. Revise el valor de la propiedad actualizada, por ejemplo __Copyright__, que está asignado a la propiedad JCR `metadata/dc:rights` actualizada, que refleja el valor proporcionado en el parámetro `propertyValue`, por ejemplo __Uso limitado de WKND__

![Actualización de metadatos de uso limitado de WKND](./assets/local-development-access-token/asset-metadata.png)

## Pasos siguientes

Ahora que hemos accedido a AEM as a Cloud Service mediante programación utilizando el token de desarrollo local. A continuación, debemos actualizar la aplicación para que se gestione mediante credenciales de servicio, de modo que esta aplicación se pueda utilizar en un contexto de producción.

+ [Cómo usar las credenciales del servicio](./service-credentials.md)
