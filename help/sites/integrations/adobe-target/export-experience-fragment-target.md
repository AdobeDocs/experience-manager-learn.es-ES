---
title: Exportar fragmentos de experiencia a Adobe Target
description: Obtenga información sobre cómo publicar y exportar AEM fragmento de experiencias como Ofertas de Adobe Target.
feature: experience-fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---


# Exportar fragmento de experiencia a Adobe Target {#experience-fragment-target}

Descubra cómo exportar AEM fragmento de experiencias como Ofertas de Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Próximos pasos

+ [Creación de una Actividad de Destinatario mediante Ofertas de fragmentos de experiencia](./create-target-activity.md)

## Solución de problemas

### Error al exportar fragmentos de experiencia a Destinatario

#### Error

Al exportar fragmento de experiencia a Adobe Target sin los permisos correctos en Adobe Admin Console, se produce el siguiente error en el servicio de AEM Author:

    ![Error de IU de la API de Destinatario](assets/error-target-offer.png)

... y los siguientes mensajes de registro en el registro `aemerror`:

    ![Error de consola de API de Destinatario](assets/target-console-error.png)

#### Resolución

1. Inicie sesión en [Admin Console](https://adminconsole.adobe.com/) con derechos administrativos para el Perfil de productos de Adobe Target utilizado pero la integración AEM
2. Seleccione __Productos > Adobe Target > Perfil del producto__
3. En la ficha __Integrations__, seleccione la integración de su AEM como entorno de Cloud Service (el mismo nombre que el proyecto de Adobe I/O)
4. Asignar la función __Editor__ o __Aprobador__

   ![Error de API de destinatario](assets/target-permissions.png)

Añadir el permiso correcto para la integración con Adobe Target debería resolver este error.

## Vínculos de soporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)