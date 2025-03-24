---
title: Uso de la API usagerights
description: Código de ejemplo para aplicar derechos de uso al PDF suministrado
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a4e2132b-3cfd-4377-8998-6944365edec5
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---

# Realizar llamada API

## Aplicar derechos de uso

Una vez que tenga el token de acceso, el siguiente paso es realizar una solicitud de API para aplicar derechos de uso al PDF especificado. Esto implica incluir el token de acceso en el encabezado de la solicitud para autenticar la llamada, lo que garantiza un procesamiento seguro y autorizado del documento.

La siguiente función aplica los derechos de uso

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## Desglose de funciones:



* **Configurar punto final y carga de API**
   * Construye la dirección URL de la API usando el `endPoint` proporcionado y un `BUCKET` predefinido.
   * Define una cadena JSON (`usageRights`) que especifica los derechos que se aplicarán, como:
      * Comentarios
      * Archivos incrustados
      * Rellenado de formularios
      * Exportación de datos de formulario

* **Cargar el archivo de PDF**
   * Recupera el archivo `withoutusagerights.pdf` del directorio `pdffiles`.
   * Registra un error y sale si no se encuentra el archivo.

* **Preparar la solicitud HTTP**
   * Lee el archivo PDF en una matriz de bytes.
   * Utiliza `MultipartEntityBuilder` para crear una solicitud de varias partes que contenga:
      * El archivo PDF como cuerpo binario.
      * El JSON `usageRights` como un cuerpo de texto.
   * Configura una solicitud HTTP `POST` con encabezados:
      * `Authorization: Bearer <accessToken>` para la autenticación.
      * `X-Adobe-Accept-Experimental: 1` (posiblemente requerido para la compatibilidad con API).

* **Enviar la solicitud y controlar la respuesta**
   * Ejecuta la solicitud HTTP mediante `httpClient.execute(httpPost)`.
   * Lee la respuesta (se espera que sea el PDF actualizado con derechos de uso aplicados).
   * Escribe el contenido de PDF recibido en **&quot;ReaderExtended.pdf&quot;** a las `SAVE_LOCATION`.

* **Control y limpieza de errores**
   * Captura y registra cualquier error de `IOException`.
   * Garantiza que todos los recursos (secuencias, cliente HTTP, respuesta) se cierren correctamente en el bloque `finally`.

El siguiente es el código main.java que invoca la función applyUsageRights

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

El método `main` se inicializa llamando a `getAccessToken()` desde `AccessTokenService`, que se espera devuelva un token válido.

* Luego llama a `applyUsageRights()` desde la clase `DocumentGeneration`, pasando:
   * Se recuperó `accessToken`
   * Punto final de la API para aplicar derechos de uso.


## Siguientes pasos

[Implementar el proyecto de ejemplo](sample-project.md)
