---
title: Marcas de agua en AEM Assets
description: AEM las funciones de marca de agua de un Cloud Service permiten que las representaciones de imágenes personalizadas se marquen con agua con cualquier imagen PNG.
feature: Asset Compute Microservices
version: Cloud Service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
exl-id: 252c7c58-3567-440a-a1d5-19c598b6788e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---

# Marcas de agua

AEM las funciones de marca de agua de un Cloud Service permiten que las representaciones de imágenes personalizadas se marquen con agua con cualquier imagen PNG.

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
