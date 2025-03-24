---
title: Interfaz de consulta de compilación
description: Tutorial de varias partes para guiarle por los pasos necesarios para consultar los envíos de formularios almacenados en Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 570a8176-ecf8-4a57-ab58-97190214c53f
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 1%

---

# Interfaz de consulta de compilación

Se creó una interfaz de consulta sencilla para permitir al &quot;administrador&quot; introducir criterios de búsqueda para recuperar envíos de formularios específicos. Los resultados se muestran en un formato tabular sencillo con la opción de ver un envío de formulario concreto.

![envíos de consultas](assets/query-submissions.png)

Los desplegables de esta interfaz se colocan en cascada. Las opciones disponibles en la lista desplegable cambian según las selecciones realizadas en la lista desplegable anterior.

Los desplegables se rellenan usando fuentes de datos RESTful.

Los resultados de la búsqueda se muestran en un componente personalizado llamado &quot;Resultados de búsqueda&quot;. Cuando el usuario hace clic en el botón Ver, el formulario se rellena previamente con los datos enviados y los archivos adjuntos.

## Siguientes pasos

[Escribir el servicio de relleno previo](./part4.md)
