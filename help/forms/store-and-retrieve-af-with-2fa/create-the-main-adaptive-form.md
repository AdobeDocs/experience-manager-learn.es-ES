---
title: Crear el formulario adaptable principal
description: Crear formularios adaptables para capturar la información del solicitante y el formulario adaptable para recuperar el formulario adaptable guardado
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# Crear el formulario adaptable principal

El formulario **StoreAFWithAttachments** es el formulario adaptable principal. Este formulario adaptable es el punto de entrada del caso de uso. En este formulario se capturan los detalles del usuario, incluido el número móvil. Este formulario también tiene la capacidad de agregar algunos datos adjuntos. Cuando se hace clic en el botón Guardar y salir, se ejecuta el código del lado del servidor para almacenar los datos del formulario en la base de datos y se genera una ID de aplicación única que se presenta al usuario para que la mantenga a salvo. Esta ID de aplicación se utilizará para recuperar el número móvil asociado a la aplicación.

![formulario de solicitud principal](assets/6552.JPG)

Este formulario está asociado con las bibliotecas de **cliente bootboxjs540,storeAFWithAttachments** creadas anteriormente en el curso y un flujo de trabajo de AEM que se activa al enviar el formulario.


* Los formularios de ejemplo se basan en una plantilla [de formulario adaptable](assets/custom-template-with-page-component.zip) personalizada que debe importarse en AEM para que los formularios de ejemplo se procesen correctamente.

* El formulario [](assets/store-af-with-attachments-form.zip) StoreAfWithAttachments completado se puede descargar e importar en la instancia de AEM.

* El flujo de trabajo de [AEM asociado con este formulario](assets/workflow-model-store-af-with-attachments.zip) debe importarse en la instancia de AEM para que el formulario funcione.



