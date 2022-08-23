---
title: Configuración de plantillas de recursos con AEM Assets y InDesign Server
description: Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para impresión y dispositivos digitales. La creación de folletos de marketing, tarjetas de visita, folletos, anuncios y tarjetas postales es mucho más fácil con las plantillas de recursos al integrarse con el servidor de InDesign. La configuración del servidor de InDesign con AEM se explica en esta sección.
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Configuración de plantillas de recursos con AEM Assets y InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para impresión y dispositivos digitales. La creación de folletos de marketing, tarjetas de visita, folletos, anuncios y tarjetas postales es mucho más fácil con las plantillas de recursos al integrarse con el servidor de InDesign. La configuración del servidor de InDesign con AEM se explica en esta sección.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **must** estar conectado a un servidor de InDesign en ejecución cuando se cargue la plantilla INDD. Parte del procesamiento inicial en el archivo INDD requiere el servidor de InDesign.

## Descargar versión de prueba de InDesign Server {#download-indesign-server-trial}

Descargar [Sitio web de descarga de prueba de InDesign Server](https://www.adobeprerelease.com/)

## Inicio del InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
