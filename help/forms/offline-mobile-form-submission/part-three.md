---
title: 'Déclencheur AEM flujo de trabajo en el envío de formulario HTML5: revisar y aprobar el PDF'
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
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 2%

---

# Flujo de trabajo para revisar y aprobar el PDF enviado

El último y último paso es crear AEM flujo de trabajo que genere un PDF estático o no interactivo para su revisión y aprobación. El flujo de trabajo se activará mediante un AEM Launcher configurado en el nodo `/content/pdfsubmissions`.

La siguiente captura de pantalla muestra los pasos involucrados en el flujo de trabajo.

![flujo de trabajo](assets/workflow.PNG)

## Generar paso del flujo de trabajo del PDF no interactivo

La plantilla XDP y los datos que se van a combinar con la plantilla se especifican aquí. Los datos que se van a combinar son los datos enviados por el PDF. Los datos enviados se almacenan en el nodo `/content/pdfsubmissions`.

![flujo de trabajo](assets/generate-pdf1.PNG)

El PDF generado se asigna a la variable de flujo de trabajo denominada `submittedPDF`.

![flujo de trabajo](assets/generate-pdf2.PNG)

### Asignar el pdf generado para su revisión y aprobación

El componente Asignar flujo de trabajo de tarea se utiliza aquí para asignar el PDF generado para su revisión y aprobación. La variable `submittedPDF` se utiliza en la pestaña Forms y documentos del componente de flujo de trabajo Asignar tarea .

![flujo de trabajo](assets/assign-task.PNG)
