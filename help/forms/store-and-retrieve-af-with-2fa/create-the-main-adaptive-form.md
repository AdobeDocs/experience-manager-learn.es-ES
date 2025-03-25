---
title: Crear el formulario adaptable principal
description: Cree los formularios adaptables para capturar la información del solicitante y el formulario adaptable para recuperar el formulario adaptable guardado
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 41
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Crear el formulario adaptable principal

El formulario **StoreAFWithAttachments** es el formulario adaptable principal. Este formulario adaptable es el punto de entrada al caso de uso. En este formulario se capturan los detalles del usuario, incluido el número de móvil. Este formulario también tiene la capacidad de agregar algunos archivos adjuntos. Cuando se hace clic en el botón Save and Exit, se ejecuta el código del lado del servidor para almacenar los datos del formulario en la base de datos y se genera un ID de aplicación único que se presenta al usuario para que lo guarde de forma segura. Este ID de aplicación se utiliza para recuperar el número de móvil asociado a la aplicación.

![formulario de solicitud principal](assets/6552.JPG)

Este formulario está asociado con **bootboxjs540,storeAFWithAttachments** bibliotecas de cliente creadas anteriormente en el curso y con un flujo de trabajo de AEM que se activa al enviar el formulario.


* Los formularios de ejemplo se basan en [plantilla de formulario adaptable personalizada](assets/custom-template-with-page-component.zip) que debe importarse en AEM para que los formularios de ejemplo se representen correctamente.

* El [Formulario StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) completado se puede descargar e importar en la instancia de AEM.

* El [flujo de trabajo de AEM asociado con este formulario](assets/workflow-model-store-af-with-attachments.zip) debe importarse en la instancia de AEM para que el formulario funcione.


## Pasos siguientes

[Crear el formulario recuperando el formulario guardado](./retrieve-saved-form.md)