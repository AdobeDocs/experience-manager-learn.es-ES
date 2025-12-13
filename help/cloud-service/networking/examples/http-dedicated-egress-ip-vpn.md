---
title: Conexiones HTTP/HTTPS para dirección IP de salida dedicada y VPN
description: Obtenga información sobre cómo realizar solicitudes HTTP/HTTPS desde AEM as a Cloud Service a servicios web externos que se ejecutan para direcciones IP de salida dedicadas y VPN
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
duration: 70
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# Conexiones HTTP/HTTPS para dirección IP de salida dedicada y VPN

Las conexiones HTTP/HTTPS se procesan como proxy automáticamente fuera de AEM as a Cloud Service con dirección IP de salida dedicada o VPN, y no necesitan reglas especiales de `portForwards`.

## Compatibilidad avanzada con redes

Las siguientes opciones avanzadas de red admiten el siguiente ejemplo de código.

Asegúrese de que la configuración avanzada de red de la dirección IP de salida [dedicada o VPN](../advanced-networking.md#advanced-networking) se haya configurado antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Este ejemplo de código es solo para [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) y [VPN](../vpn.md). Hay un ejemplo de código similar, pero diferente, disponible para [conexiones HTTP/HTTPS en puertos no estándar para salida de puerto flexible](./http-on-non-standard-ports-flexible-port-egress.md).

## Ejemplo de código

Este ejemplo de código Java™ es de un servicio OSGi que se puede ejecutar en AEM as a Cloud Service y que realiza una conexión HTTP a un servidor web externo en 8080. Las conexiones HTTPS (o HTTP) se procesan como proxy automáticamente fuera de AEM as a Cloud Service y no requieren un desarrollo especial.

>[!NOTE]
> Se recomienda usar las [API HTTP de Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) para hacer llamadas HTTP/HTTPS desde AEM.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

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
