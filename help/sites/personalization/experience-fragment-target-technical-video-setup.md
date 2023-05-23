---
title: Configuración de la integración de fragmentos de experiencias y Adobe Target AEM en
seo-title: Set Up Experience Fragments and Adobe Target Integration in AEM
description: Adobe Experience Manager AEM 6.4 reimagina el flujo de trabajo de personalización entre Adobe y Target. AEM Ahora, las experiencias creadas dentro de los grupos de informes se pueden enviar directamente a Adobe Target como ofertas de HTML. Permite a los especialistas en marketing probar y personalizar el contenido sin problemas en diferentes canales.
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 2%

---

# Configuración de fragmentos de experiencias e integración con Adobe Target{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager AEM 6.4 reimagina el flujo de trabajo de personalización entre Adobe y Target. AEM Ahora, las experiencias creadas dentro de los grupos de informes se pueden enviar directamente a Adobe Target como ofertas de HTML. Permite a los especialistas en marketing probar y personalizar el contenido sin problemas en diferentes canales.

>[!VIDEO](https://video.tv.adobe.com/v/22380?quality=12&learn=on)

>[!NOTE]
>
>Se recomienda utilizar la biblioteca de cliente at.js y la práctica recomendada es utilizar soluciones de administración de etiquetas como Launch by Adobe, Adobe DTM o cualquier solución de administración de etiquetas de terceros para agregar bibliotecas de destino a las páginas del sitio

* La configuración del servicio en la nube de Target aplicada a la carpeta de fragmentos de experiencias hereda todos los fragmentos de experiencias creados directamente en la carpeta principal. La carpeta secundaria no hereda la configuración de los servicios en la nube principales.
* El código de cliente de Target se puede obtener en Adobe Experience Cloud > Iniciar Target > En la pestaña Configuración > Implementación > Editar la configuración de at.js.
* El nombre de usuario y la contraseña de la API de Target se pueden obtener enviando un ticket al Servicio de atención al cliente con una solicitud para habilitar la capacidad de integración de Target de fragmentos de experiencias.

## Recursos adicionales {#additional-resources}

* [Documentación de fragmentos de experiencias](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Uso de fragmentos de experiencias](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
