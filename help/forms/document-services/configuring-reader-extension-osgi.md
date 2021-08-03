---
title: Configuración de extensiones de Reader en AEM Forms OSGi
description: Agregar credenciales de extensiones de Reader al almacén de confianza en AEM Forms OSGi
feature: Extensiones de Reader
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: Administración
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
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











