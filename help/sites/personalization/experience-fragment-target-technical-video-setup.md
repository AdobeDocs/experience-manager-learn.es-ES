---
title: Configuración de fragmentos de experiencias e integración de Adobe Target en AEM
seo-title: Set Up Experience Fragments and Adobe Target Integration in AEM
description: Adobe Experience Manager 6.4 reimagina el flujo de trabajo de personalización entre AEM y Target. Las experiencias creadas dentro de AEM ahora se pueden entregar directamente a Adobe Target como ofertas de HTML. Permite a los especialistas en marketing probar y personalizar contenido sin problemas en los distintos canales.
seo-description: Adobe Experience Manager 6.4 reimagines the personalization workflow between AEM and Target. Experiences created within AEM can now be delivered directly to Adobe Target as HTML Offers. It allows Marketers to seamlessly test and personalize content across different channels.
feature: Experience Fragments
topics: integrations
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.4, 6.5
uuid: 05fd477d-0c1a-42c0-ab92-2bca86602e2e
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: Personalization
role: Admin, Developer
level: Intermediate
exl-id: 9c139a36-e3c5-407e-af5d-b4fb8860f5a2
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 2%

---

# Configuración de fragmentos de experiencias e integración con Adobe Target{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 reimagina el flujo de trabajo de personalización entre AEM y Target. Las experiencias creadas dentro de AEM ahora se pueden entregar directamente a Adobe Target como ofertas de HTML. Permite a los especialistas en marketing probar y personalizar contenido sin problemas en los distintos canales.

>[!VIDEO](https://video.tv.adobe.com/v/22380/?quality=9&learn=on)

>[!NOTE]
>
>Se recomienda utilizar la biblioteca de cliente de at.js y se recomienda utilizar soluciones de administración de etiquetas como Launch by Adobe, Adobe DTM o cualquier solución de administración de etiquetas de terceros para agregar bibliotecas de destino a las páginas de su sitio

* La configuración del servicio de nube de Target aplicada a la carpeta de fragmentos de experiencias hereda de todos los fragmentos de experiencias creados directamente en la carpeta principal. La carpeta secundaria no hereda la configuración de los servicios de nube principales.
* El código de cliente de Target se puede obtener en Adobe Experience Cloud > Iniciar Target > En Configurar pestaña > Implementación > Editar la configuración de at.js .
* El nombre de usuario y la contraseña de la API de Target se pueden obtener enviando un ticket a Client Care con una solicitud para habilitar la capacidad de integración de Target de fragmentos de experiencias.

## Recursos adicionales {#additional-resources}

* [Documentación de fragmentos de experiencias](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Uso de fragmentos de experiencias](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
