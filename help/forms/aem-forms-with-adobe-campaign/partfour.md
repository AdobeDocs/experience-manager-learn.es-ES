---
title: Crear Perfil de Campaña con el modelo de datos de formulario
seo-title: Crear Perfil de Campaña con el modelo de datos de formulario
description: Pasos relacionados con la creación de Adobe Campaign Standard perfil mediante el modelo de datos de formulario AEM Forms
seo-description: Pasos relacionados con la creación de Adobe Campaign Standard perfil mediante el modelo de datos de formulario AEM Forms
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 3%

---


# Crear Perfil de Campaña usando el modelo de datos de formulario {#create-campaign-profile-using-form-data-model}

Pasos relacionados con la creación de Adobe Campaign Standard perfil mediante el modelo de datos de formulario AEM Forms

## Crear autenticación personalizada {#create-custom-authentication}

Al crear la fuente de datos con el archivo swagger, AEM Forms admite los siguientes tipos de autenticación

* Ninguna
* OAuth 2.0
* Autenticación básica
* Clave de API
* Autenticación personalizada

![campaignFdm](assets/campaignfdm.gif)

Tendremos que utilizar la autenticación personalizada para realizar llamadas REST a Adobe Campaign Standard.

Para utilizar la autenticación personalizada, tendremos que desarrollar un componente OSGi que implemente la interfaz IAuthauthentication

Se debe implementar el método getAuthDetails. Este método devolverá el objeto AuthenticationDetails. Este objeto AuthenticationDetails tendrá los encabezados HTTP necesarios establecidos para realizar la llamada de API REST a Adobe Campaign.

El siguiente es el código que se utilizó para crear la autenticación personalizada. El método getAuthDetails realiza todo el trabajo. Se crea el objeto AuthenticationDetails. Luego agregamos los HttpHeaders correspondientes a este objeto y devolvemos este objeto.

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## Crear fuente de datos {#create-data-source}

El primer paso es crear el archivo swagger. El archivo swagger define la API REST que se va a utilizar para crear un perfil en Adobe Campaign Standard. El archivo swagger define los parámetros de entrada y salida de la API REST.

Se crea una fuente de datos con el archivo swagger. Al crear la fuente de datos, puede especificar el tipo de autenticación. En este caso, vamos a utilizar la autenticación personalizada para autenticarnos con Adobe Campaign.El código que se muestra arriba se utilizó para autenticarnos con Adobe Campaign.

El archivo de swagger de muestra se le proporciona como parte del elemento relacionado con este artículo.**Asegúrese de cambiar el host y basePath en el archivo swagger para que coincidan con la instancia de ACS**

## Probar la solución {#test-the-solution}

Para probar la solución, siga los siguientes pasos:
* [Asegúrese de que ha seguido los pasos descritos aquí](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Descargue y descomprima este archivo para obtener el archivo swagger](assets/create-acs-profile-swagger-file.zip)
* Crear fuente de datos mediante el archivo swagger
Crear modelo de datos de formulario y basarlo en la fuente de datos creada en el paso anterior
* Cree un formulario adaptable basado en el modelo de datos de formulario creado en el paso anterior.
* Arrastre y suelte los siguientes elementos de la ficha orígenes de datos en el formulario adaptable

   * Correo electrónico
   * Nombre
   * Apellidos
   * Teléfono móvil

* Configure la acción de envío en &quot;Enviar mediante el modelo de datos de formulario&quot;.
* Configure el modelo de datos para que se envíe correctamente.
* Obtener una vista previa del formulario. Rellene los campos y envíe.
* Verifique que el perfil se haya creado en Adobe Campaign Standard.
