---
title: Uso de AEM Forms con Adobe Sign
description: Adobe Sign y AEM Forms permiten automatizar transacciones complejas e incluir firmas electrónicas legales como parte de una experiencia digital sin fisuras.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Uso de AEM Forms con Adobe Sign

Adobe Sign permite flujos de trabajo de firma electrónica para formularios adaptables. Las firmas electrónicas mejoran los flujos de trabajo para procesar documentos legales, de ventas, de nómina de pagos, de gestión de recursos humanos y muchas otras áreas.
La integración entre AEM Forms y Adobe Sign le permitirá hacer lo siguiente

* Utilice Forms adaptable para capturar datos y presentar el Documento autogenerado de Record(DoR) para las firmas
* Cree un Forms adaptable basado en su plantilla PDF. Combinar los datos con la plantilla pdf y presentar lo mismo para las firmas
* Envío de documentos para firmar mediante el componente de flujo de trabajo de Documento de firma

## Requisitos previos

Para integrar Adobe Sign con AEM Forms, es necesario lo siguiente:

* Servidor AEM Forms habilitado para SSL
* Cuenta de desarrollador de Adobe Sign activa.
* Una aplicación de API de Adobe Sign
* Credenciales (ID de cliente y Secreto de cliente) de la aplicación API de Adobe Sign.

