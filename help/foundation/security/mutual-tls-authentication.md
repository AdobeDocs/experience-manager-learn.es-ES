---
title: AEM Autenticación de seguridad de la capa de transporte mutuo (mTLS) desde el punto de vista de la seguridad de
description: AEM Obtenga información sobre cómo realizar llamadas HTTPS desde el punto de vista de la seguridad a las API web que requieren autenticación de Seguridad de capa de transporte mutuo (mTLS).
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-13881
thumbnail: KT-13881.png
doc-type: Article
last-substantial-update: 2023-10-10T00:00:00Z
exl-id: 7238f091-4101-40b5-81d9-87b4d57ccdb2
duration: 495
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# AEM Autenticación de seguridad de la capa de transporte mutuo (mTLS) desde el punto de vista de la seguridad de

AEM Obtenga información sobre cómo realizar llamadas HTTPS desde el punto de vista de la seguridad a las API web que requieren autenticación de Seguridad de capa de transporte mutuo (mTLS).

>[!VIDEO](https://video.tv.adobe.com/v/3424855?quality=12&learn=on)

La autenticación mTLS o TLS bidireccional mejora la seguridad del protocolo TLS al requerir que **tanto el cliente como el servidor se autenticen mutuamente**. Esta autenticación se realiza mediante certificados digitales. Se utiliza comúnmente en escenarios donde la seguridad sólida y la verificación de identidad son críticas.

De forma predeterminada, al intentar establecer una conexión HTTPS con una API web que requiera autenticación mTLS, la conexión falla con el siguiente error:

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

Este problema se produce cuando el cliente no presenta un certificado para autenticarse a sí mismo.

AEM Vamos a aprender a llamar correctamente a las API que requieren autenticación mTLS mediante [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) y **KeyStore y TrustStore**.


## AEM HttpClient y cargar material de KeyStore de la

AEM En un nivel superior, se requieren los siguientes pasos para llamar a una API protegida con mTLS desde el.

### AEM Generación de certificados de

AEM Solicite el certificado de asociándose con el equipo de seguridad de su organización. El equipo de seguridad proporciona o solicita los detalles relacionados con el certificado, como la clave, la solicitud de firma de certificado (CSR) y, mediante CSR, se emite el certificado.

Para fines de demostración, genere los detalles relacionados con el certificado, como la clave, la solicitud de firma de certificado (CSR). En el siguiente ejemplo, se utiliza una CA autofirmada para emitir el certificado.

- En primer lugar, genere el certificado interno de la entidad de certificación (CA).

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- AEM Genere el certificado de la.

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- AEM AEM Convierta la clave privada en formato DER, ya que, para usar el almacén de claves, es necesario usar la clave privada en formato DER.

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>Los certificados de CA firmados automáticamente solo se utilizan con fines de desarrollo. Para la producción, utilice una entidad emisora de certificados (CA) de confianza para emitir el certificado.


### Intercambio de certificados

AEM Si utiliza una CA autofirmada para el certificado de la, como se ha indicado anteriormente, envíe el certificado o el certificado de la entidad de certificación (CA) interno al proveedor de la API.

Además, si el proveedor de API utiliza un certificado de CA autofirmado, reciba el certificado o el certificado de entidad de certificación (CA) interno del proveedor de API.

### Importación de certificados

AEM Para importar el certificado de la, siga los siguientes pasos:

1. AEM Inicie sesión en **autor** como **administrador**.

1. AEM Vaya a **Autor de la > Herramientas > Seguridad > Usuarios > Crear o seleccionar un usuario existente**.

   ![Crear o seleccionar un usuario existente](assets/mutual-tls-authentication/create-or-select-user.png)

   Para fines de demostración, se crea un nuevo usuario denominado `mtl-demo-user`.

1. Para abrir **Propiedades de usuario**, haga clic en el nombre de usuario.

1. Haga clic en la pestaña **Almacén de claves** y luego haga clic en el botón **Crear almacén de claves**. A continuación, en el cuadro de diálogo **Establecer contraseña de acceso a KeyStore**, establezca una contraseña para el almacén de claves de este usuario y haga clic en Guardar.

   ![Crear almacén de claves](assets/mutual-tls-authentication/create-keystore.png)

1. En la nueva pantalla, en la sección **AGREGAR CLAVE PRIVADA DEL ARCHIVO DER**, siga los siguientes pasos:

   1. Especificar alias

   1. AEM Importe la clave privada de la en formato DER, generado anteriormente.

   1. Importe los archivos de la cadena de certificados generados anteriormente.

   1. Haga clic en Enviar

      AEM ![Importar Clave Privada De](assets/mutual-tls-authentication/import-aem-private-key.png)

1. Compruebe que el certificado se haya importado correctamente.

   AEM ![Clave Privada Y Certificado Importados](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

AEM Si el proveedor de API utiliza un certificado de CA autofirmado, importe el certificado recibido en el almacén de confianza de la entidad emisora de certificados de la entidad emisora de certificados de la entidad emisora de certificados de la entidad emisora de certificados de la entidad emisora de certificados de la entidad emisora de certificados, siga los pasos de [aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html#httpclient-and-load-aem-truststore-material).

AEM Del mismo modo, si el usuario utiliza un certificado de CA firmado automáticamente, solicite al proveedor de la API que lo importe.

### Código de invocación de API de mTLS prototípico mediante HttpClient

Actualice el código Java™ como se muestra a continuación. AEM Para usar la anotación `@Reference` para obtener el servicio `KeyStoreService` que se está usando, el código de llamada debe ser un componente/servicio OSGi o un modelo Sling (y se usa `@OsgiService` allí).


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
    sslbuilder.loadTrustMaterial(aemTrustStore, null);

    // Create SSL Connection Socket using above SSL Context
    SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
            sslbuilder.build(), NoopHostnameVerifier.INSTANCE);

    // Create HttpClientBuilder
    HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
    httpClientBuilder.setSSLSocketFactory(sslsf);

    // Create HttpClient
    CloseableHttpClient httpClient = httpClientBuilder.build();

    // Invoke API
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
}

/**
 * 
 * Returns the global AEM TrustStore
 * 
 * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
 *                         operations easy.
 * @param resourceResolver
 * @return
 */
private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM TrustStore from the KeyStoreService and ResourceResolver
    KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);

    return aemTrustStore;
}

