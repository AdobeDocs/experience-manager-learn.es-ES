---
title: Envío de archivos adjuntos de formularios adaptables
description: Añada archivos adjuntos de formularios adaptables y envíelos mediante el componente de envío de correo electrónico
feature: formularios adaptables
topics: adaptive forms
audience: developer
doc-type: article
activity: setup
version: 6.5
topic: Desarrollo
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 2%

---


# Introducción



Un caso de uso común es comprimir los archivos adjuntos de los formularios adaptables y enviarlos mediante el componente de envío de correo electrónico en un flujo de trabajo AEM. Para lograr el caso de uso, se escribió un paso personalizado en el proceso del flujo de trabajo. En este paso de proceso personalizado, cree un archivo zip con los archivos adjuntos del formulario en creado y almacenado en la carpeta de carga útil en un archivo denominado *zipped_attachment.zip*

![send-form-attachment](assets/send-form-attachments.JPG)


