---
title: Déclencheur AEM flujo de trabajo en la introducción del envío de formularios HTML5
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil al flujo de trabajo AEM déclencheur
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Descarga de formularios móviles parcialmente completados y envío a AEM flujo de trabajo

Un caso de uso común es tener la capacidad de procesar el XDP como HTML para las actividades de captura de datos. Esto funciona bien cuando los formularios son simples y se pueden rellenar y enviar en línea. Sin embargo, si el formulario es complejo y es posible que los usuarios no puedan cumplimentar el formulario en línea, tenemos que permitir que los usuarios que rellenen el formulario descarguen la versión interactiva del formulario que se rellenará con Acrobat/Reader sin conexión. Una vez rellenado el formulario, el usuario puede estar en línea para enviarlo.
Para lograr este caso de uso, debemos realizar los siguientes pasos:

* Capacidad para generar un PDF interactivo/rellenable con los datos introducidos en el formulario móvil
* Gestión del envío del PDF desde Acrobat/Reader
* Flujo de trabajo de Déclencheur Adobe Experience Manager (AEM) para revisar el PDF enviado

Este tutorial explica los pasos necesarios para lograr el caso de uso anterior. El código de muestra y los recursos relacionados con este tutorial son [disponible aquí.](part-four.md)

El siguiente vídeo le ofrece una descripción general del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)
