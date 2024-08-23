---
title: Ejecutar la configuración por lotes
description: Inicie el proceso de generación de documentos ejecutando el lote
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# Ejecutar configuración por lotes

Para ejecutar el lote, realice una solicitud de POST a la siguiente API

```xml
<baseURL>/confi/<configName>/execution
```

Esta API espera un objeto json vacío como parámetro en el cuerpo de la solicitud.
Esta API devuelve una dirección URL única en el encabezado de respuesta identificado por la clave **location**.
Una solicitud de GET a esta URL única le indicará el estado de ejecución del lote

El siguiente vídeo muestra la activación de la configuración por lotes

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
