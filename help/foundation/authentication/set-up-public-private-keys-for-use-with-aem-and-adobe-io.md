---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM utiliza pares de claves pública y privada para comunicarse de forma segura con Adobe I/O y otros servicios web. Este breve tutorial ilustra cómo se pueden generar claves y almacenes de claves compatibles mediante la herramienta de línea de comandos openssl que funciona tanto con AEM como con Adobe I/O. '
version: 6.4, 6.5
feature: 'Usuarios y grupos '
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '774'
ht-degree: 0%

---


# Configuración de claves públicas y privadas para su uso con Adobe I/O

AEM utiliza pares de claves pública y privada para comunicarse de forma segura con Adobe I/O y otros servicios web. Este breve tutorial ilustra cómo se pueden generar claves y almacenes de claves compatibles mediante la herramienta de línea de comandos [!DNL openssl] que funciona tanto con AEM como con Adobe I/O.

>[!CAUTION]
>
>Esta guía crea claves con firma personal útiles para el desarrollo y uso en entornos más bajos. En los casos de producción, el equipo de seguridad de TI de una organización suele generar y administrar las claves.

## Generación del par de claves pública y privada {#generate-the-public-private-key-pair}

El [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) comando [[!DNL req] de la herramienta de línea de comandos](https://www.openssl.org/docs/man1.0.2/man1/req.html) se puede utilizar para generar un par de claves compatible con Adobe I/O y Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Para completar el comando [!DNL openssl generate], proporcione la información del certificado cuando se solicite. A Adobe I/O y AEM no les importa cuáles sean estos valores; sin embargo, deberían alinearse con y describir su clave.

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

## Añadir par de claves a un nuevo almacén de claves {#add-key-pair-to-a-new-keystore}

Se pueden agregar pares de claves a un nuevo almacén de claves [!DNL PKCS12]. Como parte del comando [[!DNL openssl]'s [!DNL pcks12] ,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) se definen el nombre del almacén de claves (a través de `-  caname`), el nombre de la clave (a través de `-name`) y la contraseña del almacén de claves (a través de `-  passout`).

Estos valores son necesarios para cargar el almacén de claves y las claves en AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

El resultado de este comando es un archivo `keystore.p12`.

>[!NOTE]
>
>Los valores de parámetro de **[!DNL my-keystore]**, **[!DNL my-key]** y **[!DNL my-password]** se reemplazarán por sus propios valores.

## Verificar el contenido del almacén de claves {#verify-the-keystore-contents}

La herramienta de línea de comandos [[!DNL keytool] Java](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) proporciona visibilidad en un almacén de claves para garantizar que las claves se carguen correctamente en el archivo de almacén de claves ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Verificar el almacén de claves en Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Añadir el almacén de claves a AEM {#adding-the-keystore-to-aem}

AEM utiliza la **clave privada** generada para comunicarse de forma segura con Adobe I/O y otros servicios web. Para que AEM pueda acceder a la clave privada, esta debe instalarse en el almacén de claves de un usuario de AEM.

Vaya a **AEM > [!UICONTROL Herramientas] > [!UICONTROL Seguridad] > [!UICONTROL Usuarios]** y **edite el usuario** con el que se va a asociar la clave privada.

### Crear un almacén de claves de AEM {#create-an-aem-keystore}

![Crear KeyStore en ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM >  [!UICONTROL Herramientas]  >  [!UICONTROL Seguridad]  >  [!UICONTROL Usuarios]  > Editar usuario*

Si se le solicita que cree un almacén de claves, hágalo. Este almacén de claves solo existirá en AEM y NO es el almacén de claves creado mediante openssl. La contraseña puede ser cualquier cosa y no tiene por qué ser la misma que la contraseña utilizada en el comando [!DNL openssl].

### Instale la clave privada a través del almacén de claves {#install-the-private-key-via-the-keystore}

![Añadir clave privada en ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL AEMUser]  >  [!UICONTROL Almacén de claves]  >  [!UICONTROL Añadir clave privada del almacén de claves]*

En la consola del almacén de claves del usuario, haga clic en **[!UICONTROL Agregar clave privada del archivo KeyStore]** y agregue la siguiente información:

* **[!UICONTROL Nuevo alias]**: alias de la clave en AEM. Esto puede ser cualquier cosa y no tiene que corresponder con el nombre del almacén de claves creado con el comando openssl.
* **[!UICONTROL Archivo]** KeyStore: la salida del comando openssl pkcs12 (keystore.p12)
* **[!UICONTROL Contraseña]** del archivo KeyStore: La contraseña establecida en el comando openssl pkcs12 mediante  `-passout` argumento .
* **[!UICONTROL Alias]** de clave privada: El valor proporcionado al  `-name` argumento en el comando openssl pkcs12 anterior (es decir,  `my-key`).
* **[!UICONTROL Contraseña]** de clave privada: La contraseña establecida en el comando openssl pkcs12 mediante  `-passout` argumento .

>[!CAUTION]
>
>La contraseña del archivo KeyStore y la contraseña de clave privada son las mismas para ambas entradas. Si se introduce una contraseña que no coincide, la clave no se importa.

### Compruebe que la clave privada se carga en el almacén de claves de AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Verificar clave privada en ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL AEMUser]  >  [!UICONTROL Almacén de claves]*

Cuando la clave privada se carga correctamente desde el almacén de claves proporcionado al almacén de claves de AEM, los metadatos de la clave privada se muestran en la consola del almacén de claves del usuario.

## Adición de la clave pública a Adobe I/O {#adding-the-public-key-to-adobe-i-o}

La clave pública coincidente debe cargarse en Adobe I/O para permitir que el usuario del servicio AEM, que tiene la clave pública correspondiente privada para comunicarse de forma segura.

### Crear una nueva integración de Adobe I/O {#create-a-adobe-i-o-new-integration}

![Crear una nueva integración de Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Crear integración de Adobe I/O]](https://console.adobe.io/)  >  [!UICONTROL Nueva integración]*

Para crear una nueva integración en Adobe I/O es necesario cargar un certificado público. Cargue el **certificate.crt** generado por el comando `openssl req`.

### Compruebe que las claves públicas se cargan en Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Comprobación de claves públicas en Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Las claves públicas instaladas y sus fechas de caducidad se enumeran en la consola [!UICONTROL Integrations] de Adobe I/O. Se pueden agregar varias claves públicas mediante el botón **[!UICONTROL Add a public key]**.

Ahora, AEM tiene la clave privada y la integración de Adobe I/O contiene la clave pública correspondiente, lo que permite a AEM comunicarse de forma segura con Adobe I/O.
