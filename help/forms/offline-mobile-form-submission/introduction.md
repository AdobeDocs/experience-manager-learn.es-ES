---
title: Déclencheur AEM de flujo de trabajo de en la introducción al envío de formularios de HTML5
description: Continúe rellenando el formulario móvil en el modo sin conexión y envíe el formulario móvil al flujo de trabajo de déclencheur AEM de la
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 342
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---

# AEM Descarga de un formulario móvil parcialmente completado y envío a un flujo de trabajo de

Un caso de uso común es tener la capacidad de procesar el XDP como HTML para actividades de captura de datos. Esto funciona bien cuando los formularios son simples y se pueden rellenar y enviar en línea. Sin embargo, si el formulario es complejo, es posible que los usuarios no puedan completarlo en línea. Por ello, es necesario permitir que los usuarios que rellenan el formulario descarguen la versión interactiva del formulario que se va a rellenar con Acrobat/Reader sin conexión. Una vez rellenado el formulario, el usuario puede conectarse para enviarlo.
Para llevar a cabo este caso de uso, debemos realizar los siguientes pasos:

* Capacidad para generar PDF interactivos/rellenables con los datos introducidos en el formulario móvil
* Gestionar el envío de PDF desde Acrobat/Reader
* Flujo de trabajo de Adobe Experience Manager AEM de déclencheur () para revisar el PDF enviado

Este tutorial recorre los pasos necesarios para realizar el caso de uso anterior. El código de muestra y los recursos relacionados con este tutorial están [disponibles aquí.](./deploy-assets.md)

El siguiente vídeo le ofrece una descripción general del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)

## Siguientes pasos

[Crear perfil personalizado](./custom-profile.md)