---
title: Configuración de extensiones de Reader en AEM Forms OSGi
description: Agregue la credencial Extensiones de Reader al almacén de confianza en AEM Forms OSGi
feature: Reader Extensions
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 328
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 9%

---

# Añadir credencial de extensiones de Reader{#configuring-reader-extension-osgi}

El servicio DocAssurance puede aplicar derechos de uso a los documentos PDF. Para aplicar derechos de uso a documentos PDF, configure los certificados.

## Crear almacén de claves para el usuario de fd-service

La credencial de Extensiones de Reader está asociada al usuario de fd-service. Para agregar la credencial al usuario de fd-service, siga los siguientes pasos. Si ya ha creado el repositorio de claves para el usuario de fd-service, omita esta sección

* AEM Inicie sesión en la instancia de autor de la como administrador
* Vaya a Herramientas-Seguridad-Usuarios
* Desplácese hacia abajo por la lista de usuarios hasta que encuentre la cuenta de usuario de fd-service
* Haga clic en el usuario de fd-service
* Haga clic en la pestaña repositorio de claves
* Haga clic en Crear almacén de claves
* Establezca la Contraseña de acceso a KeyStore y guarde la configuración para crear la contraseña de KeyStore

### Agregar credencial al repositorio de claves de usuario de fd-service

Siga el vídeo para agregar las credenciales al usuario de fd-service

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


El comando para enumerar los detalles del archivo pfx es. El siguiente comando supone que está en el mismo directorio que el archivo pfx

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

Por ejemplo keytool -v -list -storetype pkcs12 -keystore 1005566.pfx donde 1005566.pfx es el nombre de mi archivo pfx
