---
title: Generación de FPID de Adobe Experience Platform con AEM Sites
description: Obtenga información sobre cómo generar o actualizar cookies FPID de Adobe Experience Platform mediante AEM Sites.
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '1040'
ht-degree: 0%

---

# Generación de FPID de Experience Platform con AEM Sites

La integración de sitios de Adobe Experience Manager AEM () con Adobe Experience Platform AEM (AEP) requiere generar, y mantener una cookie de ID de dispositivo de origen (FPID) única, para realizar un seguimiento único de la actividad del usuario.

Lea la documentación de apoyo en [obtenga información acerca de los detalles de cómo funcionan juntos los ID de dispositivo de origen y los ID de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

AEM A continuación se muestra una descripción general del funcionamiento de los FPID al utilizar a los usuarios como host web, en su lugar de trabajo.

![AEM FPID y ECID con la función de](./assets/aem-platform-fpid-architecture.png)

## AEM Generar y mantener el FPID con la función de

AEM El servicio de publicación de AEM optimiza el rendimiento almacenando en caché las solicitudes tantas como pueda, tanto en las cachés de CDN como en las de Dispatcher de.

Las solicitudes HTTP imperativas que generan la cookie FPID única por usuario y devuelven el valor FPID nunca se almacenan en caché y se atienden directamente desde AEM Publish, que puede implementar la lógica para garantizar la exclusividad.

Evite generar la cookie FPID en solicitudes de páginas web u otros recursos almacenables en caché, ya que la combinación del requisito de exclusividad de FPID haría que estos recursos no se almacenaran en caché.

En el diagrama siguiente se describe cómo administra el servicio de publicación de AEM los FPID.

![AEM Diagrama de flujo de FPID y](./assets/aem-fpid-flow.png)

1. AEM El explorador web realiza una solicitud para una página web alojada por el servidor de correo de. AEM La solicitud se puede servir utilizando una copia en caché de la página web desde CDN o la caché de Dispatcher de la red de distribución de contenido (CDN) o la red de distribución de datos (Dispatcher.
1. AEM Si la página web no se puede servir desde las cachés de CDN o Dispatcher, la solicitud llega al servicio de publicación de AEM, que genera la página web solicitada.
1. A continuación, la página web se devuelve al explorador web y se rellenan las cachés que no pudieron atender la solicitud. AEM AEM Con, espere que las tasas de aciertos de caché de CDN y Dispatcher de sean buenas en comparación con el 90 %.
1. AJAX La página web contiene JavaScript que realiza una solicitud XHR asíncrona no almacenable en caché () a un servlet FPID personalizado en el servicio de publicación de AEM. AEM Como se trata de una solicitud no almacenable en caché (debido a su parámetro de consulta aleatorio y a los encabezados Cache-Control), CDN o Dispatcher nunca la almacenan en caché y siempre llega al servicio de publicación de AEM para generar la respuesta.
1. El servlet FPID personalizado en el servicio de publicación de AEM procesa la solicitud y genera un nuevo FPID cuando no se encuentra ninguna cookie FPID existente, o amplía la vida de cualquier cookie FPID existente. El servlet también devuelve el FPID en el cuerpo de respuesta para que lo utilice JavaScript del lado del cliente. Afortunadamente, la lógica personalizada del servlet FPID es ligera, lo que evita que esta solicitud afecte al rendimiento del servicio de publicación de AEM.
1. La respuesta para la solicitud XHR vuelve al explorador con la cookie FPID y el FPID como JSON en el cuerpo de respuesta para que la utilice el SDK web de Platform.

## Muestra de código

El siguiente código y configuración se pueden implementar en el servicio de publicación de AEM para crear un punto de conexión que genere o amplíe la vida de una cookie FPID existente y devuelva el FPID como JSON.

### AEM Servlet de cookie FPID

AEM Se debe crear un extremo HTTP de la para generar o ampliar una cookie FPID mediante un [Sling servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ El servlet está enlazado a `/bin/aem/fpid` ya que no se requiere autenticación para acceder a él. Si se requiere autenticación, enlace a un tipo de recurso de Sling.
+ El servlet acepta solicitudes de GET HTTP. La respuesta está marcada con `Cache-Control: no-store` para evitar el almacenamiento en caché, pero este extremo también debe solicitarse utilizando parámetros de consulta únicos de eliminación de caché.

Cuando una solicitud HTTP llega al servlet, este comprueba si existe una cookie FPID en la solicitud:

+ Si existe una cookie FPID, amplíe la vida de la cookie y recopile su valor para escribir en la respuesta.
+ Si no existe una cookie FPID, genere una nueva cookie FPID y guarde el valor para escribir en la respuesta.

A continuación, el servlet escribe el FPID en la respuesta como un objeto JSON con el formato: `{ fpid: "<FPID VALUE>" }`.

Es importante proporcionar el FPID al cliente en el cuerpo, ya que la cookie FPID está marcada `HttpOnly`, lo que significa que solo el servidor puede leer su valor y JavaScript del lado del cliente no.

El valor FPID del cuerpo de respuesta se utiliza para parametrizar llamadas mediante el SDK web de Platform.

AEM A continuación se muestra un ejemplo de código de un extremo de servlet de (disponible a través de `HTTP GET /bin/aep/fpid`) que genera o actualiza una cookie FPID y devuelve el FPID como JSON.

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13;
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, Create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists. get its FPID UUID so it's life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the newly generate FPID value, or the extended FPID value to the response
        // Use addHeader(..), as we need to set SameSite=Lax (and addCoookie(..) does not support this)
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");
        
        // Avoid caching the response in any cache
        response.addHeader("Cache-Control", "no-store");

        // Since the FPID is HttpOnly, JavaScript cannot read it (only the server can)
        // Write the FPID to the response as JSON so client JavaScript can access it.
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);
        
        // The JSON `{ fpid: "11111111-2222-3333-4444-55555555" }` is returned in the response
        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}
```

### Script de HTML

Se debe agregar a la página un JavaScript personalizado del lado del cliente para invocar asincrónicamente el servlet, generar o actualizar la cookie FPID y devolver el FPID en la respuesta.

Este script JavaScript generalmente se añade a la página mediante uno de los siguientes métodos:

+ [Etiquetas en Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [AEM Biblioteca de cliente de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

AEM AEM La llamada XHR al servlet personalizado de FPID es rápida, aunque asíncrona, por lo que es posible para un usuario visitar una página web servida por el usuario y navegar fuera antes de que la solicitud se pueda completar.
AEM Si esto ocurre, el mismo proceso volverá a intentar la carga de página siguiente de una página web desde el punto de vista de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de.

La GET AEM HTTP al servlet FPID de la (`/bin/aep/fpid`) tiene un parámetro de consulta aleatorio para garantizar que ninguna infraestructura entre el explorador y el servicio de publicación de AEM almacene en caché la respuesta de la solicitud.
Del mismo modo, la variable `Cache-Control: no-store` Se ha agregado un encabezado de solicitud para admitir la evitación del almacenamiento en caché.

AEM Tras una invocación del servlet FPID de la, el FPID se recupera de la respuesta JSON y lo utiliza el [SDK web de Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) para enviarlo a las API de Experience Platform.

Consulte la documentación del Experience Platform para obtener más información sobre [uso de FPID en identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Invoke the AEM FPID servlet, and then do something with the response

    fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, { 
            method: 'GET',
            headers: {
                'Cache-Control': 'no-store'
            }
        })
        .then((response) => response.json())
        .then((data) => { 
            // Get the FPID from JSON returned by AEM's FPID servlet
            console.log('My FPID is: ' + data.fpid);

            // Send the `data.fpid` to Experience Platform APIs            
        });
</script>
```

### Filtro de permitidos de Dispatcher

Por último, las solicitudes de GET AEM HTTP al servlet FPID personalizado deben permitirse a través de la instancia de Dispatcher de Dispatcher de `filter.any` configuración.

Si esta configuración de Dispatcher no se implementa correctamente, la GET HTTP solicita a `/bin/aep/fpid` da como resultado un 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## recursos de Experience Platform

Revise la siguiente documentación del Experience Platform para los ID de dispositivos de origen (FPID) y para administrar los datos de identidad con el SDK web de Platform.

+ [Generar ID de dispositivos de origen](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [ID de dispositivos de origen en el SDK web de Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Datos de identidad en el SDK web de Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
