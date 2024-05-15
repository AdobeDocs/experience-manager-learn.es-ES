---
title: Crear el formulario adaptable principal
description: Cree los formularios adaptables para capturar la información del solicitante y el formulario adaptable para recuperar el formulario adaptable guardado
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Crear el formulario adaptable principal

El formulario **StoreAFWithAttachments** es el formulario adaptable principal. Este formulario adaptable es el punto de entrada al caso de uso. En este formulario se capturan los detalles del usuario, incluido el número de móvil. Este formulario también tiene la capacidad de agregar algunos archivos adjuntos. Cuando se hace clic en el botón Save and Exit, se ejecuta el código del lado del servidor para almacenar los datos del formulario en la base de datos y se genera un ID de aplicación único que se presenta al usuario para que lo guarde de forma segura. Este ID de aplicación se utiliza para recuperar el número de móvil asociado a la aplicación.

![formulario de solicitud principal](assets/6552.JPG)

Este formulario está asociado con **bootboxjs540,storeAFWithAttachments** AEM las bibliotecas de cliente creadas anteriormente en el curso y un flujo de trabajo de que se activa al enviar el formulario.


* Los formularios de ejemplo se basan en [plantilla de formulario adaptable personalizada](assets/custom-template-with-page-component.zip) AEM que debe importarse en para que los formularios de ejemplo se procesen correctamente.

* El completado [Formulario StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) AEM se puede descargar e importar en la instancia de la.

* El [AEM Flujo de trabajo de asociado a este formulario](assets/workflow-model-store-af-with-attachments.zip) AEM Debe importarse en la instancia de la para que funcione el formulario.


## Pasos siguientes

[Crear el formulario recuperando el formulario guardado](./retrieve-saved-form.md)