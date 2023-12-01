---
title: Envío de formularios de tienda en Azure Storage
description: Almacenar datos de formulario en Azure Storage mediante la API de REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
jira: KT-13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# Almacenar envíos de formularios en Azure Storage

Este artículo muestra cómo realizar llamadas REST para almacenar datos de AEM Forms enviados en Azure Storage.
Para poder almacenar los datos de formulario enviados en el almacenamiento de Azure, se deben seguir los siguientes pasos.

## Crear cuenta de almacenamiento de Azure

[Inicie sesión en su cuenta del portal de Azure y cree una cuenta de almacenamiento](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Proporcione un nombre significativo a su cuenta de almacenamiento, haga clic en Revisar y, a continuación, haga clic en Crear. Esto crea su cuenta de almacenamiento con todos los valores predeterminados. Para el propósito de este artículo hemos nombrado nuestra cuenta de almacenamiento `aemformstutorial`.


## Crear contenedor

Lo siguiente que debemos hacer es crear un contenedor para almacenar los datos de los envíos de formularios.
En la página de cuenta de almacenamiento, haga clic en el elemento de menú Contenedores de la izquierda y cree un contenedor llamado `formssubmissions`. Asegúrese de que el nivel de acceso público está establecido en privado
![contenedor](./assets/new-container.png)

## Crear SAS en el contenedor

Nos haremos con la firma de acceso compartido o el método SAS de autorización para interactuar con el contenedor de almacenamiento de Azure.
Vaya al contenedor en la cuenta de almacenamiento, haga clic en los puntos suspensivos y seleccione la opción Generate SAS como se muestra en la captura de pantalla
![sas-on-container](./assets/sas-on-container.png)
Asegúrese de especificar los permisos adecuados y la fecha de finalización adecuada, tal como se muestra en la captura de pantalla siguiente, y haga clic en Generar token SAS y URL. Copie el token SAS de blob y la URL de SAS de blob. Utilizaremos estos dos valores para realizar nuestras llamadas HTTP
![shared-access-keys](./assets/shared-access-signature.png)


## Proporcione el token SAS de blob y el URI de almacenamiento.

Para que el código sea más genérico, las dos propiedades se pueden configurar utilizando la configuración OSGi como se muestra a continuación. El _**aemformstutorial**_ es el nombre de la cuenta de almacenamiento, _**formsubmissions**_ es el contenedor en el que se almacenan los datos.
![osgi-configuration](./assets/azure-portal-osgi-configuration.png)


## Crear solicitud de PUT

El siguiente paso es crear una solicitud de PUT para almacenar los datos de formulario enviados en Azure Storage. Cada envío de formulario debe identificarse con un ID de BLOB único. El ID único de BLOB se suele crear en el código e insertar en la dirección URL de la solicitud del PUT.
La siguiente es la dirección URL parcial de la solicitud del PUT. El `aemformstutorial` es el nombre de la cuenta de Storage, formsubmissions es el contenedor en el que se almacenarán los datos con un ID de BLOB único. El resto de la URL seguirá siendo el mismo.
https://aemformstutorial.blob.core.windows.net/formsubmissions/blobid/sastoken La siguiente es una función escrita para almacenar los datos de formulario enviados en Azure Storage mediante una solicitud del PUT. Observe el uso del nombre de contenedor y el uuid en la dirección URL. Puede crear un servicio OSGi o un servlet sling mediante el código de ejemplo que se muestra a continuación y almacenar los envíos de formularios en Azure Storage.

```java
 public String saveFormDatainAzure(String formData) {
    log.debug("in SaveFormData!!!!!" + formData);
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    UUID uuid = UUID.randomUUID();
    String putRequestURL = storageURI + uuid.toString();
    putRequestURL = putRequestURL + sasToken;
    HttpPut httpPut = new HttpPut(putRequestURL);
    httpPut.addHeader("x-ms-blob-type", "BlockBlob");
    httpPut.addHeader("Content-Type", "text/plain");

    try {
        httpPut.setEntity(new StringEntity(formData));

        CloseableHttpResponse response = httpClient.execute(httpPut);
        log.debug("Response code " + response.getStatusLine().getStatusCode());
        if (response.getStatusLine().getStatusCode() == 201) {
            return uuid.toString();
        }
    } catch (IOException e) {
        log.error("Error: " + e.getMessage());
        throw new RuntimeException(e);
    }
    return null;

}
```

## Comprobar los datos almacenados en el contenedor

![form-data-in-container](./assets/form-data-in-container.png)

## Prueba de la solución

* [Implementar el paquete OSGi personalizado](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importe la plantilla de formulario adaptable personalizada y el componente de página asociado a la plantilla](./assets/store-and-fetch-from-azure.zip)

* [Importar el formulario adaptable de ejemplo](./assets/bank-account-sample-form.zip)

* Especifique los valores adecuados en la configuración de Azure Portal mediante la consola de configuración OSGi
* [Vista previa y envío del formulario BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Compruebe que los datos están almacenados en el contenedor de almacenamiento de Azure que elija. Copie el ID de blob.
* [Vista previa del formulario BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) y especifique el ID del blob como parámetro de guid en la URL para que el formulario se rellene previamente con los datos del almacenamiento de Azure

