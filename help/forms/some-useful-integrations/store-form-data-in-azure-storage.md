---
title: Envío de formularios de tienda en Azure Storage
description: Almacenar datos de formulario en Azure Storage mediante la API de REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Almacenar envíos de formularios en Azure Storage

Este artículo muestra cómo realizar llamadas REST para almacenar datos de AEM Forms enviados en Azure Storage.
Para poder almacenar los datos de formulario enviados en el almacenamiento de Azure, se deben seguir los siguientes pasos.

## Crear cuenta de almacenamiento de Azure

[Inicie sesión en su cuenta del portal de Azure y cree una cuenta de almacenamiento](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Proporcione un nombre significativo a su cuenta de almacenamiento, haga clic en Revisar y, a continuación, haga clic en Crear. Esto crea su cuenta de almacenamiento con todos los valores predeterminados. Para el propósito de este artículo hemos nombrado nuestra cuenta de almacenamiento `aemformstutorial`.

## Crear acceso compartido

Nos haremos con la firma de acceso compartido o el método SAS de autorización para interactuar con el contenedor de almacenamiento de Azure.
En la página Cuenta de almacenamiento del portal, haga clic en el elemento de menú Firma de acceso compartido de la izquierda para abrir la nueva página de configuración de la clave de firma de acceso compartido. Asegúrese de especificar la configuración y la fecha de finalización adecuada, tal como se muestra en la captura de pantalla siguiente, y haga clic en el botón Generar SAS y cadena de conexión. Copie la URL de SAS del servicio de blob. Utilizaremos esta URL para realizar nuestras llamadas HTTP
![shared-access-keys](./assets/shared-access-signature.png)

## Crear contenedor

Lo siguiente que debemos hacer es crear un contenedor para almacenar los datos de los envíos de formularios.
En la página de cuenta de almacenamiento, haga clic en el elemento de menú Contenedores de la izquierda y cree un contenedor llamado `formssubmissions`. Asegúrese de que el nivel de acceso público está establecido en privado
![contenedor](./assets/new-container.png)

## Crear solicitud de PUT

El siguiente paso es crear una solicitud de PUT para almacenar los datos de formulario enviados en Azure Storage. Tendremos que modificar la URL de SAS del servicio de blob para incluir el nombre del contenedor y el ID de BLOB en la URL. Cada envío de formulario debe identificarse con un ID de BLOB único. El ID único de BLOB se suele crear en el código e insertar en la dirección URL de la solicitud del PUT.
La siguiente es la dirección URL parcial de la solicitud del PUT. El `aemformstutorial` es el nombre de la cuenta de Storage, formsubmissions es el contenedor en el que se almacenarán los datos con un ID de BLOB único. El resto de la URL seguirá siendo el mismo.
https://aemformstutorial.blob.core.windows.net/formsubmissions/00cb1a0c-a891-4dd7-9bd2-67a22bef3b8b?...............

La siguiente es una función escrita para almacenar los datos de formulario enviados en Azure Storage mediante una solicitud del PUT. Observe el uso del nombre de contenedor y el uuid en la dirección URL. Puede crear un servicio OSGi o un servlet sling mediante el código de ejemplo que se muestra a continuación y almacenar los envíos de formularios en Azure Storage.

```java
 public String saveFormDatainAzure(String formData) {
        System.out.println("in SaveFormData!!!!!"+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        
        String url = "https://aemformstutorial.blob.core.windows.net/formsubmissions/"+uuid.toString();
        url = url+"?sv=2022-11-02&ss=bf&srt=o&sp=rwdlaciytfx&se=2024-06-28T00:42:59Z&st=2023-06-27T16:42:59Z&spr=https&sig=v1MR%2FJuhEledioturDFRTd9e2fIDVSGJuAiUt6wNlkLA%3D";
        HttpPut httpPut = new HttpPut(url);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        try {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            log.debug("Response code "+response.getStatusLine().getStatusCode());
        } catch (IOException e) {
            log.error("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return uuid.toString();


    }
```

## Comprobar los datos almacenados en el contenedor

![form-data-in-container](./assets/form-data-in-container.png)
