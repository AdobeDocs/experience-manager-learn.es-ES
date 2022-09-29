---
title: Generación de documentos mediante API por lotes en AEM Forms CS
description: Configure y déclencheur operaciones por lotes para generar documentos.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Introducción

En una solicitud por lotes se generan decenas, cientos o miles de documentos similares al mismo tiempo. Ejemplo: Una empresa financiera puede generar extractos de tarjetas de crédito para enviarlos a todos sus clientes.
Las API por lotes (API asíncronas) son adecuadas para casos de uso programados de alto rendimiento y de generación de documentos. Estas API generan documentos por lotes. Por ejemplo, facturas telefónicas, extractos de tarjetas de crédito y extractos de beneficios generados cada mes.

Para utilizar la API de operación por lotes de AEM Forms CS, se necesitan las siguientes configuraciones

1. Configurar la cuenta de almacenamiento de Azure
1. Crear configuración de nube respaldada por almacenamiento de Azure
1. Crear configuración del almacén de datos por lotes
1. Ejecución de la API por lotes

Se recomienda que se familiarice con el [Documentación de API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) antes de seguir utilizando este tutorial.
