---
title: Configuración de extensiones de Reader en AEM Forms OSGi
description: Agregar credenciales de extensiones de Reader al almacén de confianza en AEM Forms OSGi
feature: Extensiones de Reader
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administración
role: Admin
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---


# Agregar credenciales de extensiones de Reader{#configuring-reader-extension-osgi}

El servicio DocAssurance puede aplicar derechos de uso a documentos PDF. Para aplicar derechos de uso a documentos PDF, configure los certificados.

## Crear almacén de claves para usuario de fd-service

La credencial de extensiones de lector está asociada al usuario de fd-service. Para agregar la credencial al usuario del servicio fd, siga los siguientes pasos. Si ya ha creado el almacén de claves para el usuario del servicio fd, omita esta sección

* Inicie sesión en la instancia de autor de AEM como administrador
* Vaya a Herramientas-Seguridad-Usuarios
* Desplácese hacia abajo por la lista de usuarios hasta que encuentre la cuenta de usuario de fd-service
* Haga clic en el usuario del fd-service
* Haga clic en la ficha almacén de claves
* Haga clic en Crear KeyStore
* Establezca la contraseña de acceso de KeyStore y guarde la configuración para crear la contraseña de KeyStore

### Agregar credenciales al almacén de claves del usuario del fd-service

Siga el vídeo para añadir las credenciales al usuario del fd-service

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)


El comando para enumerar los detalles del archivo pfx es . El siguiente comando supone que está en el mismo directorio que el archivo pfx .

**keytool -v -list -storetype pkcs12 -keystore  &lt;name of=&quot;&quot; your=&quot;&quot;>**

Por ejemplo keytool -v -list -storetype pkcs12 -keystore 1005566.pfx donde 1005566.pfx es el nombre de mi archivo pfx













