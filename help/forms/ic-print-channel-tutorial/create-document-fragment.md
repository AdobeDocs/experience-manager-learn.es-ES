---
title: Creación de fragmento de documento
description: 'Esta es la parte 5 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, se crea un fragmento de documento para guardar el nombre y la dirección del destinatario. '
seo-description: 'Esta es la parte 5 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, se crea un fragmento de documento para guardar el nombre y la dirección del destinatario. '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: comunicación interactiva
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---


# Creación de fragmento de documento

En esta parte, se crea un fragmento de documento para guardar el nombre y la dirección del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Los fragmentos de documento contienen el contenido de texto de los documentos de comunicación interactivos. Este contenido de texto puede ser texto estático o insertarse desde los valores de elementos del modelo de datos subyacente. Por ejemplo **Estimado _{name}_**, donde Estimado es el texto estático y el nombre es el nombre del elemento del modelo de datos de formulario. Durante el tiempo de ejecución, esto se resolverá en **Estimado Gloria Rios**o **Estimado John Jacobs**dependiendo del valor del elemento de nombre.

El editor de texto enriquecido es lo suficientemente intuitivo como para que un usuario empresarial cree texto e inserte elementos de datos de formulario. El editor de fragmentos de documento tiene la capacidad de dar formato al texto, especificar tipos y estilos de fuente, insertar caracteres especiales y crear hipervínculos.

El editor de fragmentos de documento también tiene la capacidad de insertar condiciones en línea en el texto como se muestra en este [vídeo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Asegúrese de que los elementos del Modelo de datos de formulario que inserte en un fragmento de documento sean descendientes del elemento raíz. Por ejemplo, en este caso de uso, asegúrese de que los elementos del objeto Usuario que seleccione sean los elementos secundarios del objeto Balance

