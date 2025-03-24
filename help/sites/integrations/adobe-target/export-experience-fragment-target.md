---
title: Exportar fragmentos de experiencias a Adobe Target
description: Obtenga información sobre cómo publicar y exportar fragmentos de experiencias de AEM como ofertas de Adobe Target.
feature: Experience Fragments
version: Experience Manager as a Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 3%

---

# Exportar fragmento de experiencia a Adobe Target {#experience-fragment-target}

Obtenga información sobre cómo exportar fragmentos de experiencias de AEM como ofertas de Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Siguientes pasos

+ [Crear una actividad de Target mediante ofertas de fragmentos de experiencias](./create-target-activity.md)

## Solución de problemas

### Error al exportar fragmentos de experiencias a Target

#### Error

Exportar el fragmento de experiencia a Adobe Target sin los permisos correctos en Adobe Admin Console provoca el siguiente error en el servicio de AEM Author:

![Error de IU de la API de Target](assets/error-target-offer.png)

... y los siguientes mensajes de registro en el registro `aemerror`:

![Error de consola de API de Target](assets/target-console-error.png)

#### Resolución

1. Inicie sesión en [Admin Console](https://adminconsole.adobe.com/) con derechos administrativos para el perfil de producto de Adobe Target utilizado pero la integración de AEM
2. Seleccione __Productos > Adobe Target > Perfil del producto__
3. En la ficha __Integraciones__, seleccione la integración para su entorno de AEM as a Cloud Service (el mismo nombre que el proyecto de Adobe Developer)
4. Asignar el rol __Editor__ o __Aprobador__

   ![Error de API de Target](assets/target-permissions.png)

Añadir el permiso correcto a la integración de Adobe Target debería resolver este error.

## Vínculos de soporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
