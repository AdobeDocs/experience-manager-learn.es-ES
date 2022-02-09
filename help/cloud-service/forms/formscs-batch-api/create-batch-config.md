---
title: Configuración de datos por lotes
description: Configuración de datos por lotes
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Crear configuración de lote

Para utilizar una API por lotes, cree una configuración por lotes y ejecute una ejecución basada en esa configuración. El siguiente vídeo muestra una demostración de la creación de la configuración por lotes mediante la API

>[!NOTE]
>Asegúrese de que el usuario AEM pertenece a ```forms-users``` para realizar llamadas de API.


>[!VIDEO](https://video.tv.adobe.com/v/340241/?quality=12&learn=on)

## Crear configuración de lote

El siguiente es el punto final del POST para crear la configuración por lotes

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

## Comprobar configuración de lote

Para comprobar que la configuración del lote se ha creado correctamente, puede realizar una llamada de solicitud de GET al siguiente extremo


```xml
<baseURL>/config/monthlystatements
```

Solo es necesario pasar un objeto JSON vacío en el cuerpo de la solicitud HTTP

