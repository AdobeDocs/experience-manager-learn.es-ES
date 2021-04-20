---
title: Configuración de plantillas de recursos con AEM Assets e InDesign Server
description: Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para impresión y dispositivos digitales. La creación de folletos de marketing, tarjetas de visita, folletos, anuncios y tarjetas postales es mucho más fácil con las plantillas de recursos al integrarse con el servidor de InDesign. La configuración del servidor de InDesign con AEM se explica en esta sección.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 1%

---


# Configuración de plantillas de recursos con AEM Assets e InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para impresión y dispositivos digitales. La creación de folletos de marketing, tarjetas de visita, folletos, anuncios y tarjetas postales es mucho más fácil con las plantillas de recursos al integrarse con el servidor de InDesign. La configuración del servidor de InDesign con AEM se explica en esta sección.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **debe** estar conectado a un servidor InDesign en ejecución cuando se carga la plantilla INDD. Parte del procesamiento inicial en el archivo INDD requiere el servidor InDesign.

## Descargar la versión de prueba de InDesign Server {#download-indesign-server-trial}

Descargar [Sitio web de descarga de prueba de InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## Inicio de InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
