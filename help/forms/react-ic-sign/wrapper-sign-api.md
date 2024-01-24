---
title: Crear un formulario web para presentarlo al usuario para su firma
description: AEM Cree un paquete de informes para exponer los métodos de Acrobat Sign necesarios para el caso de uso.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 15364571-070c-4497-a256-f0483d6f9585
duration: 155
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 0%

---

# Creación de un contenedor para la API de REST de Acrobat Sign

AEM Se ha desarrollado un paquete de personalizado para crear y devolver el formulario web al usuario final

* [Crear documento transitorio](https://secure.na1.echosign.com/public/docs/restapi/v6#!/transientDocuments/createTransientDocument). El documento cargado a través de esta llamada se denomina transitorio, ya que solo está disponible durante 7 días después de la carga. El ID de documento transitorio devuelto se puede utilizar en las llamadas a la API donde se debe hacer referencia al archivo cargado. La solicitud de documento transitoria es una solicitud de varias partes que consta de tres partes: nombre de archivo, tipo MIME y el flujo de archivos. En esta solicitud, solo se puede cargar un archivo a la vez.
* [Creación de un formulario web](https://secure.na1.echosign.com/public/docs/restapi/v6#!/widgets/createWidget).Este es un punto final principal que se utiliza para crear un nuevo formulario web. El formulario web se creó en un estado ACTIVO para alojar inmediatamente el formulario web.
* [Recuperación del formulario web](https://secure.na1.echosign.com/public/docs/restapi/v6#!/widgets/getWidgets).Recupere el formulario web del usuario. A continuación, este formulario web se presenta a la aplicación que realiza la llamada para firmar el documento.

## Crear la configuración OSGi de Acrobat Sign

La API de REST de Acrobat Sign requiere la clave de integración y el correo electrónico asociados a ella. Estos dos valores se proporcionan como propiedades de configuración OSGi, como se muestra a continuación

![configuración de firma](assets/sign-configuration.png)

```java
package com.acrobatsign.core.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;



@ObjectClassDefinition(name="Acrobat Sign Configuration", description = "Acrobat SignConfiguration")
public @interface AcrobatSignConfiguration
{
    @AttributeDefinition(name="Acrobat Sign Integration Key", description = "Integration key you created in Acrobat Sign ")
        String getIntegrationKey();
    
    @AttributeDefinition(name="X-API-USER", description = "X-API-USER")
    String getApiUser();


}
```

```java
package com.acrobatsign.core.configuration;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(immediate = true, service = AcrobatSignConfigurationService.class)
@Designate(ocd = AcrobatSignConfiguration.class)
public class AcrobatSignConfigurationService {

  private String IntegrationKey;
  private String apiUserEmail;
  public String getIntegrationKey() {
    return IntegrationKey;
  }
  public String getApiUserEmail() {
    return apiUserEmail;
  }

  @Activate
  protected final void activate(AcrobatSignConfiguration config) {
    IntegrationKey = config.getIntegrationKey();

    apiUserEmail = config.getApiUser();
  }

}
```

## Obtener ID de documento transitorio

El siguiente código se escribió para crear un documento transitorio

```java
public String getTransientDocumentID(Document documentForSigning) throws IOException {
  String integrationKey = acrobatSignConfig.getIntegrationKey();
  String apiUser = acrobatSignConfig.getApiUserEmail();
  String url = "https://api.na1.adobesign.com/api/rest/v6/transientDocuments";
  MultipartEntityBuilder builder = MultipartEntityBuilder.create();
  org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
  HttpPost httpPost = new HttpPost(url);
  httpPost.addHeader("x-api-user", "email:" + apiUser);
  httpPost.addHeader("Authorization", "Bearer " + integrationKey);
  builder.addBinaryBody("File", documentForSigning.getInputStream(), ContentType.DEFAULT_BINARY, "NDA.PDF");
  builder.addTextBody("File-Name", "NDA.pdf");
  HttpEntity entity = builder.build();
  log.debug("Build the entity");
  httpPost.setEntity(entity);
  CloseableHttpResponse response = httpClient.execute(httpPost);
  log.debug("Sent the request!!!!");
  log.debug("REsponse code " + response.getStatusLine().getStatusCode());
  HttpEntity httpEntity = response.getEntity();
  String transientDocumentId = JsonParser.parseString(EntityUtils.toString(httpEntity)).getAsJsonObject().get("transientDocumentId").getAsString();
  log.debug("Transient ID  " + transientDocumentId);
  return transientDocumentId;

}
```

## Obtener ID del widget

```java
public String getWidgetID(String transientDocumentID) {
  String integrationKey = acrobatSignConfig.getIntegrationKey();
  String apiUser = acrobatSignConfig.getApiUserEmail();
  String url = "https://api.na1.adobesign.com:443/api/rest/v6/widgets";
  org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
  HttpPost httpPost = new HttpPost(url);
  httpPost.addHeader("x-api-user", "email:" + apiUser);
  httpPost.addHeader("Authorization", "Bearer " + integrationKey);

  String jsonREquest = "{\r\n" +
    "  \"fileInfos\": [\r\n" +
    "    {\r\n" +
    "      \"transientDocumentId\": \"a\"\r\n" +
    "    }\r\n" +
    "  ],\r\n" +
    "  \"name\": \"Release and Waiver Agreement\",\r\n" +
    "  \"state\": \"ACTIVE\",\r\n" +
    "  \"widgetParticipantSetInfo\": {\r\n" +
    "    \"memberInfos\": [\r\n" +
    "      {\r\n" +
    "        \"email\": \"\"\r\n" +
    "      }\r\n" +
    "    ],\r\n" +
    "    \"role\": \"SIGNER\"\r\n" +
    "  }\r\n" +
    "}";
  JsonObject jsonReq = JsonParser.parseString(jsonREquest).getAsJsonObject();
  jsonReq.getAsJsonArray("fileInfos").get(0).getAsJsonObject().addProperty("transientDocumentId", transientDocumentID);
  log.debug("The updated json object is " + jsonReq);
  try {
    StringEntity stringEntity = new StringEntity(jsonReq.toString());
    httpPost.setEntity(stringEntity);
    httpPost.addHeader("Content-Type", "application/json");
    CloseableHttpResponse response = httpClient.execute(httpPost);
    log.debug("The response is  " + response.getStatusLine().getStatusCode());
    String widgetID = JsonParser.parseString(EntityUtils.toString(response.getEntity())).getAsJsonObject().get("id").getAsString();
    log.debug("The widget id is " + widgetID);
    return widgetID;
  } catch (Exception e) {
    log.debug("Error in getting Widget ID:" + e.getMessage());

  }
  return null;
}
```

## Obtener URL del widget

```java
public String getWidgetURL(String widgetId) throws ClientProtocolException, IOException {
        
        log.debug("$$$$ in get Widget URL for "+widgetId+ "widget id");
        String url = "https://api.na1.adobesign.com:443/api/rest/v6/widgets";
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        HttpGet httpGet = new HttpGet(url);
        httpGet.addHeader("x-api-user", "email:"+acrobatSignConfig.getApiUserEmail());
        httpGet.addHeader("Authorization", "Bearer "+acrobatSignConfig.getIntegrationKey());
        CloseableHttpResponse response = httpClient.execute(httpGet);
        JsonObject jsonResponse = JsonParser.parseString(EntityUtils.toString(response.getEntity())).getAsJsonObject();
         log.debug("The json response from get widgets is "+jsonResponse.toString());
         JsonArray userWidgetList = jsonResponse.get("userWidgetList").getAsJsonArray();
         log.debug("The array size is "+userWidgetList.size());
          for(int i=0;i<userWidgetList.size();i++)
         {
        
             log.debug("Getting widget object "+i);
             JsonObject temp = userWidgetList.get(i).getAsJsonObject();
             log.debug("The widget object "+i+"is "+temp.toString());
             if(temp.get("id").getAsString().equalsIgnoreCase(widgetId))
             {
                 log.debug("Bingo found the matching widget id  "+i);
                 String widgetURL = temp.get("url").getAsString();
                 return widgetURL;
                 
             }
            
         }
        
        return null;
        
    }
```

## Pasos siguientes

[Generar URL del widget de Acrobat Sign](./create-servlet-to-expose-endpoint.md)
