---
title: Crear el formulario adaptable principal
description: Cree formularios adaptables para capturar la información del solicitante y el formulario adaptable para recuperar el formulario adaptable guardado
feature: Formularios adaptables
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Desarrollo
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 1%

---


# Crear el formulario adaptable principal

El formulario **StoreAFWithAttachments** es la forma adaptativa principal. Este formulario adaptable es el punto de entrada al caso de uso. En este formulario se capturan los detalles del usuario, incluido el número de móvil. Este formulario también tiene la capacidad de añadir algunos archivos adjuntos. Cuando se hace clic en el botón Guardar y salir, se ejecuta el código del lado del servidor para almacenar los datos del formulario en la base de datos y se genera un identificador de aplicación único que se presenta al usuario para que lo mantenga a salvo. Esta id de aplicación se utilizará para recuperar el número de móvil asociado a la aplicación.

![formulario de aplicación principal](assets/6552.JPG)

Este formulario está asociado con las **bootboxjs540,storeAFWithAttachments** bibliotecas de cliente creadas anteriormente en el curso y un flujo de trabajo AEM que se activa al enviar el formulario.


* Los formularios de ejemplo se basan en [plantilla de formulario adaptable personalizada](assets/custom-template-with-page-component.zip) que debe importarse en AEM para que los formularios de ejemplo se representen correctamente.

* El [StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip) completado se puede descargar e importar en la instancia de AEM.

* El flujo de trabajo [AEM asociado con este formulario](assets/workflow-model-store-af-with-attachments.zip) debe importarse en la instancia de AEM para que funcione el formulario.



