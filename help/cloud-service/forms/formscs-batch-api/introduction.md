---
title: Generación de documentos mediante API por lotes en AEM Forms CS
description: Configurar y almacenar en déclencheur operaciones por lotes para generar documentos.
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
ht-degree: 24%

---

# Introducción

Una solicitud de lote es donde se generan decenas, cientos o miles de documentos similares a la vez. Ejemplo: una empresa financiera puede generar extractos de tarjetas de crédito para enviarlos a todos sus clientes.
Las API por lotes (API asíncronas) son adecuadas para casos de uso planificados de alto rendimiento y de generación de múltiples documentos. Estas API generan documentos por lotes. Por ejemplo, facturas telefónicas, extractos de tarjetas de crédito y declaraciones de beneficios generados cada mes.

Para utilizar la API de operación por lotes de AEM Forms CS, se necesitan las siguientes configuraciones

1. Configurar la cuenta de almacenamiento de Azure
1. Crear configuración de nube respaldada por almacenamiento de Azure
1. Crear configuración del almacén de datos por lotes
1. Ejecución de la API por lotes

Se recomienda familiarizarse con el [Documentación de API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) antes de continuar con este tutorial.
