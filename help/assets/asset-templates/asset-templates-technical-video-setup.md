---
title: Configuración de plantillas de recursos con AEM Assets y InDesign Server
description: Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para impresión y edición digital. La creación de folletos de marketing, tarjetas de presentación, volantes, anuncios y tarjetas postales resulta mucho más fácil con las plantillas de recursos cuando se integran con el servidor de InDesign. La configuración del servidor de InDesign con AEM se trata en esta sección.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# Configure las plantillas de recursos con AEM Assets y InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Las plantillas de recursos permiten a los especialistas en marketing crear, administrar y distribuir recursos digitales para impresión y edición digital. La creación de folletos de marketing, tarjetas de presentación, volantes, anuncios y tarjetas postales resulta mucho más fácil con las plantillas de recursos cuando se integran con el servidor de InDesign. La configuración del servidor de InDesign con AEM se trata en esta sección.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **debe** estar conectado a un servidor de InDesign en ejecución cuando se cargue la plantilla INDD. Parte del procesamiento inicial en el archivo INDD requiere el servidor de InDesign.

## Descargar prueba de InDesign Server {#download-indesign-server-trial}

Descargar [sitio Web de descarga de prueba de InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## Iniciando InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
