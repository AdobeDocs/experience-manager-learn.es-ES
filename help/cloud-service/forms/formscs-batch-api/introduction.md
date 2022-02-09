---
title: Generación de documentos mediante API por lotes en AEM Forms CS
description: Configure y déclencheur operaciones por lotes para generar documentos.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
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

Se recomienda que se familiarice con el [Documentación de API](https://experienceleague.corp.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) antes de seguir utilizando este tutorial.




