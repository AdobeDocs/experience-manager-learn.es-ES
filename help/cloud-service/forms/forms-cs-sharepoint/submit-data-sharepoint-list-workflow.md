---
title: Enviar datos a la lista de SharePoint mediante el paso de flujo de trabajo
description: Inserción de datos en la lista de SharePoint mediante el paso de flujo de trabajo Invocar FDM
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Inserción de datos en la lista de SharePoint mediante el paso de flujo de trabajo invocar FDM


En este artículo se explican los pasos necesarios para insertar datos en la lista de SharePoint AEM mediante el paso para invocar FDM en el flujo de trabajo de.

Este artículo presupone que tiene [el formulario adaptable se ha configurado correctamente para enviar datos a la lista de SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)


## Crear un modelo de datos de formulario basado en la fuente de datos de lista de SharePoint

* Cree un nuevo modelo de datos de formulario basado en la fuente de datos de lista de SharePoint.
* Agregue el modelo adecuado y el servicio get del modelo de datos de formulario.
* Configure el servicio de inserción para insertar el objeto del modelo de nivel superior.
* Pruebe el servicio de inserción.


## Crear un flujo de trabajo

* Cree un flujo de trabajo sencillo con el paso para invocar FDM.
* Configure el paso para invocar FDM para utilizar el modelo de datos de formulario creado en el paso anterior.
* ![associated-fdm](assets/fdm-insert-1.png)

* ![map-input-parameters](assets/fdm-insert-2.png)
* Observe el uso de la notación de puntos JSON. Los datos enviados tienen el siguiente formato y se está extrayendo el objeto ContactUS de los datos enviados.

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```



## Configuración del formulario adaptable para el flujo de trabajo de déclencheur AEM de

* Crear un formulario adaptable mediante el modelo de datos de formulario creado en el paso anterior.
* Arrastre y suelte algunos campos del origen de datos en el formulario.
* Configure la acción de envío del formulario como se muestra a continuación
* ![submit-action](assets/configure-af.png)



## Prueba del formulario

Obtenga una vista previa del formulario creado en el paso anterior. Complete el formulario y envíelo. Los datos del formulario deben insertarse en la lista de SharePoint.
