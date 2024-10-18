---
title: Déclencheur AEM de flujo de trabajo de en la introducción al envío de formularios de HTML5
description: Déclencheur AEM de un flujo de trabajo de en el envío de formularios móviles
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: c6ffa8f7a398b01fc12e1e2efe4382c941900496
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# Déclencheur AEM del flujo de trabajo de la en un envío de formulario móvil

Un caso de uso común es procesar el XDP como HTML para actividades de captura de datos. Al enviar este formulario, es posible que sea necesario almacenar en déclencheur AEM un flujo de trabajo de. AEM En el flujo de trabajo de la, puede combinar los datos con la plantilla XDP y presentar el PDF generado para su revisión y aprobación. AEM El formulario se procesará en una instancia de publicación y el flujo de trabajo se activará en una instancia de procesamiento de.

Los siguientes pasos están involucrados en el caso de uso

* El usuario rellena y envía un formulario de HTML5 (representación de XDP en HTML5).
* El envío se administra mediante un servlet en la instancia de publicación.
* AEM El servlet almacena los datos en una carpeta predefinida en el repositorio de la instancia de procesamiento de la.
* El lanzador de flujos de trabajo está configurado para almacenar en déclencheur AEM un flujo de trabajo de cada vez que se añade un nuevo archivo en una carpeta concreta.

Este tutorial recorre los pasos necesarios para realizar el caso de uso anterior. El código de muestra y los recursos relacionados con este tutorial están [disponibles aquí.](./deploy-assets.md)


## Siguientes pasos

[Administrar envío de formulario](./handle-form-submission.md)
