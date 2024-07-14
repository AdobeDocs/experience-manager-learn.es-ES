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
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '81'
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
