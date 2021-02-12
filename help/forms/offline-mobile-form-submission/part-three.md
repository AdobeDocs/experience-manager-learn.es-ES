---
title: Flujo de trabajo de déclencheur AEM en el envío de formulario HTML5
seo-title: Flujo de trabajo de déclencheur AEM en envío de formulario HTML5
description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil al flujo de trabajo AEM déclencheur
seo-description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil al flujo de trabajo AEM déclencheur
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c56942831614b981684861ea78f1bd15f3bb1ab9
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 2%

---


# Flujo de trabajo para revisar y aprobar el PDF enviado

El último y último paso es crear AEM flujo de trabajo que generará un PDF estático o no interactivo para su revisión y aprobación. El flujo de trabajo se activará mediante un iniciador de AEM configurado en el nodo `/content/pdfsubmissions`.

La siguiente captura de pantalla muestra los pasos involucrados en el flujo de trabajo.

![flujo de trabajo](assets/workflow.PNG)

## Paso para generar un flujo de trabajo de PDF no interactivo

La plantilla XDP y los datos que se combinarán con la plantilla se especifican aquí. Los datos que se combinarán son los datos enviados desde el PDF. Estos datos enviados se almacenan en el nodo `/content/pdfsubmissions`.

![flujo de trabajo](assets/generate-pdf1.PNG)

El PDF generado se asigna a la variable de flujo de trabajo denominada `submittedPDF`.

![flujo de trabajo](assets/generate-pdf2.PNG)

### Asignar el PDF generado para su revisión y aprobación

El componente Asignar flujo de trabajo de tarea se utiliza aquí para asignar el PDF generado para su revisión y aprobación. La variable `submittedPDF` se utiliza en la ficha Forms y Documentos del componente de flujo de trabajo Asignar Tarea.

![flujo de trabajo](assets/assign-task.PNG)
