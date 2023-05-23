---
title: Crear fragmento de documento
description: Esta es la parte 5 de un tutorial de varios pasos para crear su primer documento de comunicaciones interactivas. En esta parte, se crea un fragmento de documento para contener el nombre y la dirección del destinatario.
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Crear fragmento de documento

En esta parte, se crea un fragmento de documento para contener el nombre y la dirección del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Los fragmentos de documento contienen el contenido de texto de los documentos de comunicaciones interactivas. Este contenido de texto puede ser texto estático o insertarse desde los valores de los elementos del modelo de datos subyacentes. Por ejemplo **Estimado _{name}_**, donde Estimado es texto estático y el nombre es el nombre del elemento del modelo de datos de formulario. En tiempo de ejecución, esto se resolverá en **Querida Gloria Ríos**o **Estimado John Jacobs:**en función del valor del elemento name.

El editor de texto enriquecido es lo suficientemente intuitivo como para que un usuario empresarial cree texto e inserte elementos de datos de formulario. El editor de fragmentos de documento tiene la capacidad de dar formato al texto, especificar tipos de fuentes y estilos, insertar caracteres especiales y crear hipervínculos.

El editor de fragmentos de documento también tiene la capacidad de insertar condiciones en línea en el texto como se muestra en este [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Asegúrese de que los elementos del modelo de datos de formulario que inserta en un documento de fragmentos sean descendientes del elemento raíz. Por ejemplo, en este caso de uso, asegúrese de que los elementos del objeto Usuario que seleccione sean los elementos secundarios del objeto de saldos

## Pasos siguientes

[Crear documento de canal de impresión](./create-print-channel-document.md)
