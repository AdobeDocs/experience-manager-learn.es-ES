---
title: Nombre de dominio personalizado con CDN administrada por Adobe
description: Obtenga información sobre cómo implementar un nombre de dominio personalizado en el sitio web de AEM as a Cloud Service que utiliza una CDN administrada por Adobe.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 1042
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
exl-id: 8936c3ae-2daf-4d0f-b260-28376ae28087
source-git-commit: 13657903c37b90c6d854dcba317dc1801d869de0
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 1%

---

# Nombre de dominio personalizado con CDN de Adobe

Obtenga información sobre cómo implementar un nombre de dominio personalizado para un sitio web de AEM as a Cloud Service que utilice la red de distribución de contenido de Adobe (CDN).

AEM En este tutorial, la marca del sitio de muestra [WKND](https://github.com/adobe/aem-guides-wknd) se ha mejorado al agregar un nombre de dominio personalizado `wknd.enablementadobe.com` al que se puede dirigir mediante HTTPS con Seguridad de la capa de transporte (TLS).

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

Los pasos de alto nivel son los siguientes:

![Nombre de dominio personalizado con CDN de Adobe](./assets/add-custom-domain-name-with-Adobe-CDN.png){width="800" zoomable="yes"}

## Requisitos previos

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) y [dig](https://www.isc.org/blogs/dns-checker/) están instalados en el equipo local.
- Acceso a servicios de terceros:
   - Autoridad de certificación (CA): para solicitar el certificado firmado para el dominio del sitio, como [DigitCert](https://www.digicert.com/)
   - Servicio de alojamiento del Sistema de nombres de dominio (DNS): para agregar registros DNS para su dominio personalizado, como Azure DNS o AWS Route 53.
- Acceso a [Cloud Manager de Adobe](https://my.cloudmanager.adobe.com/) como **Propietario del negocio** o rol de **Administrador de implementación**.
- AEM El sitio de muestra [WKND](https://github.com/adobe/aem-guides-wknd) se ha implementado en el entorno de AEM as a Cloud Service del tipo [programa de producción](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs).

Si no tiene acceso a servicios de terceros, _colabore con su equipo de seguridad o de alojamiento para completar los pasos_.

## Generar certificado SSL

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Tiene dos opciones:

1. Use la herramienta de línea de comandos `openssl` para generar una clave privada y una solicitud de firma de certificado (CSR) para el dominio del sitio. Para solicitar un certificado firmado, envíe el CSR a una entidad emisora de certificados (CA).
1. El equipo de alojamiento proporciona la clave privada y el certificado firmado necesarios para el sitio.

Revisemos los pasos de la primera opción.

Para generar una clave privada y una CSR, ejecute los siguientes comandos y proporcione la información necesaria cuando se le solicite:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Para solicitar un certificado firmado, proporcione el CSR generado a la CA siguiendo la documentación de la CA. Una vez que la CA firme el CSR, recibirá el archivo de certificado firmado.

### Revisar certificado firmado

Revise el certificado firmado antes de agregarlo a Cloud Manager. Revise los detalles del certificado mediante el siguiente comando:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

El certificado firmado puede contener la cadena de certificados, que incluye los certificados raíz e intermedios junto con el certificado de entidad final.

Adobe Cloud Manager acepta el certificado de entidad final y la cadena de certificado _en campos de formulario independientes_, por lo que debe extraer el certificado de entidad final y la cadena de certificado del certificado firmado.

En este tutorial, se usa como ejemplo el certificado firmado de [DigitCert](https://www.digicert.com/) emitido contra el dominio `*.enablementadobe.com`. La entidad final y la cadena de certificados se extraen abriendo el certificado firmado en un editor de texto y copiando el contenido entre los marcadores `-----BEGIN CERTIFICATE-----` y `-----END CERTIFICATE-----`.

## Añadir un certificado SSL en Cloud Manager

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

Para agregar el certificado SSL en Cloud Manager, siga la documentación de [agregar certificado SSL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate).

## Verificación del nombre del dominio

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

Para verificar el nombre de dominio, siga estos pasos:

- Agregue un nombre de dominio en Cloud Manager siguiendo la [documentación para agregar un nombre de dominio personalizado](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name).
- AEM Agregue un registro [TXT](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record) específico de la en su servicio de hospedaje DNS.
- Compruebe los pasos anteriores consultando los servidores DNS mediante el comando `dig`.

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

La respuesta correcta de ejemplo tiene este aspecto:

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

Este tutorial utiliza Azure DNS, aunque se puede utilizar cualquier proveedor DNS. Para agregar el registro TXT, debe seguir la documentación de su servicio de alojamiento DNS.

Revise la documentación de [comprobación del estado del nombre de dominio](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status) si hay algún problema.

## Configurar el registro DNS

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

Para configurar el registro DNS para el dominio personalizado, siga estos pasos:

1. Determine el tipo de registro DNS (CNAME o APEX) en función del tipo de dominio, como dominio raíz (APEX) o subdominio (CNAME), y siga la documentación de [Configuración de DNS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings).
1. Agregue el registro DNS en su servicio de alojamiento DNS.
1. Almacene en déclencheur la validación del registro DNS siguiendo la [Comprobación del estado del registro DNS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status).

En este tutorial, como se utiliza un **subdominio** `wknd.enablementadobe.com`, se agrega el tipo de registro CNAME que señala a `cdn.adobeaemcloud.com`.

Sin embargo, si está usando el **dominio raíz**, debe agregar un tipo de registro APEX (también conocido como A, ALIAS o ANAME) que apunte a las direcciones IP específicas proporcionadas por Adobe.

## Verificación del sitio

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

Para comprobar que se puede acceder al sitio con el nombre de dominio personalizado, abra un explorador web y vaya a la dirección URL de dominio personalizado. Asegúrese de que el sitio es accesible y el navegador muestra una conexión segura con el icono de candado.

## Vídeo de principio a fin

También puede ver el vídeo completo que muestra la descripción general, los requisitos previos y los pasos anteriores para agregar un nombre de dominio personalizado al sitio alojado en AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)
