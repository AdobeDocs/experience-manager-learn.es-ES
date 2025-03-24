---
title: Configurar la configuración de datos por lotes
description: Configurar la configuración de datos por lotes
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 233
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 12%

---

# Crear configuración por lotes

Para utilizar un API por lotes, cree una configuración por lotes y realice una ejecución basada en esa configuración. El siguiente vídeo muestra una demostración de la creación de la configuración por lotes mediante la API

>[!NOTE]
>Asegúrese de que el usuario de AEM pertenezca al grupo ```forms-users``` para realizar llamadas a la API.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## Crear configuración por lotes

El siguiente es el punto final POST para crear la configuración por lotes

```xml
<baseURL>/config
```

La siguiente es la configuración mínima que debe especificarse al crear la configuración por lotes. Esto debe pasarse como objeto JSON en el cuerpo de la solicitud HTTP

```
{
    "configName": "monthlystatements",
    "dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
    "outputTypes": [
        "PDF"
    ],
    "template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## Verificar configuración del lote

Para comprobar que la configuración por lotes se ha creado correctamente, puede realizar una llamada de petición GET al siguiente extremo


```xml
<baseURL>/config/monthlystatements
```

Solo es necesario pasar un objeto JSON vacío en el cuerpo de la solicitud HTTP
