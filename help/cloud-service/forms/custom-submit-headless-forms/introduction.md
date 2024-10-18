---
title: Enviar formularios sin encabezado a un servicio de envío personalizado
description: Personalice su respuesta en función de los datos enviados
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 78fe677c-d5ab-40f6-a381-800f24e227ae
duration: 27
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 2%

---

# Personalizar respuesta en función de los datos enviados

Una vez enviado el formulario, es importante proporcionar comentarios al usuario sobre el resultado del envío. La respuesta de envío puede incluir un ID de transacción o simplemente una respuesta personalizada. Para satisfacer este caso de uso, se escribe un servicio de envío personalizado en AEM Forms y el formulario sin encabezado se envía a este servicio de envío personalizado.

## Requisitos previos

Para implementar correctamente esta funcionalidad, se recomienda estar familiarizado con lo siguiente

* Experiencia con Git
* Experiencia con AEM Cloud Manager
* Maven (este artículo se ha probado con 3.8.6)
* Instancia de autor local preparada para AEM Forms Cloud
* Acceso a AEM Forms como entorno de Cloud Service
* IntelliJ o cualquier otro IDE


## Siguientes pasos

[Escribir el servicio de envío personalizado](./custom-submit-service.md)
