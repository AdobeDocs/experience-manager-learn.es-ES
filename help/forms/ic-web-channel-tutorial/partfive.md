---
title: Creación de fragmentos de documento para guardar el nombre y la dirección del destinatario
seo-title: Creación de fragmentos de documento para guardar el nombre y la dirección del destinatario
description: 'Esta es la parte 5 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, se crea un fragmento de documento para guardar el nombre y la dirección del destinatario. '
seo-description: 'Esta es la parte 5 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, se crea un fragmento de documento para guardar el nombre y la dirección del destinatario. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Comunicación interactiva
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 1%

---


# Creación de fragmentos de documento para guardar el nombre y la dirección del destinatario {#creating-document-fragments-to-hold-the-recipient-name-and-address}

En esta parte, se crea un fragmento de documento para guardar el nombre y la dirección del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Los fragmentos de documento contienen el contenido de texto de los documentos de comunicación interactivos. Este contenido de texto puede ser texto estático o insertarse desde los valores de elementos del modelo de datos subyacente. Por ejemplo: Estimado {nombre}, donde Estimado es texto estático y {nombre} es el nombre del elemento de datos del formulario. Durante el tiempo de ejecución, esto se resolverá para Estimada Gloria Rios o Querido John Jacobs dependiendo del valor del elemento de nombre.

El editor de texto enriquecido es lo suficientemente intuitivo como para que un usuario empresarial cree texto e inserte elementos de datos de formulario. El editor de fragmentos de documento tiene la capacidad de dar formato al texto, especificar tipos y estilos de fuente, insertar caracteres especiales y crear hipervínculos.

El editor de fragmentos de documento también tiene la capacidad de insertar condiciones en línea en el texto como se muestra en este [vídeo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Asegúrese de que los elementos del Modelo de datos de formulario que inserte en un fragmento de documento sean descendientes del elemento raíz. Por ejemplo, en este caso de uso, asegúrese de que los elementos del objeto Usuario que seleccione sean los elementos secundarios del objeto Balance

