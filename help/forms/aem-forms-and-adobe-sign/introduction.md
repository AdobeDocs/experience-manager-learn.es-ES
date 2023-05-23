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

Acrobat Sign habilita los flujos de trabajo de firma electrónica para formularios adaptables. Las firmas electrónicas mejoran los flujos de trabajo para procesar documentos para el área legal, ventas, nóminas, administración de recursos humanos y muchas más.
La integración entre AEM Forms y Acrobat Sign le permite hacer lo siguiente

* Utilizar Forms adaptable para capturar datos y presentar el documento de registro (DoR) generado automáticamente para las firmas.
* Cree un Forms adaptable basado en la plantilla de PDF de. Combine los datos con la plantilla pdf y presente la misma plantilla para las firmas
* Enviar documentos para firmar mediante el componente de flujo de trabajo Firmar documento

## Requisitos previos

Para integrar Acrobat Sign con AEM Forms, es necesario lo siguiente:

* Un servidor AEM Forms habilitado para SSL
* Una cuenta de desarrollador de Acrobat Sign activa.
* Una aplicación API de Acrobat Sign
* Las credenciales (ID de cliente y Secreto de cliente) de la aplicación API de Acrobat Sign.
