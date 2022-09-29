---
title: Crear el formulario adaptable principal
description: Cree formularios adaptables para capturar la información del solicitante y el formulario adaptable para recuperar el formulario adaptable guardado
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Crear el formulario adaptable principal

El formulario **StoreAFWithAttachments** es la forma adaptativa principal. Este formulario adaptable es el punto de entrada al caso de uso. En este formulario se capturan los detalles del usuario, incluido el número de móvil. Este formulario también tiene la capacidad de añadir algunos archivos adjuntos. Cuando se hace clic en el botón Guardar y salir, se ejecuta el código del lado del servidor para almacenar los datos del formulario en la base de datos y se genera un identificador de aplicación único que se presenta al usuario para que lo mantenga a salvo. Este id de aplicación se utiliza para recuperar el número de móvil asociado a la aplicación.

![formulario de aplicación principal](assets/6552.JPG)

Este formulario está asociado con **bootboxjs540,storeAFWithAttachments** bibliotecas de cliente creadas anteriormente en el curso y un flujo de trabajo AEM que se activa al enviar el formulario.


* Los formularios de ejemplo se basan en [plantilla de formulario adaptable personalizada](assets/custom-template-with-page-component.zip) que debe importarse en AEM para que los formularios de ejemplo se representen correctamente.

* El [Formulario StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) se puede descargar e importar en la instancia de AEM.

* La variable [AEM flujo de trabajo asociado a este formulario](assets/workflow-model-store-af-with-attachments.zip) debe importarse en la instancia de AEM para que funcione el formulario.
