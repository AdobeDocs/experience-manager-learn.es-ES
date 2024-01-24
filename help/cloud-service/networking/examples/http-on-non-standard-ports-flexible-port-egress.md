---
title: Conexiones HTTP/HTTPS en puertos no estándar para salida de puerto flexible
description: AEM Obtenga información sobre cómo realizar solicitudes HTTP/HTTPS desde servicios web as a Cloud Service a externos que se ejecutan en puertos no estándar para la salida de puerto flexible.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
duration: 115
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# Conexiones HTTP/HTTPS en puertos no estándar para salida de puerto flexible

AEM Las conexiones HTTP/HTTPS en puertos no estándar (no 80/443) deben procesarse como proxy fuera de las conexiones as a Cloud Service, aunque no necesitan ninguna conexión especial `portForwards` AEM , y puede utilizar las funciones de red avanzadas de la red de la manera más avanzada `AEM_PROXY_HOST` y un puerto proxy reservado `AEM_HTTP_PROXY_PORT` o `AEM_HTTPS_PROXY_PORT` en función de si el destino es HTTP/HTTPS.

## Compatibilidad avanzada con redes

Las siguientes opciones avanzadas de red admiten el siguiente ejemplo de código.

Asegúrese de que la [apropiado](../advanced-networking.md#advanced-networking) la configuración avanzada de red se ha establecido antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Este ejemplo de código es solo para [Salida de puerto flexible](../flexible-port-egress.md). Hay disponible un ejemplo de código similar, pero diferente, para [Conexiones HTTP/HTTPS en puertos no estándar para direcciones IP de salida dedicadas y VPN](./http-dedicated-egress-ip-vpn.md).

## Ejemplo de código

AEM Este ejemplo de código Java™ es de un servicio OSGi que se puede ejecutar en as a Cloud Service que realiza una conexión HTTP a un servidor web externo en 8080. Las conexiones a servidores web HTTPS utilizan las variables de entorno `AEM_PROXY_HOST` y `AEM_HTTPS_PROXY_PORT` (de forma predeterminada, `proxy.tunnel:3128` AEM en versiones de &lt; 6094).

>[!NOTE]
> Se recomienda el [API HTTP de Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) AEM se utilizan para realizar llamadas HTTP/HTTPS desde el servicio de llamadas a través de la red de.

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
