---
title: Generación de FPID de Adobe Experience Platform con AEM Sites
description: Obtenga información sobre cómo generar o actualizar cookies FPID de Adobe Experience Platform mediante AEM Sites.
version: Experience Manager as a Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2024-10-09T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
duration: 266
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1054'
ht-degree: 0%

---

# Generación de FPID de Experience Platform con AEM Sites

La integración de sitios de Adobe Experience Manager (AEM) entregados mediante AEM Publish con Adobe Experience Platform (AEP) requiere que AEM genere y mantenga una cookie de ID de dispositivo de origen (FPID) única para realizar un seguimiento único de la actividad del usuario.

La cookie FPID debe configurarla el servidor (AEM Publish) en lugar de utilizar JavaScript para crear una cookie del lado del cliente. Esto se debe a que los exploradores modernos, como Safari y Firefox, pueden bloquear o hacer caducar rápidamente las cookies generadas por JavaScript.

Lea la documentación de soporte técnico para [obtener más información sobre cómo funcionan juntos los ID de dispositivos de origen y los ID de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=es).

A continuación se muestra una descripción general del funcionamiento de los FPID al utilizar AEM como host web.

![FPID y ECID con AEM](./assets/aem-platform-fpid-architecture.png)

## Generar y mantener el FPID con AEM

El servicio de publicación de AEM optimiza el rendimiento almacenando en caché las solicitudes tantas como pueda, tanto en la caché de CDN como en la de AEM Dispatcher.

Las solicitudes HTTP imperativas que generan la cookie FPID única por usuario y devuelven el valor FPID nunca se almacenan en caché y se atienden directamente desde AEM Publish, que puede implementar la lógica para garantizar la exclusividad.

Evite generar la cookie FPID en solicitudes de páginas web u otros recursos almacenables en caché, ya que la combinación del requisito de exclusividad de FPID haría que estos recursos no se almacenaran en caché.

En el diagrama siguiente se describe cómo administra el servicio de publicación de AEM los FPID.

![Diagrama de flujo FPID y AEM](./assets/aem-fpid-flow.png)

1. El explorador web realiza una solicitud para una página web alojada en AEM. La solicitud se puede servir utilizando una copia en caché de la página web desde la caché de CDN o AEM Dispatcher.
1. Si la página web no se puede servir desde cachés de CDN o AEM Dispatcher, la solicitud llega al servicio AEM Publish, que genera la página web solicitada.
1. A continuación, la página web se devuelve al explorador web y se rellenan las cachés que no pudieron atender la solicitud. Con AEM, espere que las tasas de visitas de caché de CDN y AEM Dispatcher sean superiores al 90 %.
1. La página web contiene JavaScript que realiza una solicitud XHR asincrónica (AJAX) no almacenable en caché a un servlet FPID personalizado en el servicio de publicación de AEM. Como se trata de una solicitud no almacenable en caché (debido a su parámetro de consulta aleatorio y a los encabezados Cache-Control), CDN o AEM Dispatcher nunca la almacenan en caché y siempre llega al servicio AEM Publish para generar la respuesta.
1. El servlet FPID personalizado en el servicio de publicación de AEM procesa la solicitud y genera un nuevo FPID cuando no se encuentra ninguna cookie FPID existente, o amplía la vida de cualquier cookie FPID existente. El servlet también devuelve el FPID en el cuerpo de respuesta para que lo utilice JavaScript del lado del cliente. Afortunadamente, la lógica personalizada del servlet FPID es ligera, lo que evita que esta solicitud afecte al rendimiento del servicio de publicación de AEM.
1. La respuesta para la solicitud XHR vuelve al explorador con la cookie FPID y el FPID como JSON en el cuerpo de respuesta para que la utilice Platform Web SDK.

## Muestra de código

El siguiente código y configuración se pueden implementar en el servicio de publicación de AEM para crear un punto de conexión que genere o amplíe la vida de una cookie FPID existente y devuelva el FPID como JSON.

### Servlet de cookie FPID de publicación de AEM

