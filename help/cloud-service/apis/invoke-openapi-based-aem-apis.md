---
title: Invocar las API de AEM basadas en OpenAPI para la autenticación entre servidores
description: Obtenga información sobre cómo configurar e invocar las API de AEM basadas en OpenAPI en AEM as a Cloud Service desde aplicaciones personalizadas mediante la autenticación de servidor a servidor OAuth.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Tutorial
jira: KT-16516
thumbnail: KT-16516.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 24c641e7-ab4b-45ee-bbc7-bf6b88b40276
source-git-commit: b3d053a09dfc8989441a21bf0d8c4771d816106f
workflow-type: tm+mt
source-wordcount: '1855'
ht-degree: 0%

---

# Invocar las API de AEM basadas en OpenAPI para la autenticación entre servidores{#invoke-openapi-based-aem-apis}

Aprenda a configurar e invocar las API de AEM basadas en OpenAPI en AEM as a Cloud Service desde aplicaciones personalizadas mediante la autenticación de _servidor a servidor OAuth_.

La autenticación de servidor a servidor OAuth es ideal para los servicios back-end que necesitan acceso a API sin interacción del usuario. Utiliza el tipo de concesión OAuth 2.0 _client_credentials_ para autenticar la aplicación cliente.

>[!AVAILABILITY]
>
>Las API de AEM basadas en API abiertas están disponibles como parte de un programa de acceso anticipado. Si está interesado en acceder a ellos, le recomendamos que envíe un correo electrónico a [aem-apis@adobe.com](mailto:aem-apis@adobe.com) con una descripción de su caso de uso.

En este tutorial, aprenderá a:

- Habilite el acceso a las API de AEM basadas en OpenAPI para su entorno de AEM as a Cloud Service.
- Cree y configure un proyecto de Adobe Developer Console (ADC) para acceder a las API de AEM mediante _autenticación de servidor a servidor OAuth_.
- Desarrolle una aplicación NodeJS de ejemplo que llame a la API de autor de Assets para recuperar metadatos de un recurso específico.

