---
title: Uso del asistente SSL en AEM
description: Asistente de configuración SSL de Adobe Experience Manager para facilitar la configuración de una instancia de AEM para que se ejecute en HTTPS.
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: 835c01cb2ad1d154437087c51c70a2daf90493dd
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---

# Uso del asistente SSL en AEM

Asistente de configuración SSL de Adobe Experience Manager para facilitar la configuración de una instancia de AEM para que se ejecute en HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Para los entornos administrados, es mejor que el departamento de TI proporcione certificados y claves de confianza de CA.
>
>Los certificados autofirmados solo deben utilizarse con fines de desarrollo.

## Clave privada y descarga de certificado autofirmado

El siguiente zip contiene los archivos [!DNL DER] y [!DNL CRT] necesarios para configurar AEM SSL en localhost y destinados únicamente a fines de desarrollo local.

Los archivos [!DNL DER] y [!DNL CERT] se proporcionan para mayor comodidad y se generan siguiendo los pasos descritos en la sección Generar clave privada y certificado autofirmado , a continuación.

Si es necesario, la frase de pase del certificado es **admin**.

localhost: clave privada y certificate.zip autofirmado (caduca en julio de 2028)

[Descargar el archivo Certificado](assets/use-the-ssl-wizard/certificate.zip)

## Clave privada y generación de certificado autofirmado

El vídeo anterior muestra la configuración de SSL en una instancia de autor AEM que utiliza certificados autofirmados. Los siguientes comandos que utilizan [[!DNL OpenSSL]](https://www.openssl.org/) pueden generar una clave privada y un certificado que se utilizarán en el paso 2 del asistente.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
