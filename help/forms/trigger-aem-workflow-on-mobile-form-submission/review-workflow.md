---
title: 'Déclencheur del flujo de trabajo de AEM en el envío de formularios HTML5: revisar y aprobar PDF'
description: Flujo de trabajo para revisar el PDF enviado
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ec60d017-8b29-4185-a097-d809e18df4a7
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 3%

---

# Flujo de trabajo para revisar y aprobar el PDF enviado

El último y último paso es crear un flujo de trabajo de AEM que genere un PDF estático o no interactivo para su revisión y aprobación. El flujo de trabajo se activa mediante un lanzador de AEM configurado en el nodo `/content/formsubmissions`.

La siguiente captura de pantalla muestra los pasos involucrados en el flujo de trabajo.

![flujo de trabajo](assets/workflow.PNG)

## Paso Generar flujo de trabajo de PDF no interactivo

La plantilla XDP y los datos que se van a combinar con la plantilla se especifican aquí. Los datos que se van a combinar son los datos enviados desde PDF. Estos datos enviados se almacenan en el nodo ```/content/formsubmissions```

![flujo de trabajo](assets/generate-pdf1.PNG)

El PDF generado se ha asignado a la variable de flujo de trabajo `submittedPDF`.

![flujo de trabajo](assets/generate-pdf2.PNG)

### Asignar el PDF generado para su revisión y aprobación

El componente Asignar flujo de trabajo de tareas se utiliza aquí para asignar el PDF generado para su revisión y aprobación. La variable `submittedPDF` se utiliza en la pestaña Forms y documentos del componente de flujo de trabajo Asignar tarea.

![flujo de trabajo](assets/assign-task.PNG)


## Siguientes pasos

[Implementar los recursos en el entorno](./deploy-assets.md)
