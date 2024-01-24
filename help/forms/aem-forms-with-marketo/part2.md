---
title: AEM Forms con Marketo (parte 2)
description: Tutorial para integrar AEM Forms con Marketo mediante el modelo de datos de formulario de AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
duration: 172
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 1%

---

# Servicio de autenticación de Marketo

Las API de REST de Marketo están autenticadas con OAuth 2.0 de 2 patas. Necesitamos crear una autenticación personalizada para autenticarnos en Marketo. Esta autenticación personalizada suele escribirse dentro de un paquete OSGI. El siguiente código muestra el autenticador personalizado que se utilizó como parte de este tutorial.

## Servicio de autenticación personalizada

El siguiente código crea el objeto AuthenticationDetails que tiene el access_token necesario para la autenticación con Marketo

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

MarketoAuthenticationService implementa la interfaz IAuthentication. Esta interfaz forma parte del AEM Forms Client SDK. El servicio obtiene el token de acceso e inserta el token en el HttpHeader de AuthenticationDetails. Una vez rellenado el objeto HttpHeaders del objeto AuthenticationDetails, el objeto AuthenticationDetails se devuelve a la capa Dermis del modelo de datos de formulario.

Preste atención a la cadena devuelta por el método getAuthenticationType. Esta cadena se utiliza al configurar la fuente de datos.

### Obtener token de acceso

Una interfaz simple se define con un método que devuelve access_token. El código de la clase que implementa esta interfaz se muestra más adelante en la página.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

El siguiente código es del servicio que devuelve el access_token que se va a usar en hacer las llamadas a la API de REST. El código de este servicio accede a los parámetros de configuración necesarios para realizar la llamada de GET. Como puede ver, pasamos client_id,client_secret en la URL de GET para generar access_token. A continuación, se devuelve este access_token a la aplicación que realiza la llamada.

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

La captura de pantalla siguiente muestra las propiedades de configuración que deben establecerse. Estas propiedades de configuración se leen en el código mencionado anteriormente para obtener access_token

![config](assets/configuration-settings.png)

### Configuración

El siguiente código se utilizó para crear las propiedades de configuración. Estas propiedades son específicas de la instancia de Marketo

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

El siguiente código lee las propiedades de configuración y devuelve las mismas a través de los métodos de captador

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. AEM Cree e implemente el paquete en el servidor de.
1. [Dirija su explorador a configMgr](http://localhost:4502/system/console/configMgr) y busque &quot;Configuración del servicio de credenciales de Marketo&quot;
1. Especifique las propiedades adecuadas específicas de la instancia de Marketo

## Pasos siguientes

[Crear una fuente de datos basada en el servicio RESTful](./part3.md)
