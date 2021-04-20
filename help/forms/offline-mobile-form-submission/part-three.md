---
title: Activador del flujo de trabajo de AEM en el envío de formularios HTML5
seo-title: Activar el flujo de trabajo de AEM en el envío de formularios HTML5
description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil para activar el flujo de trabajo de AEM
seo-description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil para activar el flujo de trabajo de AEM
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 4%

---


# Flujo de trabajo para revisar y aprobar el PDF enviado

El último y último paso es crear un flujo de trabajo de AEM que genere un PDF estático o no interactivo para su revisión y aprobación. El flujo de trabajo se activará mediante un AEM Launcher configurado en el nodo `/content/pdfsubmissions`.

La siguiente captura de pantalla muestra los pasos involucrados en el flujo de trabajo.

![flujo de trabajo](assets/workflow.PNG)

## Generar paso del flujo de trabajo de PDF no interactivo

La plantilla XDP y los datos que se van a combinar con la plantilla se especifican aquí. Los datos que se van a combinar son los datos enviados desde el PDF. Los datos enviados se almacenan en el nodo `/content/pdfsubmissions`.

![flujo de trabajo](assets/generate-pdf1.PNG)

El PDF generado se asigna a la variable de flujo de trabajo denominada `submittedPDF`.

![flujo de trabajo](assets/generate-pdf2.PNG)

### Asignar el pdf generado para su revisión y aprobación

El componente Asignar flujo de trabajo de tarea se utiliza aquí para asignar el PDF generado para su revisión y aprobación. La variable `submittedPDF` se utiliza en la pestaña Forms y Documents del componente de flujo de trabajo Asignar tarea.

![flujo de trabajo](assets/assign-task.PNG)
