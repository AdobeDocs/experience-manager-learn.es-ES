---
title: Exportar fragmentos de experiencias a Adobe Target
description: Obtenga información sobre cómo publicar y exportar AEM fragmento de experiencia como ofertas de Adobe Target.
feature: Fragmentos de experiencias
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: Integraciones
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 3%

---


# Exportar fragmento de experiencia a Adobe Target {#experience-fragment-target}

Obtenga información sobre cómo exportar AEM fragmento de experiencia como ofertas de Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Pasos siguientes

+ [Crear una actividad de Target mediante ofertas de fragmentos de experiencias](./create-target-activity.md)

## Solución de problemas

### Error al exportar fragmentos de experiencias a Target

#### Error

Al exportar fragmento de experiencia a Adobe Target sin los permisos correctos en Adobe Admin Console, se produce el siguiente error en el servicio Autor de AEM:

    ![Error de interfaz de usuario de la API de Target](assets/error-target-offer.png)

... y los siguientes mensajes de registro en el registro `aemerror`:

    ![Error de la consola de la API de Target](assets/target-console-error.png)

#### Resolución

1. Inicie sesión en [Admin Console](https://adminconsole.adobe.com/) con derechos administrativos para el perfil de producto de Adobe Target utilizado pero la integración AEM
2. Seleccione __Productos > Adobe Target > Perfil de producto__
3. En la pestaña __Integrations__ , seleccione la integración de su AEM como entorno de Cloud Service (el mismo nombre que el proyecto de Adobe I/O)
4. Asignar la función __Editor__ o __Aprobador__

   ![Error de API de Target](assets/target-permissions.png)

Añadir el permiso correcto a la integración de Adobe Target debería resolver este error.

## Compatibilidad con vínculos

+ [Adobe Experience Cloud Debugger: Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger: Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)