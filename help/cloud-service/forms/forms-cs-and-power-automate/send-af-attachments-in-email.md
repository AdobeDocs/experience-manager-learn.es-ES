---
title: Enviar datos adjuntos de formulario en un correo electrónico
description: Extraer y enviar archivos adjuntos de formularios enviados en un correo electrónico mediante el flujo de trabajo de Power Automate
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 0%

---

# Extraer datos adjuntos de formularios enviados

Extraiga los archivos adjuntos del formulario y envíelos en un correo electrónico en el flujo de trabajo de Power Automate.
En el siguiente vídeo se explican los pasos necesarios para formar archivos adjuntos a partir de los datos enviados.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

A continuación se muestra el esquema del objeto attachment que debe utilizar en el paso Analizar esquema JSON

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
