---
title: Reaccione la aplicación con AEM Forms y Acrobat Sign
description: Acrobat Sign y AEM Forms permiten automatizar transacciones complejas e incluir firmas electrónicas legales como parte de una experiencia digital sin fisuras.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# AEM Forms con formulario web de Acrobat Sign


Este tutorial le guía por el caso de uso de la generación de un documento de comunicación interactivo con los datos enviados desde el [React](https://react.dev/) y presentar el documento generado para firmar con el formulario web de Acrobat Sign.

A continuación se muestra el flujo del caso de uso

* El usuario rellena un formulario en la aplicación React.
* Los datos del formulario se envían a un extremo de AEM Forms para generar un documento de comunicaciones interactivo.
* Cree la URL de la utilidad Acrobat Sign con el documento generado.
* Presente la URL de la utilidad a la aplicación que realiza la llamada para que el usuario firme el documento.

## Requisitos previos

Para que funcione, necesitará lo siguiente para que funcione el caso de uso:

* Un servidor AEM con Forms add en el paquete
* Un [clave de integración para una aplicación de Acrobat Sign](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)
