---
title: Déclencheur del flujo de trabajo de AEM en el envío de formularios PDF
description: Siga rellenando el formulario móvil en el modo sin conexión y envíe el formulario móvil al flujo de trabajo de AEM de déclencheur
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 342
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# Descarga de un formulario móvil parcialmente completado y envío al déclencheur de un flujo de trabajo de AEM

Un caso de uso común es tener la capacidad de procesar el XDP como HTML para actividades de captura de datos. Esto funciona bien cuando los formularios son simples y se pueden rellenar y enviar en línea. Sin embargo, si el formulario es complejo, es posible que los usuarios no puedan completarlo en línea. Por ello, es necesario permitir que los usuarios que rellenan el formulario descarguen la versión interactiva del formulario que se va a rellenar con Acrobat/Reader sin conexión. Una vez rellenado el formulario, el usuario puede conectarse para enviarlo.
Para llevar a cabo este caso de uso, debemos realizar los siguientes pasos:

* Capacidad para generar PDF interactivo/rellenables con los datos introducidos en el formulario móvil
* Administrar el envío de PDF desde Acrobat/Reader
* Flujo de trabajo de Déclencheur Adobe Experience Manager (AEM) para revisar el PDF enviado

Este tutorial recorre los pasos necesarios para realizar el caso de uso anterior. El código de muestra y los recursos relacionados con este tutorial están [disponibles aquí.](./deploy-assets.md)

El siguiente vídeo le ofrece una descripción general del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)

## Siguientes pasos

[Crear perfil personalizado](./custom-profile.md)