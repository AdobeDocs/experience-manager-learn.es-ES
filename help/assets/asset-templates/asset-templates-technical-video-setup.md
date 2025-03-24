---
title: Configuración de plantillas de recursos con AEM Assets y InDesign Server
description: Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para su uso digital e impreso. La creación de folletos de marketing, tarjetas de presentación, prospectos, anuncios y postales es mucho más sencilla con las plantillas de recursos al integrarse con el servidor de InDesign. La configuración del servidor de InDesign con AEM se trata en esta sección.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Configuración de plantillas de recursos con AEM Assets y InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para su uso digital e impreso. La creación de folletos de marketing, tarjetas de presentación, prospectos, anuncios y postales es mucho más sencilla con las plantillas de recursos al integrarse con el servidor de InDesign. La configuración del servidor de InDesign con AEM se trata en esta sección.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **debe** estar conectado a un servidor InDesign en ejecución cuando se cargue la plantilla INDD. Parte del procesamiento inicial del archivo INDD requiere un servidor InDesign.

## Descargar versión de prueba de InDesign Server {#download-indesign-server-trial}

Descargar [sitio web de descarga de prueba de InDesign Server](https://www.adobeprerelease.com/)

## Inicio de InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
