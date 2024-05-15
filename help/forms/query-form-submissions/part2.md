---
title: Consulta del envío del formulario
description: Tutorial de varias partes para guiarle por los pasos necesarios para consultar los envíos de formularios almacenados en Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: b814097c-0066-44da-bba5-6914dee0df75
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 1%

---

# Crear envío personalizado

Se ha escrito un controlador de envío personalizado para administrar el envío del formulario. En un nivel superior, el controlador de envío personalizado hace lo siguiente

* Extrae el nombre del formulario enviado.
* Extrae los datos enviados. Los datos enviados de un formulario basado en componentes principales siempre están en formato JSON.
* Extrae y almacena los archivos adjuntos del formulario en el portal de Azure. Actualiza los datos json enviados con la URL del archivo adjunto.
* Crea etiquetas de índice blob: busca la lista de campos en los que se puede buscar para el formulario y su valor correspondiente a partir de los datos enviados.
* Asocia las etiquetas de índice blob con los datos enviados y los almacena en el portal de Azure.

La siguiente captura de pantalla muestra las etiquetas de índice de blob en Azure Portal

![blob-index-tags](assets/blob-index-tags.png)

El código de envío personalizado se encuentra en **_StoreFormDataWithBlobIndexTagsInAzure_** y el código para almacenar y recuperar datos de Azure se encuentra en el componente **_SaveAndFetchFromAzure_**

## Siguientes pasos

[Interfaz de consulta de compilación](./part3.md)
