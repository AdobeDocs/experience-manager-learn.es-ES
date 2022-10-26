---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: AEM utiliza pares de claves pública y privada para comunicarse de forma segura con el Adobe I/O y otros servicios web. Este breve tutorial ilustra cómo se pueden generar claves y almacenes de claves compatibles con la herramienta de línea de comandos openssl que funciona tanto con AEM como con Adobe I/O.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Development
role: Developer
level: Experienced
exl-id: 62ed9dcc-6b8a-48ff-8efe-57dabdf4aa66
last-substantial-update: 2022-07-17T00:00:00Z
thumbnail: KT-2450.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---

# Configuración de claves públicas y privadas para su uso con Adobe I/O

AEM utiliza pares de claves pública y privada para comunicarse de forma segura con el Adobe I/O y otros servicios web. Este breve tutorial ilustra cómo se pueden generar claves y almacenes de claves compatibles mediante la función [!DNL openssl] herramienta de línea de comandos que funciona tanto con AEM como con Adobe I/O.

>[!CAUTION]
>
>Esta guía crea claves con firma personal útiles para el desarrollo y uso en entornos más bajos. En los casos de producción, el equipo de seguridad de TI de una organización suele generar y administrar las claves.

## Generación del par de claves pública y privada {#generate-the-public-private-key-pair}

La variable [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) de la herramienta de línea de comandos [[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html) se puede utilizar para generar un par de claves compatible con Adobe I/O y Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Para completar la [!DNL openssl generate] proporcione la información del certificado cuando se solicite. A Adobe I/O y AEM no les importa cuáles sean estos valores, sin embargo, deberían alinearse con y describir su clave.

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

Los pares de claves se pueden agregar a una nueva [!DNL PKCS12] almacén de claves. Como parte de [[!DNL openssl]'s [!DNL pcks12] comando,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) el nombre del almacén de claves (mediante `-  caname`), el nombre de la clave (mediante `-name`) y la contraseña del almacén de claves (mediante `-  passout`).

Estos valores son necesarios para cargar el almacén de claves y las claves en AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

El resultado de este comando es un `keystore.p12` archivo.

>[!NOTE]
>
>Los valores de parámetro de **[!DNL my-keystore]**, **[!DNL my-key]** y **[!DNL my-password]** se reemplazarán por sus propios valores.

## Verificar el contenido del almacén de claves {#verify-the-keystore-contents}

Java™ [[!DNL keytool] herramienta línea de comandos](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) proporciona visibilidad en un almacén de claves para garantizar que las claves se cargan correctamente en el archivo del almacén de claves ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Verificar el almacén de claves en el Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Añadir el almacén de claves a AEM {#adding-the-keystore-to-aem}

AEM utiliza el **clave privada** para comunicarse de forma segura con Adobe I/O y otros servicios web. Para que la clave privada sea accesible para AEM, debe instalarse en el almacén de claves de un usuario AEM.

Vaya a **AEM > [!UICONTROL Herramientas] > [!UICONTROL Seguridad] > [!UICONTROL Usuarios]** y **editar el usuario** la clave privada se va a asociar.

### Crear un almacén de claves AEM {#create-an-aem-keystore}

![Crear KeyStore en AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM > [!UICONTROL Herramientas] > [!UICONTROL Seguridad] > [!UICONTROL Usuarios] > Editar usuario*

Cuando se le pida que cree un almacén de claves, hágalo. Este almacén de claves solo existe en AEM y NO es el almacén de claves creado mediante openssl. La contraseña puede ser cualquier cosa y no tiene por qué ser la misma que la contraseña utilizada en la [!DNL openssl] comando.

### Instale la clave privada a través del almacén de claves {#install-the-private-key-via-the-keystore}

![Añadir clave privada en AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL Usuario] > [!UICONTROL Almacén de claves] > [!UICONTROL Añadir clave privada del almacén de claves]*

En la consola del almacén de claves del usuario, haga clic en **[!UICONTROL Agregar clave privada del archivo KeyStore]** y añada la siguiente información:

* **[!UICONTROL Nuevo alias]**: alias de la clave en AEM. Esto puede ser cualquier cosa y no tiene que corresponder con el nombre del almacén de claves creado con el comando openssl.
* **[!UICONTROL Archivo KeyStore]**: la salida del comando openssl pkcs12 (keystore.p12)
* **[!UICONTROL Contraseña del archivo KeyStore]**: La contraseña establecida en el comando openssl pkcs12 mediante `-passout` argumento.
* **[!UICONTROL Alias de clave privada]**: El valor proporcionado a la variable `-name` en el comando openssl pkcs12 anterior (p. ej. `my-key`).
* **[!UICONTROL Contraseña de clave privada]**: La contraseña establecida en el comando openssl pkcs12 mediante `-passout` argumento.

>[!CAUTION]
>
>La contraseña del archivo KeyStore y la contraseña de clave privada son las mismas para ambas entradas. Si se introduce una contraseña que no coincide, la clave no se importa.

### Compruebe que la clave privada se carga en el almacén de claves de AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Verificar clave privada en AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL Usuario] > [!UICONTROL Almacén de claves]*

Cuando la clave privada se carga correctamente desde el almacén de claves proporcionado al almacén de claves de AEM, los metadatos de la clave privada se muestran en la consola del almacén de claves del usuario.

## Adición de la clave pública al Adobe I/O {#adding-the-public-key-to-adobe-i-o}

La clave pública coincidente debe cargarse en el Adobe I/O para permitir que el usuario del servicio de AEM, que tiene la clave pública correspondiente como privada, se comunique de forma segura.

### Crear una nueva integración de Adobe I/O {#create-a-adobe-i-o-new-integration}

![Crear una nueva integración de Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Crear integración de Adobe I/O]](https://developer.adobe.com/console/) > [!UICONTROL Nueva integración]*

La creación de una integración en Adobe I/O requiere la carga de un certificado público. Cargue el **certificate.crt** generado por el `openssl req` comando.

### Compruebe que las claves públicas se cargan en el Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Verificar claves públicas en el Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Las claves públicas instaladas y sus fechas de caducidad se enumeran en la variable [!UICONTROL Integraciones] en el Adobe I/O. Se pueden agregar varias claves públicas a través de la variable **[!UICONTROL Adición de una clave pública]** botón.

Ahora AEM contiene la clave privada y la integración de Adobe I/O contiene la clave pública correspondiente, lo que permite AEM comunicarse de forma segura con el Adobe I/O.
