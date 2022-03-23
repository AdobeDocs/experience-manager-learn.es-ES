---
title: 'Probar la solución: Pasos necesarios para probar los dos enfoques'
description: Pruebe la solución añadiendo archivos adjuntos al formulario y almacenando en déclencheur el flujo de trabajo para enviar el correo electrónico.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Pasos necesarios para probar los dos enfoques

* Configurar [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) para enviar correos electrónicos desde el servidor de AEM Forms
* Implemente el [archivos de formulario](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) paquete usando [consola web felix](http://localhost:4502/system/console/bundles)

## Enviar archivo zip como archivo adjunto de correo electrónico



* Implemente el [Flujo de trabajo SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Este flujo de trabajo utiliza el componente de envío de correo electrónico para enviar el archivo zip zip zip, que se guarda en la carpeta de carga útil según el paso de proceso personalizado. Configure las direcciones de correo electrónico de los remitentes y los destinatarios según sus necesidades.
* Importe el [formulario de ejemplo](assets/zip-form-attachments-form.zip) from [Interfaz de usuario de Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) y añada un par de archivos adjuntos y envíe el formulario.
* El flujo de trabajo se debe activar y se debe enviar una notificación por correo electrónico con el archivo zip.

## Enviar archivos adjuntos de formulario como archivos individuales

* Implemente el [Flujo de trabajo de SendForm.](assets/send-form-attachments-model.zip) Este flujo de trabajo utiliza el componente Enviar correo electrónico para enviar los archivos adjuntos del formulario como archivos individuales. Configure la dirección de correo electrónico de los remitentes y los destinatarios según sus necesidades.
* Importe el [formulario de ejemplo](assets/send-list-attachments-form.zip) from [Interfaz de usuario de Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) y añada un par de archivos adjuntos y envíe el formulario.
* El flujo de trabajo se debe activar y se envía una notificación por correo electrónico con los archivos adjuntos del formulario.
