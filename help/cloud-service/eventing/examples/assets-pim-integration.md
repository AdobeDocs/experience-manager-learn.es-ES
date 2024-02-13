---
title: Eventos de AEM Assets para la integración con PIM
description: Obtenga información sobre cómo integrar AEM Assets y sistemas de administración de la información del producto (PIM) para actualizaciones de metadatos de recursos.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-02-13T00:00:00Z
jira: KT-14901
thumbnail: KT-14901.jpeg
source-git-commit: f150a2517c4cafe55917e1aa50dca297c9bb3bc5
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 0%

---


# Eventos de AEM Assets para la integración con PIM

Obtenga información sobre cómo integrar AEM Assets y el sistema de administración de la información del producto (PIM) para actualizar los metadatos de los recursos **AEM uso de ventilación de la**. Al recibir un evento de AEM Assets AEM, los metadatos de los recursos se pueden actualizar en sistemas de gestión de recursos, PIM o ambos, según los requisitos comerciales. AEM Sin embargo, en este ejemplo, vamos a actualizar los metadatos de los recursos en la.

Para ejecutar la actualización de metadatos del recurso **AEM código fuera de la**, el [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) se utiliza una plataforma sin servidor. El flujo de procesamiento de eventos es el siguiente:

![Eventos de AEM Assets para la integración con PIM](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. AEM Los déclencheur del servicio de autor de la _Procesamiento de recursos completado_ evento cuando se completa la carga de un recurso.
1. El evento se envía a [Eventos de Adobe I/O](https://developer.adobe.com/events/) servicio.
1. El servicio Eventos de Adobe I/O pasa el evento a [Acción de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) para su procesamiento.
1. La acción de Adobe I/O Runtime llama a la API de PIM burlada para recuperar metadatos adicionales como el SKU o la información del proveedor.
1. El PIM recuperado de metadatos adicionales se actualiza en AEM Assets utilizando el [API de autor de Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

## Requisitos previos

Para completar este tutorial, necesita lo siguiente:

- AEM entorno as a Cloud Service con [AEM Eventing activado de](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Además, la muestra [Sitios WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) el proyecto debe implementarse en él.

- Acceso a [Consola de Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [CLI de Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) instalado en el equipo local.

## Pasos de desarrollo

Los pasos de desarrollo de alto nivel son los siguientes:

1. [Crear un proyecto en la consola de Adobe Developer (ADC)](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [Inicializar proyecto para desarrollo local](./runtime-action.md#initialize-project-for-local-development)
1. Configurar un proyecto en ADC
1. AEM Configurar el servicio de autor de la para habilitar la comunicación del proyecto ADC
1. Desarrollar una acción de tiempo de ejecución que organice la recuperación y actualización de metadatos
1. AEM Cargar un recurso en el servicio de creación de y comprobar la actualización de metadatos

Para ver los pasos detallados del 1 al 2, consulte [Eventos de acción y de de Adobe I/O Runtime AEM](./runtime-action.md#) Por ejemplo, y para 3-6 consulte las siguientes secciones.

### Configuración de un proyecto en la consola de Adobe Developer (ADC)

Para recibir eventos de AEM Assets y ejecutar la acción de Adobe I/O Runtime creada en el paso anterior, configure el proyecto en ADC.

- En ADC, vaya a [proyecto](https://developer.adobe.com/console/projects). Seleccione el `Stage` espacio de trabajo, aquí es donde se implementó la acción de tiempo de ejecución.

- Clic **Añadir servicio** y seleccione **Evento** opción. En el **Añadir eventos** diálogo, seleccione **Experience Cloud** > **AEM Assets** y haga clic en **Siguiente**. Siga los pasos de configuración adicionales, seleccione la instancia de AEM CS, _Procesamiento de recursos completado_ evento, tipo de autenticación de servidor a servidor OAuth y otros detalles.

  ![Evento de AEM Assets: añadir evento](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- Finalmente, en la **Cómo recibir eventos** paso, expandir **Acción Runtime** y seleccione la opción _genérico_ acción creada en el paso anterior. Clic **Guardar eventos configurados**.

  ![Evento de AEM Assets: recibir evento](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- Igualmente, haga clic en **Añadir servicio** y seleccione **API** opción. En el **Añadir una API** modal, seleccione **Experience Cloud** > **AEM API AS A CLOUD SERVICE** y haga clic en **Siguiente**.

  ![AEM Añadir API as a Cloud Service: Configurar el proyecto](../assets/examples/assets-pim-integration/add-aem-api.png)

- A continuación seleccione **Servidor a servidor OAuth** para el tipo de autenticación y haga clic en **Siguiente**.

- A continuación, seleccione la **AEM Administradores de-XXX** perfil del producto y haga clic en **Guardar API configurada**. Para obtener acceso a las funciones granulares y a los permisos, el perfil de producto seleccionado debe estar asociado al evento de AEM Assets que produce el entorno de AEM CS.

  ![AEM Añadir API as a Cloud Service: Configurar el proyecto](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

### AEM Configurar el servicio de autor de la para habilitar la comunicación del proyecto ADC

AEM AEM Para actualizar los metadatos de los recursos en la desde el proyecto ADC anterior, configure el servicio de autor de la configuración con el ID de cliente del proyecto ADC. El _id de cliente_ se agrega como variable de entorno usando la variable [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html#add-variables) IU.

- Inicie sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/), seleccione **Programa** > **Entorno** > **Puntos suspensivos** > **Ver detalles** > **Configuración** pestaña.

  ![Adobe Cloud Manager: Configuración del entorno](../assets/examples/assets-pim-integration/cloud-manager-environment-configuration.png)

- Entonces **Agregar configuración** e introduzca los detalles de la variable como

  | Nombre | Value | AEM servicio de | Tipo |
  | ----------- | ----------- | ----------- | ----------- |
  | ADOBE_PROVIDED_CLIENT_ID | &lt;COPY_FROM_ADC_PROJECT_CREDENTIALS> | Autor | Variable |

  ![Adobe Cloud Manager: Configuración del entorno](../assets/examples/assets-pim-integration/add-environment-variable.png)

- Clic **Añadir** y **Guardar** la configuración.

### Desarrollar acción de tiempo de ejecución

Para realizar la recuperación y actualización de metadatos, comience actualizando el creado automáticamente _genérico_ código de acción en `src/dx-excshell-1/actions/generic` carpeta.

Consulte el adjunto [WKND-Assets-PIM-Integration.zip](../assets/examples/assets-pim-integration/WKND-Assets-PIM-Integration.zip) para obtener el código completo, y en la sección siguiente se destacan los archivos clave.

- El `src/dx-excshell-1/actions/generic/mockPIMCommunicator.js` se burla de la llamada de API de PIM para recuperar metadatos adicionales como el SKU y el nombre del proveedor.

  ```javascript
  /**
   * Mock PIM API to get the product data such as SKU, Supplier, etc.
   *
   * In a real-world scenario, this function would call the PIM API to get the product data.
   * For this example, we are returning mock data.
   *
   * @param {string} assetId - The assetId to get the product data.
   */
  module.exports = {
      async getPIMData(assetId) {
          if (!assetId) {
          throw new Error('Invalid assetId');
          }
          // Mock response data for demo purposes
          const data = {
          SKUID: 'MockSKU 123',
          SupplierName: 'mock-supplier',
          // ... other product data
          };
          return data;
      },
  };
  ```

- El `src/dx-excshell-1/actions/generic/aemCommunicator.js` AEM El archivo actualiza los metadatos del recurso en mediante el uso de la variable de archivo [API de autor de Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

  ```javascript
  const fetch = require('node-fetch');
  
  ...
  
  /**
  *  Get IMS Access Token using Client Credentials Flow
  *
  * @param {*} clientId - IMS Client ID from ADC project's OAuth Server-to-Server Integration
  * @param {*} clientSecret - IMS Client Secret from ADC project's OAuth Server-to-Server Integration
  * @param {*} scopes - IMS Meta Scopes from ADC project's OAuth Server-to-Server Integration as comma separated strings
  * @returns {string} - Returns the IMS Access Token
  */
  async function getIMSAccessToken(clientId, clientSecret, scopes) {
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';
  
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
    };
  
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();
  
    return responseJSON.access_token;
  }    
  
  async function updateAEMAssetMetadata(metadataDetails, aemAssetEvent, params) {
    ...
    // Transform the metadata details to JSON Patch format,
    // see https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/patchAssetMetadata
    const transformedMetadata = Object.keys(metadataDetails).map((key) => ({
      op: 'add',
      path: `wknd-${key.toLowerCase()}`,
      value: metadataDetails[key],
    }));
  
    ...
  
    // Get ADC project's OAuth Server-to-Server Integration credentials
    const clientId = params.ADC_CECREDENTIALS_CLIENTID;
    const clientSecret = params.ADC_CECREDENTIALS_CLIENTSECRET;
    const scopes = params.ADC_CECREDENTIALS_METASCOPES;
  
    // Get IMS Access Token using Client Credentials Flow
    const access_token = await getIMSAccessToken(clientId, clientSecret, scopes);
  
    // Call AEM Author service to update the metadata using Assets Author API
    // See https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/
    const res = await fetch(`${aemAuthorHost}/adobe/assets/${assetId}/metadata`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json-patch+json',
        'If-Match': '*',
        'X-Adobe-Accept-Experimental': '1',
        'X-Api-Key': 'aem-assets-management-api', // temporary value
        Authorization: `Bearer ${access_token}`,
      },
      body: JSON.stringify(transformedMetadata),
    });
  
    ...
  }
  
  module.exports = { updateAEMAssetMetadata };
  ```

  El `.env` El archivo almacena los detalles de credenciales de servidor a servidor OAuth del proyecto ADC y se pasan como parámetros a la acción mediante `ext.config.yaml` archivo. Consulte la [Archivos de configuración de App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) para administrar secretos y parámetros de acción.

- El `src/dx-excshell-1/actions/model` La carpeta contiene `aemAssetEvent.js` y `errors.js` archivos, que utiliza la acción para analizar el evento recibido y controlar los errores respectivamente.

- El `src/dx-excshell-1/actions/generic/index.js` utiliza los módulos mencionados para organizar la recuperación y actualización de metadatos.

  ```javascript
  ...
  
  let responseMsg;
  // handle the challenge probe request, they are sent by I/O to verify the action is valid
  if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
  } else {
    logger.info('AEM Asset Event request received');
  
    // create AEM Asset Event object from request parameters
    const aemAssetEvent = new AEMAssetEvent(params);
  
    // Call mock PIM API to get the product data such as SKU, Supplier, etc.
    const mockPIMData = await mockPIMAPI.getPIMData(
      aemAssetEvent.getAssetName(),
    );
    logger.info('Mock PIM API response', mockPIMData);
  
    // Update PIM received data in AEM as Asset metadata
    const aemUpdateStatus = await updateAEMAssetMetadata(
      mockPIMData,
      aemAssetEvent,
      params,
    );
    logger.info('AEM Asset metadata update status', aemUpdateStatus);
  
    if (aemUpdateStatus) {
      // create response message
      responseMsg = JSON.stringify({
        message:
          'AEM Asset Event processed successfully, updated the asset metadata with PIM data.',
        assetdata: {
          assetName: aemAssetEvent.getAssetName(),
          assetPath: aemAssetEvent.getAssetPath(),
          assetId: aemAssetEvent.getAssetId(),
          aemHost: aemAssetEvent.getAEMHost(),
          pimdata: mockPIMData,
        },
      });
    } 
  
    // response object
    const response = {
      statusCode: 200,
      body: responseMsg,
    };
  
    // Return the response to the caller
    return response;
  
    ...
  }
  ```

Implemente la acción actualizada en Adobe I/O Runtime con el siguiente comando:

```bash
$ aio app deploy
```

### Verificación de metadatos y carga de recursos

Para comprobar la integración de AEM Assets y PIM, siga estos pasos:

- Para ver los metadatos de prueba proporcionados por el PIM, como el SKU y el nombre del proveedor, cree un esquema de metadatos en AEM Assets, consulte [Esquemas de metadatos](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/configuring/metadata-schemas.html) que muestra las propiedades de metadatos SKU y nombre del proveedor.

- AEM Cargue un recurso en el servicio de creación de y compruebe la actualización de los metadatos.

  ![Actualización de metadatos de AEM Assets](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## Concepto y elementos clave

AEM La sincronización de metadatos de recursos entre los sistemas de administración de recursos y otros sistemas, como PIM, suele ser necesaria en la empresa. AEM Se puede lograr el uso de ventilación por parte de los EASYVENTING.

- AEM AEM El código de metadatos del recurso se ejecuta fuera de la, lo que evita la carga en el servicio de creación, por lo que la arquitectura orientada a eventos que se adapta de forma independiente.
- AEM La recién introducida API de autor de recursos se utiliza para actualizar los metadatos de los recursos en la creación de recursos de la.
- La autenticación de la API utiliza OAuth servidor a servidor (también conocido como flujo de credenciales de cliente). Consulte [Guía de implementación de credenciales de servidor a servidor de OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/).
- En lugar de Acciones de Adobe I/O Runtime, se pueden utilizar otros webhooks o Amazon EventBridge para recibir el evento de AEM Assets y procesar la actualización de metadatos.
- AEM Eventos de recursos a través de la ventilación de la permite a las empresas automatizar y optimizar los procesos críticos, fomentando la eficacia y la coherencia en todo el ecosistema de contenido.

