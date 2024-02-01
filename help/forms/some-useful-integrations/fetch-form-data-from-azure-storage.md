---
title: Envío de formularios de tienda en Azure Storage
description: Almacenar datos de formulario en Azure Storage mediante la API de REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 81
exl-id: 77f93aad-0cab-4e52-b0fd-ae5af23a13d0
source-git-commit: b1734f75bdda174788d880be28fa19f8e787af0a
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Buscar datos del almacenamiento de Azure

Este artículo muestra cómo rellenar un formulario adaptable con los datos almacenados en Azure Storage.
Se da por hecho que ha almacenado los datos del formulario adaptable en Azure Storage y ahora desea rellenar previamente el formulario adaptable con esos datos.
>[!NOTE]
>El código de este artículo no funciona con los componentes principales basados en el formulario adaptable.[El artículo equivalente para el formulario adaptable basado en componentes principales está disponible aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/prefill-form-with-data-attachments/introduction.html?lang=en)


## Crear solicitud de GET

El siguiente paso es escribir el código para recuperar los datos del almacenamiento de Azure con el blobID. El siguiente código se escribió para recuperar los datos. La URL se construyó utilizando los valores sasToken y storageURI de la configuración OSGi y el blobID pasado a la función getBlobData

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Cuando se procesa un formulario adaptable con un parámetro guid en la dirección URL, el componente de página personalizada asociado a la plantilla recupera y rellena el formulario adaptable con los datos del almacenamiento de Azure.
El siguiente es el código en el jsp del componente de página asociado a la plantilla

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## Prueba de la solución

* [Implementar el paquete OSGi personalizado](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importe la plantilla de formulario adaptable personalizada y el componente de página asociado a la plantilla](./assets/store-and-fetch-from-azure.zip)

* [Importar el formulario adaptable de ejemplo](./assets/bank-account-sample-form.zip)

* [Especifique los valores adecuados en la configuración de Azure Portal mediante la consola de configuración OSGi.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/some-useful-integrations/store-form-data-in-azure-storage.html?lang=en#provide-the-blob-sas-token-and-storage-uri)

* [Vista previa y envío del formulario BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Compruebe que los datos están almacenados en el contenedor de almacenamiento de Azure que elija. Copie el ID de blob.

* [Vista previa del formulario BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) y especifique el ID del blob como parámetro de guid en la URL para que el formulario se rellene previamente con los datos del almacenamiento de Azure
