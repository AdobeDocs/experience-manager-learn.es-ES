---
title: Uso de AEM Forms con Acrobat Sign
description: Acrobat Sign y AEM Forms permiten automatizar transacciones complejas e incluir firmas electrónicas legales como parte de una experiencia digital sin fisuras.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 11%

---

# Uso de AEM Forms con Acrobat Sign

Acrobat Sign permite los flujos de trabajo de firma electrónica para formularios adaptables. Las firmas electrónicas mejoran los flujos de trabajo para procesar documentos para el área legal, ventas, nóminas, administración de recursos humanos y muchas más.
La integración entre AEM Forms y Acrobat Sign le permitirá hacer lo siguiente

* Utilice Forms adaptable para capturar datos y presentar el documento de registro generado automáticamente (DoR) para las firmas
* Cree Forms adaptable en función de la plantilla de PDF. Combine los datos con la plantilla pdf y presente lo mismo para las firmas
* Envío de documentos para firmar mediante el componente de flujo de trabajo Firmar documento

## Requisitos previos

Para integrar Acrobat Sign con AEM Forms, es necesario lo siguiente:

* Servidor AEM Forms habilitado para SSL
* Una cuenta de desarrollador de Acrobat Sign activa.
* Una aplicación de API de Acrobat Sign
* Credenciales (ID de cliente y Secreto de cliente) de la aplicación de API de Acrobat Sign.
