---
title: Uso de AEM Forms con Adobe Sign
description: Adobe Sign y AEM Forms permiten automatizar transacciones complejas e incluir firmas electrónicas legales como parte de una experiencia digital sin fisuras.
feature: formularios adaptables
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# Uso de AEM Forms con Adobe Sign

Adobe Sign habilita los flujos de trabajo de firma electrónica para los formularios adaptables. Las firmas electrónicas mejoran los flujos de trabajo para procesar documentos para áreas legales, de ventas, de nómina, de gestión de recursos humanos y muchas otras áreas.
La integración entre AEM Forms y Adobe Sign le permitirá hacer lo siguiente

* Utilice los formularios adaptables para capturar datos y presentar el documento de registro generado automáticamente (DoR) para las firmas
* Cree formularios adaptables basados en la plantilla PDF. Combine los datos con la plantilla pdf y presente lo mismo para las firmas
* Envío de documentos para firmar mediante el componente de flujo de trabajo Firmar documento

## Requisitos previos

Para integrar Adobe Sign con AEM Forms, es necesario lo siguiente:

* Servidor de AEM Forms habilitado para SSL
* Una cuenta de desarrollador activa de Adobe Sign.
* Una aplicación de API de Adobe Sign
* Credenciales (ID de cliente y Secreto de cliente) de la aplicación de API de Adobe Sign.

