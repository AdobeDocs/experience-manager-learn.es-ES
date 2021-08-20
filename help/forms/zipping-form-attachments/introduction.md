---
title: Envío de archivos adjuntos de formularios adaptables
description: Envío de archivos adjuntos de formularios adaptables mediante el componente de envío de correo electrónico
feature: Formularios adaptables
version: 6.5
topic: Desarrollo
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 2%

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