Se debe crear un extremo HTTP de publicación de AEM para generar o ampliar una cookie FPID mediante un [servlet Sling](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ El servlet está enlazado a `/bin/aem/fpid`, ya que no se requiere autenticación para acceder a él. Si se requiere autenticación, enlace a un tipo de recurso de Sling.
+ El servlet acepta solicitudes HTTP GET. La respuesta está marcada con `Cache-Control: no-store` para evitar el almacenamiento en caché, pero este extremo también se debe solicitar usando parámetros de consulta únicos de eliminación de caché.

Cuando una solicitud HTTP llega al servlet, este comprueba si existe una cookie FPID en la solicitud:

+ Si existe una cookie FPID, amplíe la vida de la cookie y recopile su valor para escribir en la respuesta.
+ Si no existe una cookie FPID, genere una nueva cookie FPID y guarde el valor para escribir en la respuesta.

A continuación, el servlet escribe el FPID en la respuesta como un objeto JSON con el formato: `{ fpid: "<FPID VALUE>" }`.

Es importante proporcionar el FPID al cliente en el cuerpo, ya que la cookie FPID está marcada como `HttpOnly`, lo que significa que solo el servidor puede leer su valor y JavaScript del lado del cliente no. Para evitar recuperar innecesariamente el FPID en cada carga de página, también se establece una cookie `FPID_CLIENT`, que indica que el FPID se ha generado y expone el valor a JavaScript del lado del cliente para su uso.

El valor FPID se utiliza para parametrizar llamadas que utilizan Platform Web SDK.

A continuación se muestra un ejemplo de código de un extremo de servlet AEM (disponible a través de `HTTP GET /bin/aep/fpid`) que genera o actualiza una cookie FPID y devuelve el FPID como JSON.

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
    private static final String CLIENT_COOKIE_NAME = "FPID_CLIENT";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13; // 13 months
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists, get its FPID UUID so its life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the FPID value to the response, either newly generated or the extended one
        // This can be read by the Server (AEM Publish) due to HttpOnly flag.
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");

        // Also set FPID_CLIENT cookie to avoid further server-side FPID generation
        // This can be read by the client-side JavaScript to check if FPID is already generated
        // or if it needs to be requested from server (AEM Publish)
        response.addHeader("Set-Cookie",
                CLIENT_COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "Secure; " + 
                        "SameSite=Lax");

        // Avoid caching the response
        response.addHeader("Cache-Control", "no-store");

        // Return FPID in the response as JSON for client-side access
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);

        response.setContentType("application/json");
        response.getWriter().write(json.toString());
```

### Script de HTML

Se debe agregar un JavaScript personalizado del lado del cliente a la página para invocar asincrónicamente el servlet, generar o actualizar la cookie FPID y devolver el FPID en la respuesta.

Este script de JavaScript se suele añadir a la página mediante uno de los siguientes métodos:

+ [Etiquetas en Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=es)
+ [Biblioteca de cliente de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=es)

La llamada XHR al servlet FPID de AEM personalizado es rápida, aunque asíncrona, por lo que es posible para un usuario visitar una página web servida por AEM y salir antes de que se pueda completar la solicitud.
Si esto sucede, el mismo proceso se volverá a intentar en la siguiente carga de página web desde AEM.

El GET HTTP al servlet FPID de AEM (`/bin/aep/fpid`) está parametrizado con un parámetro de consulta aleatorio para garantizar que cualquier infraestructura entre el explorador y el servicio de publicación de AEM no almacene en caché la respuesta de la solicitud.
Del mismo modo, el encabezado de solicitud `Cache-Control: no-store` se agrega para admitir la evitación del almacenamiento en caché.

Tras una invocación del servlet FPID de AEM, el FPID se recupera de la respuesta JSON y [Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=es) lo utiliza para enviarlo a las API de Experience Platform.

Consulte la documentación de Experience Platform para obtener más información sobre [el uso de FPID en identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html?lang=es#identityMap)

```javascript
...
<script>
    // Wrap in anonymous function to avoid global scope pollution

    (function() {
        // Utility function to get a cookie value by name
        function getCookie(name) {
            const value = `; ${document.cookie}`;
            const parts = value.split(`; ${name}=`);
            if (parts.length === 2) return parts.pop().split(';').shift();
        }

        // Async function to handle getting the FPID via fetching from AEM, or reading an existing FPID_CLIENT cookie
        async function getFpid() {
            let fpid = getCookie('FPID_CLIENT');
            
            // If FPID can be retrieved from FPID_CLIENT then skip fetching FPID from server
            if (!fpid) {
                // Fetch FPID from the server if no FPID_CLIENT cookie value is present
                try {
                    const response = await fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, {
                        method: 'GET',
                        headers: {
                            'Cache-Control': 'no-store'
                        }
                    });
                    const data = await response.json();
                    fpid = data.fpid;
                } catch (error) {
                    console.error('Error fetching FPID:', error);
                }
            }

            console.log('My FPID is: ', fpid);
            return fpid;
        }

        // Invoke the async function to fetch or skip FPID
        const fpid = await getFpid();

        // Add the fpid to the identityMap in the Platform Web SDK
        // and/or send to AEP via AEP tags or direct AEP Web SDK calls (alloy.js)
    })();
</script>
```

### Filtro de permitidos de Dispatcher

Por último, las solicitudes HTTP GET al servlet FPID personalizado deben permitirse a través de la configuración `filter.any` de AEM Dispatcher.

Si esta configuración de Dispatcher no se implementa correctamente, las solicitudes HTTP GET a `/bin/aep/fpid` resultarán en un error 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Recursos de Experience Platform

Revise la siguiente documentación de Experience Platform para los ID de dispositivos de origen (FPID) y para administrar los datos de identidad con Platform Web SDK.

+ [Generar ID de dispositivos de origen](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=es)
+ [ID de dispositivos de origen en Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html?lang=es)
+ [Datos de identidad en Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=es)
