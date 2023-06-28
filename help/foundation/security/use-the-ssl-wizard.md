---
title: AEM Uso del Asistente para SSL en la
description: Asistente de configuración de SSL de Adobe Experience Manager AEM para facilitar la configuración de una instancia de para que se ejecute en HTTPS.
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
topics: security, operations
feature: Security
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# AEM Uso del Asistente para SSL en la

Asistente de configuración de SSL de Adobe Experience Manager AEM para facilitar la configuración de una instancia de para que se ejecute en HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

Abra el __Asistente de configuración SSL__ se puede abrir directamente navegando hasta __AEM Author > Herramientas > Seguridad > Configuración SSL__.

>[!NOTE]
>
>Para entornos administrados, es mejor que el departamento de TI proporcione certificados y claves de confianza para la CA.
>
>Los certificados autofirmados solo se utilizan con fines de desarrollo.

## Clave privada y descarga de certificado autofirmada

El siguiente zip contiene [!DNL DER] y [!DNL CRT] AEM archivos necesarios para configurar SSL en el host local y destinados únicamente a fines de desarrollo local.

El [!DNL DER] y [!DNL CERT] Los archivos se proporcionan para su comodidad y se generan siguiendo los pasos descritos en la sección Generar clave privada y certificado firmado automáticamente a continuación.

Si es necesario, la frase de contraseña del certificado es **administrador**.

localhost: clave privada y certificado autofirmado.zip (caduca en julio de 2028)

[Descargar el archivo de certificado](assets/use-the-ssl-wizard/certificate.zip)

## Generación de claves privadas y certificados autofirmados

AEM El vídeo anterior muestra la instalación y configuración de SSL en una instancia de autor de mediante certificados firmados automáticamente. Los siguientes comandos utilizan [[!DNL OpenSSL]](https://www.openssl.org/) Puede generar una clave privada y un certificado para utilizarlos en el paso 2 del asistente.

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
