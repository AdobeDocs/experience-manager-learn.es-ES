---
title: Conexiones HTTP/HTTPS en puertos no estándar para salida de puerto flexible
description: Aprenda a hacer solicitudes HTTP/HTTPS de AEM as a Cloud Service a servicios web externos que se ejecutan en puertos no estándar para el consumo de puerto flexible.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Conexiones HTTP/HTTPS en puertos no estándar para salida de puerto flexible

Las conexiones HTTP/HTTPS en puertos no estándar (no 80/443) deben procesarse como proxy fuera de AEM as a Cloud Service, pero no necesitan ninguna conexión especial `portForwards` y puede utilizar AEM `AEM_PROXY_HOST` y un puerto proxy reservado `AEM_HTTP_PROXY_HOST` o `AEM_HTTPS_PROXY_HOST` según es el destino es HTTP/HTTPS.

## Compatibilidad avanzada con redes

El siguiente ejemplo de código es compatible con las siguientes opciones avanzadas de red.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ü | š | ü | ü |

>[!CAUTION]
>
> Este ejemplo de código solo sirve para [Salida de puerto flexible](../flexible-port-egress.md). Hay un ejemplo de código similar, pero diferente, disponible para [Conexiones HTTP/HTTPS en puertos no estándar para direcciones IP de salida dedicadas y VPN](./http-on-non-standard-ports.md).

## Ejemplo de código

Este ejemplo de código Java™ es de un servicio OSGi que puede ejecutarse en AEM as a Cloud Service que realiza una conexión HTTP a un servidor web externo en el 8080. Las conexiones a servidores web HTTPS utilizan variables de entorno `AEM_PROXY_HOST` y `AEM_HTTPS_PROXY_PORT` (de forma predeterminada, `proxy.tunnel:3128` en versiones de AEM &lt; 6094).

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

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_HOST") 
        // or System.getenv("AEM_HTTPS_PROXY_HOST"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that maps to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel
            // And proxy port contained in the AEM_HTTP_PROXY_PORT variable if the destination requires HTTP, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT
            // The explicit fallback of 3128 will be obsoleted in Jan 2022, and only the  AEM_HTTP_PROXY_PORT/AEM_HTTPS_PROXY_PORT will be required
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", "3128"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
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
