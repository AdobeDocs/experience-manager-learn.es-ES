---
title: Crear Forms para firmar
description: Cree formularios que deban incluirse en el paquete de firma.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 71
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Crear formularios para firmar

El siguiente paso es crear los formularios adaptables que desea incluir en el paquete. Recuerde cumplir los siguientes puntos al crear formularios para firmar:

* Asegúrese de que los formularios estén basados en la plantilla **SignMultipleForms**. Esto garantiza que los formularios se rellenen previamente con los datos recuperados de la base de datos.

* Los formularios deben configurarse para utilizar Acrobat Sign y el campo signer1 debe asociarse al campo Customer Email
* Los formularios también deben asociarse con clientLib llamado **getnextform**
* Los formularios deben utilizar el componente Paso de firma.
* El formulario también debe usar el componente personalizado **Firmar formulario múltiple**. Este componente le permite desplazarse al siguiente formulario para iniciar sesión en el paquete.
* El envío del formulario debe configurarse para que se déclencheur AEM el flujo de trabajo **Actualizar estado de la firma** de la
* Asegúrese de que la ruta del archivo de datos esté establecida en **Data.xml**. Esto es muy importante, ya que el código de ejemplo busca un archivo llamado Data.xml en la carga útil del proceso de envío del formulario.

Una vez que haya creado el formulario, incluya el fragmento de formulario adaptable **commonfields** en el formulario. El fragmento se marca como oculto. Este fragmento contiene los siguientes campos.

* **firmado**: campo que contendrá el estado de la firma
* **guid**: identificador único para identificar el formulario en el paquete
* **customerEmail**: este campo contiene el correo electrónico del cliente



>[!NOTE]
>Si desea transferir datos de un formulario a otro en el paquete, asegúrese de que los campos del formulario tengan un nombre idéntico en todos los formularios.

## Todo el formulario terminado

Una vez que todos los formularios del paquete se hayan rellenado y firmado, se deberá mostrar el mensaje correspondiente. Este mensaje se muestra con la ayuda del formulario Alldone. El formulario Todo listo se incluye en los formularios de ejemplo.

## Recursos

Los formularios de ejemplo, incluido el utilizado en este tutorial, se pueden [descargar desde aquí](assets/forms-for-signing.zip)

## Siguientes pasos

[Prueba de la solución en el sistema local](./testing-and-trouble-shooting.md)
