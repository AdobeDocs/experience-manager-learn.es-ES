---
title: Activar AEM flujo de trabajo en el envío de formulario HTML5
seo-title: Activar flujo de trabajo AEM en envío de formulario HTML5
description: Seguir rellenando formularios móviles en modo sin conexión y enviar formularios móviles para activar AEM flujo de trabajo
seo-description: Seguir rellenando formularios móviles en modo sin conexión y enviar formularios móviles para activar AEM flujo de trabajo
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# Descarga del formulario móvil parcialmente completado y envío a AEM flujo de trabajo

Un caso de uso común es tener la capacidad de representar el XDP como HTML para actividades de captura de datos. Esto funciona bien cuando los formularios son simples y se pueden rellenar y enviar en línea. Sin embargo, si el formulario es complejo, es posible que los usuarios no puedan completarlo en línea, necesitamos proporcionar la capacidad de permitir que los usuarios que lo rellenen descarguen la versión interactiva del formulario que se va a rellenar con Acrobat/Reader sin conexión. Una vez completado el formulario, el usuario puede estar en línea para enviarlo.
Para lograr este caso de uso, debemos realizar los siguientes pasos:

* Capacidad para generar un PDF interactivo/rellenable con los datos introducidos en el formulario móvil
* Gestión del envío de archivos PDF desde Acrobat/Reader
* Activar el flujo de trabajo de Adobe Experience Manager (AEM) para revisar el PDF enviado

En este tutorial se explican los pasos necesarios para llevar a cabo el caso de uso anterior. El código de muestra y los recursos relacionados con este tutorial están [disponibles aquí.](part-four.md)

El siguiente vídeo le proporciona información general sobre el caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

