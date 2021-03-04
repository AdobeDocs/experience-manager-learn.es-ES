---
title: Activador del flujo de trabajo de AEM en el envío de formularios HTML5
seo-title: Activar el flujo de trabajo de AEM en el envío de formularios HTML5
description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil para activar el flujo de trabajo de AEM
seo-description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil para activar el flujo de trabajo de AEM
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# Descarga de formularios móviles parcialmente completados y envío al flujo de trabajo de AEM

Un caso de uso común es tener la capacidad de procesar el XDP como HTML para actividades de captura de datos. Esto funciona bien cuando los formularios son simples y se pueden rellenar y enviar en línea. Sin embargo, si el formulario es complejo y es posible que los usuarios no puedan cumplimentar el formulario en línea, es necesario que se pueda permitir que los usuarios que rellenen el formulario descarguen la versión interactiva del formulario que se rellenará con Acrobat/Reader sin conexión. Una vez rellenado el formulario, el usuario puede estar en línea para enviarlo.
Para lograr este caso de uso, debemos realizar los siguientes pasos:

* Capacidad para generar PDF interactivos/rellenables con los datos introducidos en el formulario móvil
* Gestión del envío de PDF desde Acrobat/Reader
* Activador del flujo de trabajo de Adobe Experience Manager (AEM) para revisar el PDF enviado

Este tutorial explica los pasos necesarios para lograr el caso de uso anterior. El código de muestra y los recursos relacionados con este tutorial están [disponibles aquí.](part-four.md)

El siguiente vídeo le ofrece una descripción general del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

