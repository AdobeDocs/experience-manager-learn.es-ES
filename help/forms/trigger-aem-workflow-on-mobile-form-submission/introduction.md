---
title: Déclencheur del flujo de trabajo de AEM en la introducción al envío de formularios HTML5
description: Déclencheur de un flujo de trabajo de AEM en el envío de formularios móviles
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ce51b25f-6069-4799-9a61-98c0a672e821
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# Déclencheur del flujo de trabajo de AEM en un envío de formulario móvil

Un caso de uso común es procesar el XDP como HTML para actividades de captura de datos. Al enviar este formulario, puede que sea necesario almacenar en déclencheur un flujo de trabajo de AEM. En el flujo de trabajo de AEM, puede combinar los datos con la plantilla XDP y presentar el PDF generado para su revisión y aprobación. El formulario se procesará en una instancia de publicación y el flujo de trabajo se activará en una instancia de procesamiento de AEM.

Los siguientes pasos están involucrados en el caso de uso

* El usuario rellena y envía un formulario HTML5 (representación de XDP en HTML5).
* El envío se administra mediante un servlet en la instancia de publicación.
* El servlet almacena los datos en una carpeta predefinida en el repositorio de la instancia de procesamiento de AEM.
* El iniciador del flujo de trabajo está configurado para almacenar en déclencheur un flujo de trabajo de AEM cada vez que se agrega un nuevo archivo en una carpeta concreta.

Este tutorial recorre los pasos necesarios para realizar el caso de uso anterior. El código de muestra y los recursos relacionados con este tutorial están [disponibles aquí.](./deploy-assets.md)


## Siguientes pasos

[Administrar envío de formulario](./handle-form-submission.md)
