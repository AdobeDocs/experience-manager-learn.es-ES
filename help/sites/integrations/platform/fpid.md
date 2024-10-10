---
title: Generación de FPID de Adobe Experience Platform con AEM Sites
description: Obtenga información sobre cómo generar o actualizar cookies FPID de Adobe Experience Platform mediante AEM Sites.
version: Cloud Service
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
source-git-commit: c638c1e012952f2f43806a325d729cde088ab9f5
workflow-type: tm+mt
source-wordcount: '1015'
ht-degree: 0%

---

# Generación de FPID de Experience Platform con AEM Sites

La integración de sitios de Adobe Experience Manager AEM AEM () entregados a través de Publish, con Adobe Experience Platform AEM (AEP) requiere que los usuarios generen y mantengan una cookie de ID de dispositivo de origen (FPID) única, con el fin de realizar un seguimiento único de la actividad del usuario.

Lea la documentación de soporte técnico para [obtener más información acerca de cómo funcionan juntos los identificadores de dispositivos de origen y los identificadores de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

AEM A continuación se muestra una descripción general del funcionamiento de los FPID al utilizar a los usuarios como host web, en su lugar de trabajo.

AEM ![FPID y ECID con {1000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000](./assets/aem-platform-fpid-architecture.png)

## AEM Generar y mantener el FPID con la función de

AEM El servicio de Publish AEM optimiza el rendimiento almacenando en caché las solicitudes tantas como pueda, tanto en las cachés de CDN como en las de Dispatcher de la red de distribución de contenido (CDN) de la red de distribución de contenido (CDN) de la red ().

AEM Las solicitudes HTTP imperativas que generan la cookie FPID única por usuario y devuelven el valor FPID nunca se almacenan en caché y se atienden directamente desde Publish, que puede implementar la lógica para garantizar la exclusividad.

Evite generar la cookie FPID en solicitudes de páginas web u otros recursos almacenables en caché, ya que la combinación del requisito de exclusividad de FPID haría que estos recursos no se almacenaran en caché.

AEM En el diagrama siguiente se describe cómo gestiona el servicio de Publish los FPIDs.

AEM ![Diagrama de flujo de FPID y de](./assets/aem-fpid-flow.png)

1. AEM El explorador web realiza una solicitud para una página web alojada por el servidor de correo de. AEM La solicitud se puede servir utilizando una copia en caché de la página web desde CDN o caché de Dispatcher de la.
1. AEM Si la página web no se puede servir desde las cachés de CDN o Dispatcher AEM, la solicitud llega al servicio de Publish, que es quien genera la página web solicitada.
1. A continuación, la página web se devuelve al explorador web y se rellenan las cachés que no pudieron atender la solicitud. AEM AEM Con, espere que las tasas de aciertos de caché de CDN y Dispatcher de la sean superiores al 90 %.
1. La página web contiene JavaScript AJAX AEM que realiza una solicitud XHR asíncrona (XHR) no almacenable en caché a un servlet FPID personalizado en el servicio de Publish de. AEM Dado que se trata de una solicitud no almacenable en caché (debido a su parámetro de consulta aleatorio y a los encabezados Cache-Control), CDN o Dispatcher AEM nunca la almacenan en caché, y siempre llega al servicio de Publish para generar la respuesta.
1. AEM El servlet FPID personalizado en el servicio de Publish procesa la solicitud y genera un nuevo FPID cuando no se encuentra ninguna cookie FPID existente, o amplía la vida de cualquier cookie FPID existente. El servlet también devuelve el FPID en el cuerpo de respuesta para que lo utilice JavaScript del lado del cliente. AEM Afortunadamente, la lógica personalizada del servlet FPID es ligera, lo que evita que esta solicitud afecte al rendimiento del servicio de Publish de la manera más eficiente y más eficaz.
1. La respuesta para la solicitud XHR vuelve al explorador con la cookie FPID y el FPID como JSON en el cuerpo de respuesta para que la utilice el SDK web de Platform.

## Muestra de código

AEM El siguiente código y configuración se pueden implementar en el servicio de Publish para crear un punto de conexión que genere o extienda la vida de una cookie FPID existente y devuelva el FPID como JSON.

### AEM Servlet de cookie FPID de Publish

AEM Se debe crear un extremo HTTP de Publish para generar o extender una cookie FPID, usando un [servlet Sling](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ El servlet está enlazado a `/bin/aem/fpid`, ya que no se requiere autenticación para acceder a él. Si se requiere autenticación, enlace a un tipo de recurso de Sling.
+ El servlet acepta solicitudes de GET HTTP. La respuesta está marcada con `Cache-Control: no-store` para evitar el almacenamiento en caché, pero este extremo también se debe solicitar usando parámetros de consulta únicos de eliminación de caché.

Cuando una solicitud HTTP llega al servlet, este comprueba si existe una cookie FPID en la solicitud:

+ Si existe una cookie FPID, amplíe la vida de la cookie y recopile su valor para escribir en la respuesta.
+ Si no existe una cookie FPID, genere una nueva cookie FPID y guarde el valor para escribir en la respuesta.

A continuación, el servlet escribe el FPID en la respuesta como un objeto JSON con el formato: `{ fpid: "<FPID VALUE>" }`.

Es importante proporcionar el FPID al cliente en el cuerpo, ya que la cookie FPID está marcada como `HttpOnly`, lo que significa que solo el servidor puede leer su valor y JavaScript del lado del cliente no. Para evitar recuperar innecesariamente el FPID en cada carga de página, también se establece una cookie `FPID_CLIENT`, que indica que el FPID se ha generado y expone el valor a JavaScript del lado del cliente para su uso.

El valor FPID se utiliza para parametrizar llamadas mediante el SDK web de Platform.

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

+ [Etiquetas en Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ AEM [Biblioteca de cliente de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

AEM AEM La llamada XHR al servlet personalizado de FPID es rápida, aunque asíncrona, por lo que es posible para un usuario visitar una página web servida por el usuario y navegar fuera antes de que la solicitud se pueda completar.
AEM Si esto ocurre, el mismo proceso volverá a intentar la carga de página siguiente de una página web desde el punto de vista de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de.

La GET AEM AEM HTTP al servlet FPID (`/bin/aep/fpid`) de la está parametrizada con un parámetro de consulta aleatorio para garantizar que cualquier infraestructura entre el explorador y el servicio de Publish no almacene en caché la respuesta de la solicitud.
Del mismo modo, el encabezado de solicitud `Cache-Control: no-store` se agrega para admitir la evitación del almacenamiento en caché.

AEM Tras la invocación del servlet FPID de la aplicación, el FPID se recupera de la respuesta JSON y se utiliza en el [SDK web de Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) para enviarlo a las API de Experience Platform.

Consulte la documentación del Experience Platform para obtener más información sobre [el uso de FPID en identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

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

Por último, las solicitudes HTTP al servlet FPID personalizado deben permitirse a través de la configuración `filter.any` de Dispatcher, que se encuentra en la configuración de la GET de datos de AEM.

Si esta configuración de Dispatcher no se implementa correctamente, las solicitudes de GET HTTP a `/bin/aep/fpid` generarán un error 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## recursos de Experience Platform

Revise la siguiente documentación del Experience Platform para los ID de dispositivos de origen (FPID) y para administrar los datos de identidad con el SDK web de Platform.

+ [Generar ID de dispositivos de origen](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [ID de dispositivos de origen en el SDK web de Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Datos de identidad en el SDK web de Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
