---
title: Marcas de agua en AEM Assets
description: Las funciones de marca de agua de AEM as a Cloud Service permiten que las representaciones de imágenes personalizadas se marquen con agua con cualquier imagen PNG.
feature: Asset Compute Microservices
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 4%

---


# Marcas de agua

Las funciones de marca de agua de AEM as a Cloud Service permiten que las representaciones de imágenes personalizadas se marquen con agua con cualquier imagen PNG.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## Configuración de OSGi

El siguiente código auxiliar de configuración de OSGi se puede actualizar y agregar al subproyecto `ui.config` del proyecto de AEM.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