Antes de comenzar, asegúrese de revisar la sección [Acceso a las API de Adobe y conceptos relacionados](overview.md#accessing-adobe-apis-and-related-concepts).

## Requisitos previos

Para completar este tutorial, necesita lo siguiente:

- Entorno de AEM as a Cloud Service modernizado con lo siguiente:
   - Versión de AEM `2024.10.18459.20241031T210302Z` o posterior.
   - Nuevos perfiles de producto de estilo (si el entorno se creó antes de noviembre de 2024)

- El proyecto de muestra [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) debe implementarse en él.

- Acceso a [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- Instale [Node.js](https://nodejs.org/en/) en el equipo local para ejecutar la aplicación NodeJS de ejemplo.

## Pasos de desarrollo

Los pasos de desarrollo de alto nivel son los siguientes:

1. Modernización del entorno de AEM as a Cloud Service.
1. Habilite el acceso a las API de AEM.
1. Crear un proyecto de Adobe Developer Console (ADC).
1. Configurar proyecto de ADC
   1. Añadir las API de AEM deseadas
   1. Configurar su autenticación
   1. Asociar el perfil de producto con la configuración de autenticación
1. Configure la instancia de AEM para habilitar la comunicación del proyecto de ADC
1. Desarrollar una aplicación NodeJS de ejemplo
1. Verificar el flujo de extremo a extremo

## Modernización del entorno de AEM as a Cloud Service

Empecemos por modernizar el entorno de AEM as a Cloud Service. Este paso solo es necesario si el entorno no se moderniza.

La modernización del entorno de AEM as a Cloud Service es un proceso de dos pasos,

- Actualice a la última versión de AEM
- Añádale nuevos perfiles de producto.

### Actualizar instancia de AEM

Para actualizar la instancia de AEM, en la sección _Entornos_ de Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/), seleccione el icono de _puntos suspensivos_ junto al nombre del entorno y seleccione la opción **Actualizar**.

![Actualizar instancia de AEM](assets/update-aem-instance.png)

A continuación, haga clic en el botón **Enviar** y ejecute la canalización de pila completa sugerida.

![Seleccione la última versión de AEM](assets/select-latest-aem-release.png)

En mi caso, el nombre de la canalización de Fullstack es _Dev :: Fullstack-Deploy_ y el nombre del entorno de AEM es _wknd-program-dev_; puede variar en su caso.

### Añadir nuevos perfiles de producto

Para agregar nuevos perfiles de producto a la instancia de AEM, en la sección _Entornos_ de Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/), seleccione el icono de _puntos suspensivos_ junto al nombre del entorno y seleccione la opción **Agregar perfiles de producto**.

![Agregar nuevos perfiles de producto](assets/add-new-product-profiles.png)

Para revisar los perfiles de producto agregados recientemente, haga clic en el icono de _elipsis_ junto al nombre del entorno y seleccione **Administrar acceso** > **Perfiles de autor**.

La ventana _Admin Console_ muestra los perfiles de producto agregados recientemente.

![Revisar nuevos perfiles de producto](assets/review-new-product-profiles.png)

Los pasos anteriores completan la modernización del entorno de AEM as a Cloud Service.

## Habilitar el acceso a las API de AEM

Los nuevos perfiles de producto permiten el acceso a la API de AEM basada en OpenAPI en Adobe Developer Console (ADC).

Los perfiles de producto agregados recientemente están asociados con _Servicios_ que representan grupos de usuarios de AEM con Listas de control de acceso (ACL) predefinidas. Los _servicios_ se utilizan para controlar el nivel de acceso a las API de AEM.

También puede seleccionar o deseleccionar los _servicios_ asociados con el perfil de producto para reducir o aumentar el nivel de acceso.

Revise la asociación haciendo clic en el icono _Ver detalles_ junto al nombre del perfil del producto.

![Revisar servicios asociados con el perfil de producto](assets/review-services-associated-with-product-profile.png)

De manera predeterminada, el servicio **Usuarios de API de AEM Assets** no está asociado con ningún perfil de producto. Vamos a asociarlo con los **Administradores de AEM recién agregados: autor - Programa XXX - Entorno XXX** Perfil del producto. Después de esta asociación, la _API de autor de recursos_ del proyecto ADC puede configurar la autenticación de servidor a servidor OAuth y asociar la cuenta de autenticación con el perfil de producto.

![Asociar el servicio de usuarios de la API de AEM Assets con el perfil de producto](assets/associate-aem-assets-api-users-service-with-product-profile.png)

Es importante tener en cuenta que antes de la modernización, en la instancia de autor de AEM, había dos perfiles de producto disponibles, **Administradores de AEM-XXX** y **Usuarios de AEM-XXX**. También es posible asociar estos perfiles de producto existentes con los nuevos servicios.

## Crear proyecto de Adobe Developer Console (ADC)

A continuación, cree un proyecto de ADC para acceder a las API de AEM.

1. Inicie sesión en [Adobe Developer Console](https://developer.adobe.com/console) con su Adobe ID.

   ![Adobe Developer Console](assets/adobe-developer-console.png)

1. En la sección _Inicio rápido_, haga clic en el botón **Crear nuevo proyecto**.

   ![Crear nuevo proyecto](assets/create-new-project.png)

1. Crea un nuevo proyecto con el nombre predeterminado.

   ![Nuevo proyecto creado](assets/new-project-created.png)

1. Edite el nombre del proyecto haciendo clic en el botón **Editar proyecto** en la esquina superior derecha. Proporcione un nombre descriptivo y haga clic en **Guardar**.

   ![Editar nombre de proyecto](assets/edit-project-name.png)

## Configurar proyecto de ADC

A continuación, configure el proyecto ADC para agregar las API de AEM, configurar su autenticación y asociar el perfil de producto.

1. Para agregar las API de AEM, haga clic en el botón **Agregar API**.

   ![Agregar API](assets/add-api.png)

1. En el cuadro de diálogo _Agregar API_, filtre por _Experience Cloud_, seleccione la tarjeta **API de autor de AEM Assets** y haga clic en **Siguiente**.

   ![Agregar API de AEM](assets/add-aem-api.png)

1. A continuación, en el diálogo _Configurar API_, seleccione la opción de autenticación **Servidor a servidor** y haga clic en **Siguiente**. La autenticación de servidor a servidor es ideal para los servicios back-end que necesitan acceso a API sin interacción del usuario.

   ![Seleccionar autenticación](assets/select-authentication.png)

1. Cambie el nombre de la credencial para facilitar la identificación (si es necesario) y haga clic en **Siguiente**. Para fines de demostración, se utiliza el nombre predeterminado.

   ![Cambiar nombre de credencial](assets/rename-credential.png)

1. Seleccione el perfil de producto **Administradores de AEM - Autor - Programa XXX - Entorno XXX** y haga clic en **Guardar**. Como puede ver, solo está disponible para su selección el perfil de producto asociado al servicio de usuarios de la API de AEM Assets.

   ![Seleccionar perfil de producto](assets/select-product-profile.png)

   >[!CAUTION]
   >
   >    Tenga en cuenta que el usuario de la cuenta de servicio (también conocida como cuenta técnica) obtiene acceso COMPLETO ya que está asociado con el perfil de producto de **Administradores de AEM - XX - XX**.


1. Revise la configuración de autenticación y la API de AEM.

   ![Configuración de la API de AEM](assets/aem-api-configuration.png)

   ![Configuración de autenticación](assets/authentication-configuration.png)

## Configuración de la instancia de AEM para habilitar la comunicación del proyecto ADC

Para habilitar el ID de cliente de la credencial de servidor a servidor OAuth del proyecto ADC para comunicarse con la instancia de AEM, debe configurar la instancia de AEM.

Se realiza definiendo la configuración en el archivo `config.yaml` en el proyecto de AEM. A continuación, implemente el archivo `config.yaml` mediante la canalización de configuración en Cloud Manager.

1. En AEM Project, busque o cree el archivo `config.yaml` en la carpeta `config`.

   ![Localizar configuración YAML](assets/locate-config-yaml.png)

1. Agregue la siguiente configuración al archivo `config.yaml`.

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's OAuth Server-to-Server credential ClientID>"
   ```

   Reemplace `<ADC Project's OAuth Server-to-Server credential ClientID>` por el ClientID real de la credencial de servidor a servidor OAuth del proyecto ADC. El extremo de API que se usa en este tutorial solo está disponible en el nivel de creación, pero para otras API, la configuración yaml también puede tener un nodo _publish_ o _preview_.

   >[!CAUTION]
   >
   > Para fines de demostración, se utiliza el mismo ClientID para todos los entornos. Se recomienda utilizar ClientID independiente por entorno (dev, stage, prod) para mejorar la seguridad y el control.

1. Confirme los cambios de configuración en el repositorio de Git e inserte los cambios en el repositorio remoto.

1. Implemente los cambios anteriores mediante la canalización de configuración en Cloud Manager. Tenga en cuenta que el archivo `config.yaml` también se puede instalar en un RDE mediante herramientas de la línea de comandos.

   ![Implementar config.yaml](assets/config-pipeline.png)

## Desarrollar una aplicación NodeJS de ejemplo

Desarrollemos una aplicación NodeJS de ejemplo que llame a la API de autor de Assets.

Puede utilizar otros lenguajes de programación como Java, Python, etc. para desarrollar la aplicación.

Para hacer pruebas, puede usar [Postman](https://www.postman.com/), [curl](https://curl.se/) o cualquier otro cliente REST para invocar las API de AEM.

### Revisión de la API

Antes de desarrollar la aplicación, vamos a revisar [el extremo ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata) de los metadatos del recurso especificado desde la _API de autor de Assets_. La sintaxis de la API es:

```http
GET https://{bucket}.adobeaemcloud.com/adobe/assets/{assetId}/metadata
```

Para recuperar los metadatos de un recurso específico, necesita los valores `bucket` y `assetId`. `bucket` es el nombre de instancia de AEM sin el nombre de dominio de Adobe (.adobeaemcloud.com), por ejemplo, `author-p63947-e1420428`.

`assetId` es el UUID JCR del recurso con el prefijo `urn:aaid:aem:`, por ejemplo, `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da`. Existen varias formas de obtener `assetId`:

- Anexe la extensión de la ruta de acceso del recurso de AEM `.json` para obtener los metadatos del recurso. Por ejemplo, `https://author-p63947-e1420429.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg.json` y busque la propiedad `jcr:uuid`.

- También puede obtener `assetId` si inspecciona el recurso en el inspector de elementos del explorador. Busque el atributo `data-id="urn:aaid:aem:..."`.

  ![Inspeccionar recurso](assets/inspect-asset.png)

### Invocar la API mediante el explorador

Antes de desarrollar la aplicación, invoquemos la API con la función **Probarla** en la [documentación de la API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata).

1. Abra [la documentación de la API de autor de Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author) en el explorador.

1. Expanda la sección _Metadatos_ y haga clic en la opción **Entrega los metadatos del recurso especificado**.

1. En el panel derecho, haga clic en el botón **Probarlo**.
   ![Documentación de API](assets/api-documentation.png)

1. Introduzca los siguientes valores:
   1. El valor `bucket` es el nombre de instancia de AEM sin el nombre de dominio de Adobe (.adobeaemcloud.com), por ejemplo, `author-p63947-e1420428`.

   1. Los valores `Bearer Token` y `X-Api-Key` relacionados con la sección **Security** se obtienen de la credencial de servidor a servidor OAuth del proyecto ADC. Haga clic en **Generar token de acceso** para obtener el valor `Bearer Token` y usar el valor `ClientID` como `X-Api-Key`.
      ![Generar token de acceso](assets/generate-access-token.png)

   1. El valor `assetId` relacionado con la sección **Parameters** es el identificador único del recurso en AEM. `X-Adobe-Accept-Experimental` está establecido en 1.

      ![Invocar API - valores de entrada](assets/invoke-api-input-values.png)

1. Haga clic en **Enviar** para invocar la API.

1. Revise la ficha **Respuesta** para ver la respuesta de la API.

   ![Invocar API - respuesta](assets/invoke-api-response.png)

Los pasos anteriores confirman la modernización del entorno de AEM as a Cloud Service, lo que permite el acceso a las API de AEM. También confirma la configuración correcta del proyecto ADC y la comunicación de ID de cliente de credencial de servidor a servidor OAuth con la instancia de autor de AEM.

### Aplicación NodeJS de muestra

Vamos a desarrollar una aplicación NodeJS de ejemplo.

Para desarrollar la aplicación, puede usar las instrucciones _Ejecutar-la-aplicación-de-ejemplo_ o _Desarrollo-paso-a-paso_.


>[!BEGINTABS]

>[!TAB Ejecutar-la-aplicación-de-ejemplo]

1. Descargue el archivo zip de la aplicación [demo-nodejs-app-to-invoke-aem-openapi](assets/demo-nodejs-app-to-invoke-aem-openapi.zip) de muestra y extráigalo.

1. Vaya a la carpeta extraída e instale las dependencias.

   ```bash
   $ npm install
   ```

1. Reemplace los marcadores de posición del archivo `.env` por los valores reales de la credencial de servidor a servidor OAuth del proyecto ADC.

1. Reemplazar `<BUCKETNAME>` y `<ASSETID>` en el archivo `src/index.js` con los valores reales.

1. Ejecute la aplicación NodeJS.

   ```bash
   $ node src/index.js
   ```

>[!TAB Desarrollo paso a paso]

1. Cree un nuevo proyecto de NodeJS.

   ```bash
   $ mkdir demo-nodejs-app-to-invoke-aem-openapi
   $ cd demo-nodejs-app-to-invoke-aem-openapi
   $ npm init -y
   ```

1. Instale la biblioteca _fetch_ y _dotenv_ para realizar solicitudes HTTP y leer las variables de entorno respectivamente.

   ```bash
   $ npm install node-fetch
   $ npm install dotenv
   ```

1. Abra el proyecto en su editor de código favorito y actualice el archivo `package.json` para agregar `type` a `module`.

   ```json
   {
       ...
       "version": "1.0.0",
       "type": "module",
       "main": "index.js",
       ...
   }
   ```

1. Cree el archivo `.env` y agregue la configuración siguiente. Reemplace los marcadores de posición por los valores reales de la credencial de servidor a servidor OAuth del proyecto ADC.

   ```properties
   CLIENT_ID=<ADC Project OAuth Server-to-Server credential ClientID>
   CLIENT_SECRET=<ADC Project OAuth Server-to-Server credential Client Secret>
   SCOPES=<ADC Project OAuth Server-to-Server credential Scopes>
   ```

1. Cree el archivo `src/index.js`, agregue el código siguiente y reemplace `<BUCKETNAME>` y `<ASSETID>` por los valores reales.

   ```javascript
   // Import the dotenv configuration to load environment variables from the .env file
   import "dotenv/config";
   
   // Import the fetch function to make HTTP requests
   import fetch from "node-fetch";
   
   // REPLACE THE FOLLOWING VALUES WITH YOUR OWN
   const bucket = "<BUCKETNAME>"; // Bucket name is the AEM instance name (e.g. author-p63947-e1420428)
   const assetId = "<ASSETID>"; // Asset ID is the unique identifier for the asset in AEM (e.g. urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da). You can get it by inspecting the asset in browser's element inspector, look for data-id="urn:aaid:aem:..."
   
   // Load environment variables for authentication
   const clientId = process.env.CLIENT_ID; // Adobe IMS client ID
   const clientSecret = process.env.CLIENT_SECRET; // Adobe IMS client secret
   const scopes = process.env.SCOPES; // Scope for the API access
   
   // Adobe IMS endpoint for obtaining an access token
   const adobeIMSV3TokenEndpointURL =
   "https://ims-na1.adobelogin.com/ims/token/v3";
   
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
       console.log("Getting access token from IMS"); // Log process initiation
       //console.log("Client ID: " + clientId); // Display client ID for debugging purposes
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       //console.log("Response status: " + response.status); // Log the HTTP status for debugging
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       console.log("Access token received"); // Log success message
   
       // Return the access token
       return responseJSON.access_token;
   };
   
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   
   // Call the getAssets function to start the process
   getAssetMetadat();
   ```

1. Ejecute la aplicación NodeJS.

   ```bash
   $ node src/index.js
   ```

>[!ENDTABS]

### Respuesta de API

Una vez ejecutada correctamente, la respuesta de la API se muestra en la consola. La respuesta contiene los metadatos del recurso especificado.

```json
{
  "assetId": "urn:aaid:aem:9c09ff70-9ee8-4b14-a5fa-ec37baa0d1b3",
  "assetMetadata": {    
    ...
    "dc:title": "A Young Mountain Biking Couple Takes A Minute To Take In The Scenery",
    "xmp:CreatorTool": "Adobe Photoshop Lightroom Classic 7.5 (Macintosh)",
    ...
  },
  "repositoryMetadata": {
    ...
    "repo:name": "adobestock-221043703.jpg",
    "repo:path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg",
    "repo:state": "ACTIVE",
    ...
  }
}
```

¡Enhorabuena! Ha invocado correctamente las API de AEM basadas en OpenAPI desde la aplicación personalizada mediante la autenticación de servidor a servidor OAuth.

### Revisar el código de la aplicación

Las llamadas clave desde el código de aplicación NodeJS de ejemplo son:

1. **Autenticación IMS**: obtiene un token de acceso mediante la configuración de credenciales de servidor a servidor OAuth en el proyecto ADC.

   ```javascript
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token from Adobe IMS token endpoint https://ims-na1.adobelogin.com/ims/token/v3
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       // Return the access token
       return responseJSON.access_token;
   };
   ...
   ```

1. **Invocación de API**: invoca la API de autor de Assets para recuperar los metadatos de un recurso específico al proporcionar el token de acceso para la autorización.

   ```javascript
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   ...
   ```

## Resumen

En este tutorial, ha aprendido a invocar las API de AEM basadas en OpenAPI desde aplicaciones personalizadas. Ha habilitado el acceso a las API de AEM, y ha creado y configurado un proyecto de Adobe Developer Console (ADC).
En el proyecto de ADC, ha añadido las API de AEM, ha configurado su tipo de autenticación y ha asociado el perfil de producto. También configuró la instancia de AEM para habilitar la comunicación del proyecto ADC y desarrolló una aplicación NodeJS de ejemplo que llama a la API de autor de Assets.
