---
title: Conexiones HTTP/HTTPS en puertos no estándar para salida de puerto flexible
description: Aprenda a realizar solicitudes HTTP/HTTPS desde AEM as a Cloud Service a servicios web externos que se ejecutan en puertos no estándar para la salida de puerto flexible.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
duration: 86
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# Conexiones HTTP/HTTPS en puertos no estándar para salida de puerto flexible

Las conexiones HTTP/HTTPS en puertos no estándar (no 80/443) deben procesarse como proxy fuera de AEM as a Cloud Service AEM, sin embargo, no necesitan reglas especiales de `portForwards` y pueden usar el puerto de red avanzado `AEM_PROXY_HOST` y un puerto proxy reservado `AEM_HTTP_PROXY_PORT` o `AEM_HTTPS_PROXY_PORT`, según si el destino es HTTP/HTTPS.

## Compatibilidad avanzada con redes

Las siguientes opciones avanzadas de red admiten el siguiente ejemplo de código.

Asegúrese de que la configuración avanzada de red [proper](../advanced-networking.md#advanced-networking) se haya configurado antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Este ejemplo de código es solo para [salida de puerto flexible](../flexible-port-egress.md). Hay un ejemplo de código similar, pero diferente, disponible para [conexiones HTTP/HTTPS en puertos no estándar para direcciones IP de salida dedicadas y VPN](./http-dedicated-egress-ip-vpn.md).

## Ejemplo de código

Este ejemplo de código Java™ es de un servicio OSGi que se puede ejecutar en AEM as a Cloud Service y que realiza una conexión HTTP a un servidor web externo en 8080. AEM Las conexiones a servidores web HTTPS utilizan las variables de entorno `AEM_PROXY_HOST` y `AEM_HTTPS_PROXY_PORT` (el valor predeterminado es `proxy.tunnel:3128` en versiones de &lt; 6094).

>[!NOTE]
> AEM Se recomienda usar las [API HTTP de Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) para hacer llamadas HTTP/HTTPS desde el servidor de correo electrónico de la red (HTTPs) de la red de llamadas de la red de área de servicio (HTTPs.

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

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

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
