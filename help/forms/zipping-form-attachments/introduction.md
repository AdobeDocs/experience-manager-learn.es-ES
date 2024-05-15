---
title: Enviar archivos adjuntos de formularios adaptables
description: Enviar archivos adjuntos de formularios adaptables mediante el componente Enviar correo electrónico
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
duration: 28
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 2%

---

# Introducción



AEM Un caso de uso común es enviar los archivos adjuntos del formulario adaptable mediante el componente Enviar correo electrónico en un flujo de trabajo de.
Los clientes suelen comprimir los archivos adjuntos del formulario o enviarlos como archivos individuales mediante el componente Enviar correo electrónico.

## Enviar los archivos adjuntos del formulario en un archivo zip

Para realizar el caso de uso, se ha escrito un paso de proceso de flujo de trabajo personalizado. En este paso de proceso personalizado, se crea un archivo zip con los archivos adjuntos del formulario y se almacena en la carpeta de carga útil, en un archivo llamado *zip_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## Enviar los archivos adjuntos del formulario individualmente

Para aplicar este caso de uso, se ha escrito un paso de proceso de flujo de trabajo personalizado. En este paso de proceso personalizado rellenamos variables de flujo de trabajo de tipo ArrayList of Documents y ArrayList of Strings.

![send-list-of-documents](assets/send-list-of-documents.JPG)

## Siguientes pasos

[Archivos adjuntos de formularios Zip](./custom-process-step.md)
