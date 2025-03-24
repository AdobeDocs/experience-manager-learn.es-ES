---
title: 'Prueba de la solución: pasos necesarios para probar los dos enfoques'
description: Pruebe la solución agregando archivos adjuntos al formulario y almacene en déclencheur el flujo de trabajo para enviar el correo electrónico.
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Pasos necesarios para probar los dos enfoques

* Configure el servicio de correo CQ de [Day](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) para enviar correos electrónicos desde el servidor de AEM Forms
* Implementar el paquete [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) mediante la consola web [felix](http://localhost:4502/system/console/bundles)

## Enviar archivo zip como archivo adjunto de correo electrónico



* Implemente el flujo de trabajo [SendFormAttachmentsViaEmail.](assets/zipped-form-attachments-model.zip) Este flujo de trabajo utiliza el componente Enviar correo electrónico para enviar el archivo zip_attachments.zip, que se guarda en la carpeta de carga útil mediante el paso de proceso personalizado. Configure las direcciones de correo electrónico de los remitentes y destinatarios según sus necesidades.
* Importar el [formulario de ejemplo](assets/zip-form-attachments-form.zip) desde [Forms y la interfaz de usuario del documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Obtenga una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled), agregue un par de archivos adjuntos y envíe el formulario.
* El flujo de trabajo debe activarse y se debe enviar una notificación por correo electrónico con el archivo zip.

## Enviar archivos adjuntos de formulario como archivos individuales

* Implemente el flujo de trabajo [SendForm.](assets/send-form-attachments-model.zip) Este flujo de trabajo utiliza el componente Enviar correo electrónico para enviar los archivos adjuntos del formulario como archivos individuales. Configure las direcciones de correo electrónico de los remitentes y destinatarios según sus necesidades.
* Importar el [formulario de ejemplo](assets/send-list-attachments-form.zip) desde [Forms y la interfaz de usuario del documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Obtenga una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled), agregue un par de archivos adjuntos y envíe el formulario.
* El flujo de trabajo debe activarse y se enviará una notificación por correo electrónico con los archivos adjuntos del formulario.
