---
title: Creación De Un Perfil De Campaign Con El Modelo De Datos De Formulario
description: Pasos necesarios para crear un perfil de Adobe Campaign Standard mediante el modelo de datos de formulario de AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
duration: 126
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 3%

---

# Creación De Un Perfil De Campaign Con El Modelo De Datos De Formulario {#create-campaign-profile-using-form-data-model}

Pasos necesarios para crear un perfil de Adobe Campaign Standard mediante el modelo de datos de formulario de AEM Forms

## Crear autenticación personalizada {#create-custom-authentication}

Al crear una fuente de datos con el archivo swagger, AEM Forms admite los siguientes tipos de autenticación

* Ninguno
* OAuth 2.0
* Autenticación básica
* Clave de API
* Autenticación personalizada

![campaingfdm](assets/campaignfdm.gif)

Tendremos que utilizar la autenticación personalizada para hacer llamadas REST a Adobe Campaign Standard.

Para utilizar la autenticación personalizada, tendremos que desarrollar un componente OSGi que implemente la interfaz IAuthentication

Se debe implementar el método getAuthDetails. Este método devolverá el objeto AuthenticationDetails. Este objeto AuthenticationDetails tendrá los encabezados HTTP necesarios para realizar la llamada de la API de REST a Adobe Campaign.

El siguiente es el código que se utilizó para crear la autenticación personalizada. El método getAuthDetails hace todo el trabajo. Creamos el objeto AuthenticationDetails. A continuación, agregamos los HttpHeaders adecuados a este objeto y devolvemos este objeto.

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

El primer paso es crear el archivo swagger. El archivo swagger define la API de REST que se utilizará para crear un perfil en Adobe Campaign Standard. El archivo swagger define los parámetros de entrada y los parámetros de salida de la API de REST.

Se crea una fuente de datos con el archivo swagger. Al crear la fuente de datos, puede especificar el tipo de autenticación. En este caso, utilizaremos la autenticación personalizada para autenticarnos con Adobe Campaign. El código mencionado anteriormente se utilizó para autenticarnos con Adobe Campaign.

Se le proporciona un archivo swagger de muestra como parte de los recursos relacionados con este artículo.**Asegúrese de cambiar el host y la basePath en el archivo swagger para que coincida con su instancia de ACS**

## Prueba de la solución {#test-the-solution}

Para probar la solución, siga estos pasos:
* [Asegúrese de haber seguido los pasos descritos aquí](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Descargue y descomprima este archivo para obtener el archivo swagger](assets/create-acs-profile-swagger-file.zip)
* Crear fuente de datos con el archivo swagger Crear modelo de datos de formulario y basarlo en la fuente de datos creada en el paso anterior
* Cree un formulario adaptable basado en el modelo de datos de formulario creado en el paso anterior.
* Arrastre y suelte los siguientes elementos de la pestaña fuentes de datos en el formulario adaptable

   * Correo electrónico
   * Nombre
   * Apellidos
   * Teléfono móvil

* Configure la acción de envío a &quot;Enviar mediante el modelo de datos de formulario&quot;.
* Configure el modelo de datos para que se envíe correctamente.
* Previsualice el formulario. Rellene los campos y envíe.
* Compruebe que el perfil se ha creado en Adobe Campaign Standard.
