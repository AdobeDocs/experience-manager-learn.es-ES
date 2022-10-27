---
title: Envío de archivos adjuntos de formularios adaptables
description: Envío de archivos adjuntos de formularios adaptables mediante el componente de envío de correo electrónico
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Introducción



Un caso de uso común es enviar los archivos adjuntos de los formularios adaptables mediante el componente de envío de correo electrónico en un flujo de trabajo AEM.
Los clientes suelen comprimir los archivos adjuntos del formulario o enviarlos como archivos individuales mediante el componente de envío de correo electrónico.

## Enviar los archivos adjuntos del formulario en un archivo zip

Para lograr el caso de uso, se escribió un paso personalizado en el proceso del flujo de trabajo. En este paso de proceso personalizado, cree un archivo zip con los archivos adjuntos del formulario en creado y almacenado en la carpeta de carga útil en un archivo denominado *zipped_attachment.zip*

![send-form-attachment](assets/send-form-attachments.JPG)

## Enviar los archivos adjuntos del formulario individualmente

Para lograr este caso de uso, se escribió un paso personalizado en el proceso del flujo de trabajo. En este paso de proceso personalizado rellenamos variables de flujo de trabajo de tipo ArrayList of Documents and ArrayList of Strings.

![enviar lista de documentos](assets/send-list-of-documents.JPG)
