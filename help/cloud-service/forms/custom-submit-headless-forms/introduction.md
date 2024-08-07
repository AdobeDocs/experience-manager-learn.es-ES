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
exl-id: 78fe677c-d5ab-40f6-a381-800f24e227ae
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '135'
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
