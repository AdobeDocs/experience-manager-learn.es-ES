---
title: Utilice el Asistente para SSL en AEM
description: Asistente de configuración de SSL de Adobe Experience Manager para facilitar la configuración de una instancia de AEM para que se ejecute mediante HTTPS.
seo-description: Asistente de configuración de SSL de Adobe Experience Manager para facilitar la configuración de una instancia de AEM para que se ejecute mediante HTTPS.
version: 6.3, 6,4, 6.5
feature: null
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# Utilice el Asistente para SSL en AEM

Asistente de configuración de SSL de Adobe Experience Manager para facilitar la configuración de una instancia de AEM para que se ejecute mediante HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Para entornos administrados, lo mejor es que el departamento de TI proporcione certificados y claves de confianza de CA.
>
>Los certificados con firma automática solo se utilizan para fines de desarrollo.

## Clave privada y descarga de certificado con firma automática

El siguiente zip contiene [!DNL DER] y [!DNL CRT] archivos necesarios para configurar AEM SSL en localhost y destinados únicamente a fines de desarrollo local.

Los archivos [!DNL DER] y [!DNL CERT] se proporcionan para mayor comodidad y se generan siguiendo los pasos descritos en la sección Generar clave privada y certificado con firma automática que se muestra a continuación.

Si es necesario, la frase de pase del certificado es **admin**.

localhost: clave privada y certificado autofirmado.zip (caduca en julio de 2028)

[Descargar el archivo de certificado](assets/use-the-ssl-wizard/certificate.zip)

## Generación de certificados con firma automática y clave privada

El vídeo de arriba muestra la configuración de SSL en una instancia de autor de AEM mediante certificados con firma automática. Los siguientes comandos que utilizan [[!DNL OpenSSL]](https://www.openssl.org/) pueden generar una clave privada y un certificado que se utilizarán en el paso 2 del asistente.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
