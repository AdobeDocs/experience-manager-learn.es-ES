---
title: Enviar datos a la lista de SharePoint mediante el paso de flujo de trabajo
description: Inserción de datos en la lista de SharePoint mediante el paso de flujo de trabajo Invocar FDM
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
duration: 52
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 1%

---

# Inserción de datos en la lista de SharePoint mediante el paso de flujo de trabajo invocar FDM


En este artículo se explican los pasos necesarios para insertar datos en la lista de SharePoint mediante el paso para invocar FDM en el flujo de trabajo de AEM.

Este artículo supone que ha [configurado correctamente el formulario adaptable para enviar datos a la lista de SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)


## Crear un modelo de datos de formulario basado en la fuente de datos de lista de SharePoint

* Cree un nuevo modelo de datos de formulario basado en la fuente de datos de lista de SharePoint.
* Agregue el modelo adecuado y el servicio get del modelo de datos de formulario.
* Configure el servicio de inserción para insertar el objeto del modelo de nivel superior.
* Pruebe el servicio de inserción.


## Crear un flujo de trabajo

* Cree un flujo de trabajo sencillo con el paso para invocar FDM.
* Configure el paso para invocar FDM para utilizar el modelo de datos de formulario creado en el paso anterior.
* ![asociado-fdm](assets/fdm-insert-1.png)

## Formulario adaptable basado en componentes principales

Los datos enviados tienen el siguiente formato. Necesitamos extraer el objeto ContactUS mediante la notación de puntos en el paso de flujo de trabajo invocar el servicio de modelo de datos de formulario, como se muestra en la captura de pantalla

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


* ![map-input-parameters](assets/fdm-insert-2.png)


## Formulario adaptable basado en componentes de base

Los datos enviados tienen el siguiente formato. Extraiga el objeto JSON de ContactUS utilizando la notación de puntos en el paso de flujo de trabajo invocar servicio de modelo de datos de formulario

```json
{
    "afData": {
        "afUnboundData": {
            "data": {}
        },
        "afBoundData": {
            "data": {
                "ContactUS": {
                    "Title": "Lord",
                    "HighNetWorth": "true",
                    "SubmitterName": "John Doe",
                    "Products": "Forms"
                }
            }
        },
        "afSubmissionInfo": {
            "lastFocusItem": "guide[0].guide1[0].guideRootPanel[0].afJsonSchemaRoot[0]",
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/foundationform",
            "afSubmissionTime": "20240517100126"
        }
    }
}
```

![foundation-based-form](assets/foundation-based-form.png)

## Configuración del formulario adaptable para el flujo de trabajo de déclencheur AEM

* Crear un formulario adaptable mediante el modelo de datos de formulario creado en el paso anterior.
* Arrastre y suelte algunos campos del origen de datos en el formulario.
* Configure la acción de envío del formulario como se muestra a continuación
* ![acción de envío](assets/configure-af.png)



## Prueba del formulario

Obtenga una vista previa del formulario creado en el paso anterior. Complete el formulario y envíelo. Los datos del formulario deben insertarse en la lista de SharePoint.
