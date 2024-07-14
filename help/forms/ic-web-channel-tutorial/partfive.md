---
title: Creación de fragmentos de documento para contener el nombre y la dirección del destinatario
description: Esta es la parte 5 de un tutorial de varios pasos para crear su primer documento de comunicaciones interactivas. En esta parte, se crea un fragmento de documento para contener el nombre y la dirección del destinatario.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
duration: 219
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# Creación de fragmentos de documento para contener el nombre y la dirección del destinatario {#creating-document-fragments-to-hold-the-recipient-name-and-address}

En esta parte, se crea un fragmento de documento para contener el nombre y la dirección del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Los fragmentos de documento contienen el contenido de texto de los documentos de comunicaciones interactivas. Este contenido de texto puede ser texto estático o insertarse desde los valores de los elementos del modelo de datos subyacentes. Por ejemplo, Estimado {name}, donde Estimado es texto estático y {name} es el nombre del elemento de datos del formulario. En tiempo de ejecución, esto se resolverá en Estimada Gloria Ríos o Estimado John Jacobs según el valor del elemento de nombre.

El editor de texto enriquecido es lo suficientemente intuitivo como para que un usuario empresarial cree texto e inserte elementos de datos de formulario. El editor de fragmentos de documento tiene la capacidad de dar formato al texto, especificar tipos de fuentes y estilos, insertar caracteres especiales y crear hipervínculos.

El editor de fragmentos de documento también puede insertar condiciones dentro de la línea en el texto, como se muestra en este [vídeo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Asegúrese de que los elementos del modelo de datos de formulario que inserta en un documento de fragmentos sean descendientes del elemento raíz. Por ejemplo, en este caso de uso, asegúrese de que los elementos del objeto Usuario que seleccione sean los elementos secundarios del objeto de saldos

## Siguientes pasos

[Crear documento de comunicación interactiva](./partsix.md)