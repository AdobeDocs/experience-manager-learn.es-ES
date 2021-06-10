---
title: Probar la solución
description: Pruebe la solución añadiendo archivos adjuntos al formulario y almacenando en déclencheur el flujo de trabajo para enviar el correo electrónico.
sub-product: formularios
feature: Flujo de trabajo
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: Desarrollo
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---


# Probar la solución


* Configure [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) para enviar correos electrónicos desde el servidor de AEM Forms
* Implemente el paquete [archivos adjuntos de formulario](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) utilizando la consola web [felix](http://localhost:4502/system/console/bundles)
* Implemente el flujo de trabajo [SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Este flujo de trabajo utiliza el componente de envío de correo electrónico para enviar el archivo zip zip zip, que se guarda en la carpeta de carga útil según el paso de proceso personalizado. Configure la dirección de correo electrónico de los remitentes y los destinatarios según sus necesidades.
* Importe el [formulario de ejemplo](assets/zip-form-attachments-form.zip) desde [Forms y la IU de documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Obtenga una vista previa del ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) formulario, añada un par de archivos adjuntos y envíe el formulario.
* El flujo de trabajo se debe activar y se debe enviar una notificación por correo electrónico con el archivo zip.