...
```

- Inserte el servicio OSGi OOTB `com.adobe.granite.keystore.KeyStoreService` en su componente OSGi.
- AEM Para obtener el almacén de claves de del usuario con `KeyStoreService` y `ResourceResolver`, el método `getAEMKeyStore(...)` lo hace.
- AEM Si el proveedor de API utiliza un certificado de CA autofirmado, obtenga el almacén de confianza de la entidad emisora de certificados de la red global, el método `getAEMTrustStore(...)` hace eso.
- Cree un objeto de `SSLContextBuilder`, consulte Java™ [Detalles de la API](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
- AEM Cargue el almacén de claves de la clave de acceso del usuario en `SSLContextBuilder` mediante el método `loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)`.
- La contraseña del almacén de claves es la contraseña que se estableció al crear el almacén de claves; debe almacenarse en la configuración OSGi; consulte [Valores de configuración secreta](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values).

## Evitar cambios en el almacén de claves JVM

Un enfoque convencional para invocar de forma eficaz las API de mTLS con certificados privados implica la modificación del repositorio de claves JVM. Se logra importando los certificados privados mediante el comando Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549).

AEM Sin embargo, este método no está alineado con las prácticas recomendadas de seguridad y ofrece una opción superior mediante el uso de **KeyStores específicos del usuario y Global TrustStore** y [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).

## Paquete de soluciones

El proyecto Node.js de muestra que se muestra en el vídeo se puede descargar desde [aquí](assets/internal-api-call/REST-APIs.zip).

AEM El código de servlet de la está disponible en la rama `tutorial/web-api-invocation` del proyecto WKND Sites, [consulte](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
