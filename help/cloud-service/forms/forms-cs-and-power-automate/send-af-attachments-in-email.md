---
title: Enviar archivos adjuntos en un mensaje de correo electrónico
description: Extraer y enviar archivos adjuntos de formularios enviados en un mensaje de correo electrónico mediante la automatización del flujo de trabajo
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: aea43a705b3959f8be26238d32b816b3953e153e
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Extraer datos adjuntos de formularios enviados

Extraiga los archivos adjuntos de los formularios y envíelos en un mensaje de correo electrónico en Power Automatice el flujo de trabajo.
En el siguiente vídeo se explican los pasos necesarios para formar archivos adjuntos a partir de los datos enviados.
>[!VIDEO](https://video.tv.adobe.com/v/3409017/?quality=12&learn=on)

El siguiente es el esquema de objeto de adjunto que debe utilizar en el paso de esquema Parse JSON

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
