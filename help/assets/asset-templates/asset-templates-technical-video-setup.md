---
title: Configuración de plantillas de recursos con AEM Assets y InDesign Server
description: Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para impresión y dispositivos digitales. La creación de folletos de marketing, tarjetas de visita, folletos, anuncios y tarjetas postales es mucho más fácil con las plantillas de recursos al integrarse con el servidor de InDesign. La configuración del servidor de InDesign con AEM se explica en esta sección.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# Configuración de plantillas de recursos con AEM Assets y InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para impresión y dispositivos digitales. La creación de folletos de marketing, tarjetas de visita, folletos, anuncios y tarjetas postales es mucho más fácil con las plantillas de recursos al integrarse con el servidor de InDesign. La configuración del servidor de InDesign con AEM se explica en esta sección.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **debe** conectarse a un servidor de InDesign en ejecución cuando se cargue la plantilla INDD. Parte del procesamiento inicial en el archivo INDD requiere el servidor de InDesign.

## Descargar versión de prueba de InDesign Server {#download-indesign-server-trial}

Descargar [Sitio Web de descarga de prueba de InDesign Server](https://www.adobe.com/devnet/premiere/sdk/cs5/indesign-server-trial-downloads.html)

## Inicio del InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
