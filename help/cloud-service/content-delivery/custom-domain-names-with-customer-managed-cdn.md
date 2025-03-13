---
title: Nombre de dominio personalizado con CDN administrada por el cliente
description: Obtenga información sobre cómo implementar un nombre de dominio personalizado en el sitio web de AEM as a Cloud Service que utiliza una CDN administrada por el cliente.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-21T00:00:00Z
jira: KT-15945
thumbnail: KT-15945.jpeg
exl-id: fa9ee14f-130e-491b-91b6-594ba47a7278
source-git-commit: 98f1996dbeb6a683f98ae654e8fa13f6c7a2f9b2
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Nombre de dominio personalizado con CDN administrada por el cliente

Aprenda a agregar un nombre de dominio personalizado a un sitio web de AEM as a Cloud Service que use un **CDN administrado por el cliente**.

En este tutorial, la marca del sitio de muestra [AEM WKND](https://github.com/adobe/aem-guides-wknd) se ha mejorado al agregar un nombre de dominio personalizado `wkndviaawscdn.enablementadobe.com` al que se puede dirigir mediante HTTPS con Seguridad de la capa de transporte (TLS) mediante una CDN administrada por el cliente. En este tutorial, AWS CloudFront se utiliza como CDN administrada por el cliente, pero cualquier proveedor de CDN debe ser compatible con AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432561?quality=12&learn=on)

Los pasos de alto nivel son los siguientes:

![Nombre de dominio personalizado con CDN del cliente](./assets/add-custom-domain-name-with-customer-CDN.png){width="800" zoomable="yes"}

## Requisitos previos

>[!VIDEO](https://video.tv.adobe.com/v/3432562?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) y [dig](https://www.isc.org/blogs/dns-checker/) están instalados en el equipo local.
- Acceso a servicios de terceros:
   - Autoridad de certificación (CA): para solicitar el certificado firmado para el dominio del sitio, como [DigitCert](https://www.digicert.com/)
   - CDN del cliente: para configurar la CDN del cliente y agregar certificados SSL y detalles de dominio, como AWS CloudFront, Azure CDN o Akamai.
   - Servicio de alojamiento del Sistema de nombres de dominio (DNS): para agregar registros DNS para su dominio personalizado, como Azure DNS o AWS Route 53.
- Acceso a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) para implementar la regla de CDN de validación de encabezado HTTP en el entorno de AEM as a Cloud Service.
- El sitio de muestra [AEM WKND](https://github.com/adobe/aem-guides-wknd) se ha implementado en el entorno AEM as a Cloud Service de tipo [programa de producción](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs).

Si no tiene acceso a servicios de terceros, _colabore con su equipo de seguridad o de alojamiento para completar los pasos_.

## Generar certificado SSL

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Tiene dos opciones:

1. Con la herramienta de línea de comandos `openssl`, puede generar una clave privada y una solicitud de firma de certificado (CSR) para el dominio del sitio. Para solicitar un certificado firmado, envíe el CSR a una entidad emisora de certificados (CA).
1. El equipo de alojamiento proporciona la clave privada y el certificado firmado necesarios para el sitio.

Revisemos los pasos de la primera opción.

Para generar una clave privada y una CSR, ejecute los siguientes comandos y proporcione la información necesaria cuando se le solicite:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Para solicitar un certificado firmado, proporcione el CSR generado a la CA siguiendo su documentación. Una vez que la CA firme el CSR, recibirá el archivo de certificado firmado.

### Revisar certificado firmado

Es recomendable revisar el certificado firmado antes de agregarlo a Cloud Manager. Puede revisar los detalles del certificado mediante el siguiente comando:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

El certificado firmado puede contener la cadena de certificados, que incluye los certificados raíz e intermedios junto con el certificado de entidad final.

Adobe Cloud Manager acepta el certificado de entidad final y la cadena de certificado _en campos de formulario independientes_, por lo que debe extraer el certificado de entidad final y la cadena de certificado del certificado firmado.

En este tutorial, se usa como ejemplo el certificado firmado de [DigitCert](https://www.digicert.com/) emitido contra el dominio `*.enablementadobe.com`. La entidad final y la cadena de certificados se extraen abriendo el certificado firmado en un editor de texto y copiando el contenido entre los marcadores `-----BEGIN CERTIFICATE-----` y `-----END CERTIFICATE-----`.

## Configurar CDN administrado por el cliente

>[!VIDEO](https://video.tv.adobe.com/v/3432563?quality=12&learn=on)

Configure la CDN del cliente, como AWS Cloud Front, Azure CDN o Akamai, y añada el certificado SSL y los detalles de dominio. En este tutorial, se utiliza AWS CloudFront como ejemplo. Sin embargo, según el proveedor de CDN, los pasos pueden variar. Las llamadas clave son:

- Añada el certificado SSL a la CDN.
- Añada el nombre de dominio personalizado a la CDN.
- Configure la CDN para almacenar en caché el contenido, como imágenes, CSS y archivos JavaScript.
- Agregue el encabezado HTTP `X-Forwarded-Host` a la configuración de la CDN para que la CDN incluya este encabezado en todas las solicitudes que envía al origen de AEMCD.
- Asegúrese de que el valor del encabezado `Host` esté establecido en el dominio de AEM as a Cloud Service predeterminado que contiene el ID de programa y entorno y termina con `adobeaemcloud.com`. El valor de la cabecera del host HTTP pasado desde la CDN del cliente a la CDN de Adobe debe ser el dominio de AEM as a Cloud Service predeterminado; cualquier otro valor genera un estado de error.

## Configuración de registros DNS

>[!VIDEO](https://video.tv.adobe.com/v/3432564?quality=12&learn=on)

Para configurar el registro DNS para su dominio personalizado, siga estos pasos,

1. Agregue un registro CNAME para el dominio personalizado que apunte al nombre de dominio CDN.

Este tutorial agrega un registro CNAME al DNS de Azure para el dominio personalizado `wkndviaawscdn.enablementadobe.com` y lo señala al nombre de dominio de distribución de AWS CloudFront.

### Verificación del sitio

Compruebe el nombre de dominio personalizado accediendo al sitio utilizando el nombre de dominio personalizado.
Puede funcionar o no según la configuración de vhhost en el entorno de AEM as a Cloud Service.

Un paso de seguridad crucial es implementar la regla CDN de validación de encabezado HTTP en el entorno de AEM as a Cloud Service. La regla garantiza que la solicitud proviene de la CDN del cliente y no de ninguna otra fuente.

## Estado de trabajo actual sin la regla de CDN de validación de encabezado HTTP

>[!VIDEO](https://video.tv.adobe.com/v/3432565?quality=12&learn=on)

Sin la regla de CDN de validación de encabezado HTTP, el valor del encabezado `Host` se establece en el dominio de AEM as a Cloud Service predeterminado que contiene el ID de programa y entorno y termina con `adobeaemcloud.com`. Adobe CDN transforma el valor del encabezado `Host` al valor de `X-Forwarded-Host` recibido de la CDN del cliente solo si se implementa la regla CDN de validación del encabezado HTTP. De lo contrario, el valor del encabezado `Host` se pasa tal cual al entorno de AEM as a Cloud Service y no se utiliza el encabezado `X-Forwarded-Host`.

### Código servlet de ejemplo para imprimir el valor del encabezado Host

El siguiente código de servlet imprime los valores de encabezado HTTP `Host`, `X-Forwarded-*`, `Referer` y `Via` en la respuesta JSON.

```java
package com.adobe.aem.guides.wknd.core.servlets;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.ServletResolverConstants;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = Servlet.class, property = {
        ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/verify-headers",
        ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
})
public class VerifyHeadersServlet extends SlingSafeMethodsServlet {

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");

        // Create JSON response
        StringBuilder jsonResponse = new StringBuilder();
        jsonResponse.append("{");

        Enumeration<String> headerNames = request.getHeaderNames();
        boolean firstHeader = true;

        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();

            if (headerName.startsWith("X-Forwarded-") || headerName.startsWith("Host")
                    || headerName.startsWith("Referer") || headerName.startsWith("Via")) {
                if (!firstHeader) {
                    jsonResponse.append(",");
                }
                jsonResponse.append("\"").append(headerName).append("\": \"").append(request.getHeader(headerName))
                        .append("\"");
                firstHeader = false;
            }
        }

        jsonResponse.append("}");

        response.getWriter().write(jsonResponse.toString());
    }
}
```

Para probar el servlet, actualice el archivo `../dispatcher/src/conf.dispatcher.d/filters/filters.any` con la configuración siguiente. Asegúrese también de que la CDN está configurada para **NO almacenar en caché** la ruta de acceso `/bin/*`.

```plaintext
# Testing purpose bin
/0300 { /type "allow" /extension "json" /path "/bin/*"}
/0301 { /type "allow" /path "/bin/*"}
/0302 { /type "allow" /url "/bin/*"}
```

## Configurar e implementar la regla de CDN de validación de encabezado HTTP

>[!VIDEO](https://video.tv.adobe.com/v/3432566?quality=12&learn=on)

Para configurar e implementar la regla CDN de validación de encabezado HTTP, siga estos pasos:

- Agregue la regla CDN de validación de encabezado HTTP en el archivo `cdn.yaml`. A continuación se proporciona un ejemplo.

  ```yaml
  kind: "CDN"
  version: "1"
  metadata:
    envTypes: ["prod"]
  data:
    authentication:
      authenticators:
        - name: edge-auth
          type: edge
          edgeKey1: ${{CDN_EDGEKEY_080124}}
          edgeKey2: ${{CDN_EDGEKEY_110124}}
      rules:
        - name: edge-auth-rule
          when: { reqProperty: tier, equals: "publish" }
          action:
          type: authenticate
          authenticator: edge-auth
  ```

- Cree variables de entorno de tipo secreto (CDN_EDGEKEY_080124, CDN_EDGEKEY_110124) mediante la interfaz de usuario de Cloud Manager.
- Implemente la regla CDN de validación de encabezado HTTP en el entorno de AEM as a Cloud Service mediante la canalización de Cloud Manager.

## Pasar secreto en el encabezado HTTP X-AEM-Edge-Key

>[!VIDEO](https://video.tv.adobe.com/v/3432567?quality=12&learn=on)

Actualice la CDN del cliente para pasar el secreto en el encabezado HTTP `X-AEM-Edge-Key`. El secreto lo utiliza la CDN de Adobe para validar que la solicitud proviene de la CDN del cliente y transformar el valor del encabezado `Host` al valor de `X-Forwarded-Host` recibido de la CDN del cliente.

## Vídeo de principio a fin

También puede ver el vídeo completo que muestra los pasos anteriores para agregar un nombre de dominio personalizado con una CDN administrada por el cliente a un sitio alojado en AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432568?quality=12&learn=on)
