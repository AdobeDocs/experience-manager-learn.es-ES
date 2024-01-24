---
title: Crear interfaz de servicio
description: Defina los métodos en la interfaz que desee exponer
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7825.jpg
jira: KT-7825
exl-id: f262013b-aaf1-43d1-84b8-6173942c3415
duration: 8
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 4%

---


# Interfaz

Cree una interfaz con las dos definiciones de método siguientes.

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {	
	public Document getPDF(String location,String accessToken,String fileName);
	
    public Document createPDFFromInputStream(InputStream is,String fileName);
}
```
