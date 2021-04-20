---
title: Creación de un perfil de campaña mediante el modelo de datos de formulario
seo-title: Creación de un perfil de campaña mediante el modelo de datos de formulario
description: Pasos para crear un perfil de Adobe Campaign Standard mediante el modelo de datos de formulario de AEM Forms
seo-description: Pasos para crear un perfil de Adobe Campaign Standard mediante el modelo de datos de formulario de AEM Forms
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: Output Service
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 4%

---


# Crear perfil de campaña utilizando el modelo de datos de formulario {#create-campaign-profile-using-form-data-model}

Pasos para crear un perfil de Adobe Campaign Standard mediante el modelo de datos de formulario de AEM Forms

## Crear autenticación personalizada {#create-custom-authentication}

Al crear una fuente de datos con el archivo de intercambio, AEM Forms admite los siguientes tipos de autenticación

* Ninguna
* OAuth 2.0
* Autenticación básica
* Clave de API
* Autenticación personalizada

![campaignfdm](assets/campaignfdm.gif)

Tendremos que utilizar la autenticación personalizada para realizar llamadas de REST a Adobe Campaign Standard.

Para utilizar la autenticación personalizada, tendremos que desarrollar un componente OSGi que implemente la interfaz IAuthauthentication

Se debe implementar el método getAuthDetails . Este método devolverá el objeto AuthenticationDetails. Este objeto AuthenticationDetails tendrá los encabezados HTTP requeridos establecidos que son necesarios para realizar la llamada de API REST a Adobe Campaign.

El siguiente es el código que se utilizó para crear autenticación personalizada. El método getAuthDetails hace todo el trabajo. Creamos el objeto AuthenticationDetails . A continuación, agregamos los HttpHeaders adecuados a este objeto y devolveremos este objeto.

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

El primer paso es crear el archivo swagger. El archivo de intercambio define la API de REST que se utilizará para crear un perfil en Adobe Campaign Standard. El archivo swagger define los parámetros de entrada y los parámetros de salida de la API de REST.

Se crea una fuente de datos mediante el archivo de intercambio. Al crear fuentes de datos, puede especificar el tipo de autenticación. En este caso, utilizaremos la autenticación personalizada para autenticarnos con Adobe Campaign. El código mencionado anteriormente se utilizó para autenticarnos con Adobe Campaign.

El archivo de intercambio de muestra se le da como parte del archivo del recurso relacionado con este artículo.**Asegúrese de cambiar el host y basePath en el archivo swagger para que coincidan con su instancia ACS**

## Probar la solución {#test-the-solution}

Para probar la solución, siga los siguientes pasos:
* [Asegúrese de haber seguido los pasos descritos aquí](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Descargue y descomprima este archivo para obtener el archivo swagger](assets/create-acs-profile-swagger-file.zip)
* Creación de una fuente de datos mediante el archivo de intercambio
Crear modelo de datos de formulario y basarlo en el origen de datos creado en el paso anterior
* Cree un formulario adaptable basado en el modelo de datos de formulario creado en el paso anterior.
* Arrastre y suelte los siguientes elementos de la ficha orígenes de datos en el formulario adaptable

   * Correo electrónico
   * Nombre
   * Apellidos
   * Teléfono móvil

* Configure la acción de envío en &quot;Enviar mediante el modelo de datos de formulario&quot;.
* Configure el modelo de datos para que se envíe correctamente.
* Obtener una vista previa del formulario. Complete los campos y envíe.
* Verifique que el perfil se haya creado en Adobe Campaign Standard.
