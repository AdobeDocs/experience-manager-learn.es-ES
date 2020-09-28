---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM utiliza pares de claves públicas y privadas para comunicarse de forma segura con la E/S de Adobe y otros servicios Web. Este breve tutorial ilustra cómo se pueden generar claves y almacenes de claves compatibles con la herramienta de línea de comandos openssl que funciona con E/S de AEM y Adobe. '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---


# Configuración de claves públicas y privadas para su uso con E/S de Adobe

AEM utiliza pares de claves públicas y privadas para comunicarse de forma segura con la E/S de Adobe y otros servicios Web. Este breve tutorial ilustra cómo se pueden generar claves y almacenes de claves compatibles con la herramienta de línea de comandos que funciona con la E/S de AEM y Adobe. [!DNL openssl]

>[!CAUTION]
>
>Esta guía crea claves con firma personal útiles para el desarrollo y uso en entornos inferiores. En los escenarios de producción, las claves generalmente son generadas y administradas por el equipo de seguridad de TI de una organización.

## Generación del par de claves pública y privada {#generate-the-public-private-key-pair}

El [comando](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) de la herramienta de línea de comandos [[!DNL req] [!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/req.html) se puede utilizar para generar un par de claves compatible con la E/S de Adobe y Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Para completar el [!DNL openssl generate] comando, proporcione la información del certificado cuando se solicite. A la E/S de Adobe y a AEM no les importa cuáles sean estos valores, sin embargo, deberían alinearse con la clave y describirla.

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## Añadir par de claves en un nuevo almacén de claves {#add-key-pair-to-a-new-keystore}

Los pares de claves se pueden agregar a un nuevo [!DNL PKCS12] almacén de claves. Como parte del [[!DNL openssl]'s [!DNL pcks12] comando,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) se define el nombre del almacén de claves (mediante `-  caname`), el nombre de la clave (mediante `-name`) y la contraseña del almacén de claves (mediante `-  passout`).

Estos valores son necesarios para cargar el almacén de claves y las claves en AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

El resultado de este comando es un `keystore.p12` archivo.

>[!NOTE]
>
>Los valores de parámetro de **[!DNL my-keystore]**, **[!DNL my-key]** y **[!DNL my-password]** se reemplazarán por sus propios valores.

## Verificar el contenido del almacén de claves {#verify-the-keystore-contents}

La herramienta [[!DNL keytool] de línea de](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) comandos Java proporciona visibilidad en un almacén de claves para garantizar que las claves se carguen correctamente en el archivo de almacén de claves ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Verificación del almacén de claves en E/S de Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Añadir el almacén de claves a AEM {#adding-the-keystore-to-aem}

AEM utiliza la clave **** privada generada para comunicarse de forma segura con la E/S de Adobe y otros servicios Web. Para que la clave privada sea accesible para AEM, debe instalarse en el almacén de claves de un usuario AEM.

Vaya a **AEM >[!UICONTROL Herramientas]>[!UICONTROL Seguridad]>[!UICONTROL Usuarios]** y **edite el usuario** con el que se asociará la clave privada.

### Creación de un almacén de claves de AEM {#create-an-aem-keystore}

![Crear KeyStore en AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)*AEM >[!UICONTROL Herramientas]>[!UICONTROL Seguridad]>[!UICONTROL Usuarios]> Editar usuario*

Si se le solicita que cree un almacén de claves, hágalo. Este almacén de claves sólo existirá en AEM y NO es el almacén de claves creado mediante openssl. La contraseña puede ser cualquier cosa y no tiene por qué ser la misma que la contraseña utilizada en el [!DNL openssl] comando.

### Instalar la clave privada mediante el almacén de claves {#install-the-private-key-via-the-keystore}

![Añadir clave privada en AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)*[!UICONTROL Usuario]>[!UICONTROL Almacén]de claves >[!UICONTROL Añadir clave privada del almacén de claves]*

En la consola del almacén de claves del usuario, haga clic en **[!UICONTROL Añadir clave privada del archivo]** KeyStore y agregue la siguiente información:

* **[!UICONTROL Nuevo alias]**: alias de la clave en AEM. Esto puede ser cualquier cosa y no tiene por qué corresponderse con el nombre del almacén de claves creado con el comando openssl.
* **[!UICONTROL Archivo]** Keystore: la salida del comando openssl pkcs12 (keystore.p12)
* **[!UICONTROL Alias]** de clave privada: La contraseña establecida en el comando openssl pkcs12 mediante `-  passout` argumento.

* **[!UICONTROL Contraseña]** de clave privada: La contraseña establecida en el comando openssl pkcs12 mediante `-  passout` argumento.

### Verifique que la clave privada esté cargada en el almacén de claves de AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Verificar clave privada en AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)*[!UICONTROL usuario]>[!UICONTROL almacén de claves]*

Cuando la clave privada se carga correctamente desde el almacén de claves proporcionado al almacén de claves de AEM, los metadatos de la clave privada se muestran en la consola del almacén de claves del usuario.

## Añadir la clave pública para E/S de Adobe {#adding-the-public-key-to-adobe-i-o}

La clave pública coincidente debe cargarse en la E/S de Adobe para permitir que el usuario del servicio de AEM, que tiene la clave pública correspondiente privada, se comunique de forma segura.

### Crear una nueva integración de E/S Adobe {#create-a-adobe-i-o-new-integration}

![Crear una nueva integración de E/S Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Crear integración]](https://console.adobe.io/)de E/S de Adobe >[!UICONTROL Nueva integración]*

La creación de una nueva integración en la E/S de Adobe requiere la carga de un certificado público. Cargue **certificate.crt** generado por el `openssl req` comando.

### Verifique que las claves públicas se carguen en la E/S de Adobe {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Verificar claves públicas en E/S de Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Las claves públicas instaladas y sus fechas de caducidad se enumeran en la consola [!UICONTROL Integrations] en la E/S de Adobe. Se pueden agregar varias claves públicas mediante el botón **[!UICONTROL Añadir una clave]** pública.

Ahora AEM la clave privada y la integración de E/S de Adobe contiene la clave pública correspondiente, lo que permite AEM comunicarse de forma segura con E/S de Adobe.
