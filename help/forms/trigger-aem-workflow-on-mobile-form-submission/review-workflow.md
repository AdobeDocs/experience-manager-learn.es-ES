---
title: 'Déclencheur AEM de flujo de trabajo de la en el envío de formularios de HTML5: revisar y aprobar el PDF'
description: Flujo de trabajo para revisar el PDF enviado
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 3%

---

# Flujo de trabajo para revisar y aprobar el PDF enviado

AEM El último y último paso es crear un flujo de trabajo que genere un PDF estático o no interactivo para su revisión y aprobación. AEM El flujo de trabajo se activa mediante un lanzador de configurado en el nodo `/content/formsubmissions`.

La siguiente captura de pantalla muestra los pasos involucrados en el flujo de trabajo.

![flujo de trabajo](assets/workflow.PNG)

## Paso Generar flujo de trabajo de PDF no interactivo

La plantilla XDP y los datos que se van a combinar con la plantilla se especifican aquí. Los datos que se van a combinar son los datos enviados desde el PDF. Estos datos enviados se almacenan en el nodo ```/content/formsubmissions```

![flujo de trabajo](assets/generate-pdf1.PNG)

El PDF generado se ha asignado a la variable de flujo de trabajo `submittedPDF`.

![flujo de trabajo](assets/generate-pdf2.PNG)

### Asignar el PDF generado para su revisión y aprobación

El componente Asignar flujo de trabajo de tareas se utiliza aquí para asignar el PDF generado para su revisión y aprobación. La variable `submittedPDF` se utiliza en la pestaña Forms y documentos del componente de flujo de trabajo Asignar tarea.

![flujo de trabajo](assets/assign-task.PNG)


## Siguientes pasos

[Implementar los recursos en el entorno](./deploy-assets.md)