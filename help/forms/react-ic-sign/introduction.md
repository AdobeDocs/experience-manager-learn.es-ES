---
title: Aplicación React con AEM Forms y Acrobat Sign
description: Acrobat Sign y AEM Forms permiten automatizar transacciones complejas e incluir firmas electrónicas legales como parte de una experiencia digital sin fisuras.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 31
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# AEM Forms con formulario web de Acrobat Sign


Este tutorial lo acompañará en el caso de uso de generar un documento de comunicación interactiva con los datos enviados desde la aplicación [React](https://react.dev/) y presentar el documento generado para su firma mediante el formulario web de Acrobat Sign.

A continuación se muestra el flujo del caso de uso

* El usuario rellena un formulario en la aplicación React.
* Los datos del formulario se envían a un extremo de AEM Forms para generar un documento de comunicaciones interactivas.
* Cree la URL del widget de Acrobat Sign con el documento generado.
* Presente la URL del widget a la aplicación de llamada para que el usuario firme el documento.

## Requisitos previos

Necesitará lo siguiente para que funcione el caso de uso:

* AEM Un servidor de con el paquete de complementos de Forms
* Una clave de integración [para una aplicación de Acrobat Sign](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## Siguientes pasos

Escriba un servicio OSGi [personalizado para generar un documento de comunicación interactiva](./create-ic-document.md) mediante una API documentada
