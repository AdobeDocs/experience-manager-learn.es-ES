---
title: Marcas de agua en AEM Assets
description: AEM como función de marca de agua de Cloud Service permite que las representaciones de imágenes personalizadas se marquen con agua con cualquier imagen PNG.
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# Marcas de agua

AEM como función de marca de agua de Cloud Service permite que las representaciones de imágenes personalizadas se marquen con agua con cualquier imagen PNG.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## Configuración OSGi

El siguiente código auxiliar de configuración OSGi se puede actualizar y agregar al subproyecto `ui.config` del proyecto de AEM.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
