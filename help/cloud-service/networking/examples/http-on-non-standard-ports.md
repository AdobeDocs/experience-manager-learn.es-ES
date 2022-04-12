---
title: Conexiones HTTP/HTTPS en puertos no estándar para direcciones IP de salida dedicadas y VPN
description: Aprenda a hacer solicitudes HTTP/HTTPS de AEM as a Cloud Service a servicios web externos que se ejecuten en puertos no estándar para direcciones IP y VPN de salida dedicadas
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---

# Conexiones HTTP/HTTPS en puertos no estándar para direcciones IP de salida dedicadas y VPN

Las conexiones HTTP/HTTPS en puertos no estándar (no 80/443) deben procesarse como proxy fuera de AEM as a Cloud Service, pero no necesitan ninguna conexión especial `portForwards` y puede utilizar AEM `AEM_HTTP_PROXY_HOST`, `AEM_HTTP_PROXY_PORT`, `AEM_HTTPS_PROXY_HOST`y `AEM_HTTPS_PROXY_PORT`.

## Compatibilidad avanzada con redes

El siguiente ejemplo de código es compatible con las siguientes opciones avanzadas de red.

Asegúrese de que la variable [apropiado](../advanced-networking.md#advanced-networking) se ha configurado la configuración avanzada de redes antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ü | ü | š | š |

>[!CAUTION]
>
> Este ejemplo de código solo sirve para [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) y [VPN](../vpn.md). Hay un ejemplo de código similar, pero diferente, disponible para [Conexiones HTTP/HTTPS en puertos no estándar para alimentación de puerto flexible](./http-on-non-standard-ports-flexible-port-egress.md).

## Ejemplo de código

Este ejemplo de código Java™ es de un servicio OSGi que puede ejecutarse en AEM as a Cloud Service que realiza una conexión HTTP a un servidor web externo en el 8080. Las conexiones a los servidores web HTTPS utilizan la variable `AEM_HTTPS_PROXY_HOST` y `AEM_HTTPS_PROXY_PORT` en lugar de  `AEM_HTTP_PROXY_HOST` y `AEM_HTTP_PROXY_PORT`.

>[!NOTE]
> Se recomienda usar la variable [API HTTP de Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) se utilizan para realizar llamadas HTTP/HTTPS desde AEM.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // If the URL is http, use System.getenv("AEM_HTTP_PROXY_HOST") and System.getenv("AEM_HTTP_PROXY_PORT")
        // Else if the URL is https, us System.getenv("AEM_HTTPS_PROXY_HOST") and System.getenv("AEM_HTTPS_PROXY_PORT")

        if (System.getenv("AEM_HTTP_PROXY_HOST") != null) {
            // Create a ProxySelector that maps to AEM's provided AEM_HTTP_PROXY_HOST and AEM_HTTP_PROXY_PORT
            ProxySelector proxySelector = ProxySelector.of(
                    new InetSocketAddress(System.getenv("AEM_HTTP_PROXY_HOST"),
                            Integer.parseInt(System.getenv("AEM_HTTP_PROXY_PORT"))));
            // Create an HttpClient and provide the proxy selector that will use AEM's native HTTP proxy configuration
            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_HTTP_PROXY");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_HTTP_PROXY");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
