---
title: Uso del asistente SSL en AEM
description: Asistente de configuración SSL de Adobe Experience Manager para facilitar la configuración de una instancia de AEM para que se ejecute en HTTPS.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 564
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '448'
ht-degree: 100%

---

# Uso del asistente SSL en AEM

Aprenda a configurar SSL en Adobe Experience Manager para que se ejecute a través de HTTPS mediante el asistente de SSL integrado.

>[!VIDEO](https://video.tv.adobe.com/v/33680?quality=12&learn=on&captions=spa)


>[!NOTE]
>
>En los entornos administrados, es mejor que el departamento de TI proporcione certificados y claves para la CA de confianza.
>
>Los certificados autofirmados solo se utilizan para el desarrollo.

## Uso del asistente de configuración SSL

Vaya a __AEM Author > Herramientas > Seguridad > Configuración SSL__ y abra el __Asistente de configuración SSL__.

![Asistente de configuración SSL](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### Creación de credenciales de almacenamiento

Para crear un _almacén de claves_ asociado al `ssl-service`usuario del sistema y un _almacén de confianza_ global, use el paso del asistente __Credenciales de almacenamiento__.

1. Escriba la contraseña y confírmela para el __almacén de claves__ asociado al usuario del sistema `ssl-service`.
1. Escriba la contraseña y confírmela para el __almacén de confianza__ global. Tenga en cuenta que es un almacén de confianza de todo el sistema y, si ya se ha creado, se ignorará la contraseña introducida.

   ![Configuración SSL: credenciales de almacenamiento](assets/use-the-ssl-wizard/store-credentials.png)

### Carga de la clave privada y el certificado

Para cargar la _clave privada_ y el _certificado SSL_, use el paso del asistente __Clave y certificado__.

Normalmente, el departamento de TI proporciona el certificado y la clave de la CA de confianza; sin embargo, el certificado autofirmado se puede usar para el __desarrollo__ y para __pruebas__.

Para crear o descargar el certificado autofirmado, consulte [Clave privada y certificado autofirmados](#self-signed-private-key-and-certificate).

1. Cargue la __clave privada__ en el formato DER (Reglas de codificación distinguidas, Distinguished Encoding Rules). A diferencia de PEM, los archivos con codificación DER no contienen instrucciones de texto sin formato como `-----BEGIN CERTIFICATE-----`
1. Cargue el __certificado SSL__ asociado al formato `.crt`.

   ![Configuración SSL: clave privada y certificado](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### Actualización de los detalles del conector SSL

Para actualizar el _nombre de host_ y el _puerto_, use el paso del asistente __Conector SSL__.

1. Actualice o verifique el valor de __Nombre de host HTTPS__; debe coincidir con el `Common Name (CN)` del certificado.
1. Actualice o verifique el valor del __puerto HTTPS__.

   ![Configuración SSL: detalles del conector SSL](assets/use-the-ssl-wizard/ssl-connector-details.png)

### Verificación de la configuración SSL

1. Para verificar SSL, haga clic en el botón __Ir a la dirección URL HTTPS__.
1. Si usa un certificado autofirmado, verá el error `Your connection is not private`.

   ![Configuración SSL: verificación de AEM a través de HTTPS](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## Clave privada y certificado autofirmados

El siguiente archivo comprimido zip contiene los archivos [!DNL DER] y [!DNL CRT] necesarios para configurar AEM SSL localmente y destinados únicamente para el desarrollo local.

Los archivos [!DNL DER] y [!DNL CERT] se proporcionan para su comodidad y se generan siguiendo los pasos que se describen en la siguiente sección Generación de clave privada y certificado autofirmado.

Si es necesario, la frase de contraseña para el certificado es **admin**.

Este localhost: clave privada y certificado autofirmado.zip (caduca en julio de 2028)

[Descargar el archivo de certificado](assets/use-the-ssl-wizard/certificate.zip)

### Generación de clave privada y certificado autofirmado

El vídeo anterior muestra la configuración de SSL en una instancia de autor de AEM mediante certificados autofirmados. Los comandos siguientes que utilizan [[!DNL OpenSSL]](https://www.openssl.org/) pueden generar una clave privada y un certificado para usarlos en el paso 2 del asistente.

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
