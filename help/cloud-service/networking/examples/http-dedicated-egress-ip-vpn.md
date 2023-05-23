---
title: Conexiones HTTP/HTTPS para dirección IP de salida dedicada y VPN
description: AEM Obtenga información sobre cómo realizar solicitudes HTTP/HTTPS desde servicios web as a Cloud Service a externos que se ejecutan para direcciones IP de salida dedicadas y VPN
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: bdce84fdcc949c8f8d0690ee7110238d8e8d3e42
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 0%

---

# Conexiones HTTP/HTTPS para dirección IP de salida dedicada y VPN

AEM Las conexiones HTTP/HTTPS se procesan como proxy automáticamente fuera de las conexiones as a Cloud Service con direcciones IP de salida dedicadas o VPN, y no necesitan ninguna conexión especial `portForwards` reglas.

## Compatibilidad avanzada con redes

Las siguientes opciones avanzadas de red admiten el siguiente ejemplo de código.

Asegúrese de que la [dirección IP de salida dedicada o VPN](../advanced-networking.md#advanced-networking) la configuración avanzada de red se ha establecido antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Este ejemplo de código es solo para [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) y [VPN](../vpn.md). Hay disponible un ejemplo de código similar, pero diferente, para [Conexiones HTTP/HTTPS en puertos no estándar para salida de puerto flexible](./http-on-non-standard-ports-flexible-port-egress.md).

## Ejemplo de código

AEM Este ejemplo de código Java™ es de un servicio OSGi que se puede ejecutar en as a Cloud Service que realiza una conexión HTTP a un servidor web externo en 8080. AEM Las conexiones HTTPS (o HTTP) se procesan como proxy automáticamente fuera de las conexiones as a Cloud Service, y no requieren un desarrollo especial.

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
