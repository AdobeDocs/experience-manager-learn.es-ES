---
title: Crear Forms para firmar
description: Cree formularios que deban incluirse en el paquete de firma.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# Crear formularios para firmar

El siguiente paso es crear los formularios adaptables que desee incluir en el paquete. Recuerde seguir estos puntos al crear formularios para firmar:

* Asegúrese de que los formularios se basan en la plantilla **SignMultipleForms**. Esto garantiza que los formularios se rellenen previamente con los datos recuperados de la base de datos.

* Los formularios deben configurarse para utilizar Adobe Sign y el campo signer1 debe asociarse al campo Correo electrónico del cliente
* Los formularios también deben asociarse con clientLib llamado **getnextform**
* Los formularios deben utilizar el componente Paso de firma.
* El formulario también debe utilizar el componente **Firmar formulario múltiple** personalizado. Este componente permite desplazarse al siguiente formulario para iniciar sesión en el paquete.
* El envío del formulario debe configurarse para AEM flujo de trabajo **Actualizar estado de firma**
* Asegúrese de que la ruta del archivo de datos esté configurada en **Data.xml**. Esto es muy importante, ya que el código de muestra busca un archivo llamado Data.xml en la carga útil del proceso de envío del formulario.

Una vez creado el formulario, incluya el fragmento de formulario adaptable **campos comunes** en el formulario. El fragmento se marcará como oculto. Este fragmento contiene los campos siguientes.

* **firmado** : campo que contiene el estado de la firma
* **guid** : identificador único para identificar el formulario en el paquete
* **customerEmail** : este campo contendrá el correo electrónico del cliente



>[!NOTE]
>Si desea transferir datos de un formulario a otro en el paquete, asegúrese de que los campos del formulario tienen el mismo nombre en todos los formularios.

## Todos los formularios finalizados

Una vez que se hayan rellenado y firmado todos los formularios del paquete, debemos mostrar el mensaje correspondiente. Este mensaje se muestra con la ayuda del formulario Alldone. El formulario Alldone se incluye en los formularios de ejemplo.

## Assets

Los formularios de ejemplo, incluidos los utilizados en este tutorial, se pueden [descargar desde aquí](assets/forms-for-signing.zip)
