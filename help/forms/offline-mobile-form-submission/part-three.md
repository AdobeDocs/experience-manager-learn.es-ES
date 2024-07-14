---
title: 'Déclencheur AEM del flujo de trabajo de la en el envío de formularios HTML5: revisar y aprobar el PDF'
description: Continúe rellenando el formulario móvil en el modo sin conexión y envíe el formulario móvil al flujo de trabajo de déclencheur AEM de la
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# Flujo de trabajo para revisar y aprobar el PDF enviado

AEM El último y último paso es crear un flujo de trabajo que genere un PDF estático o no interactivo para su revisión y aprobación. AEM El flujo de trabajo se activa mediante un lanzador de configurado en el nodo `/content/pdfsubmissions`.

La siguiente captura de pantalla muestra los pasos involucrados en el flujo de trabajo.

![flujo de trabajo](assets/workflow.PNG)

## Paso Generar flujo de trabajo de PDF no interactivo

La plantilla XDP y los datos que se van a combinar con la plantilla se especifican aquí. Los datos que se van a combinar son los datos enviados desde el PDF. Estos datos enviados se almacenan en el nodo `/content/pdfsubmissions`.

![flujo de trabajo](assets/generate-pdf1.PNG)

El PDF generado se ha asignado a la variable de flujo de trabajo `submittedPDF`.

![flujo de trabajo](assets/generate-pdf2.PNG)

### Asignar el PDF generado para su revisión y aprobación

El componente Asignar flujo de trabajo de tareas se utiliza aquí para asignar el PDF generado para su revisión y aprobación. La variable `submittedPDF` se utiliza en la pestaña Forms y documentos del componente de flujo de trabajo Asignar tarea.

![flujo de trabajo](assets/assign-task.PNG)
