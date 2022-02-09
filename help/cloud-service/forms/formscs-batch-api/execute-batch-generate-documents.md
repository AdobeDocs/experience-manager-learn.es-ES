---
title: Ejecución de la configuración del lote
description: Inicie el proceso de generación de documentos ejecutando el lote
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# Ejecutar configuración de lote

Para ejecutar el lote, realice una solicitud de POST a la siguiente API

```xml
<baseURL>/confi/<configName>/execution
```

Esta API espera un objeto json vacío como parámetro en el cuerpo de la solicitud.
Esta API devuelve una dirección URL única en el encabezado de respuesta identificado por **ubicación** clave.
Una solicitud de GET a esta URL única le indicará el estado de la ejecución por lotes

En el siguiente vídeo se muestra la activación de la configuración por lotes

>[!VIDEO](https://video.tv.adobe.com/v/340242/?quality=12&learn=on)
