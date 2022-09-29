---
title: Crear Forms para firmar
description: Cree formularios que deban incluirse en el paquete de firma.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Creación de formularios para firmar

El siguiente paso es crear los formularios adaptables que desea incluir en el paquete. Cuando cree formularios para firmar, tenga en cuenta los siguientes puntos:

* Asegúrese de que los formularios se basen en la variable **SignMultipleForms** plantilla. Esto garantiza que los formularios se rellenen previamente con los datos recuperados de la base de datos.

* Los formularios deben configurarse para utilizar Adobe Sign y el campo signer1 debe asociarse al campo Correo electrónico del cliente
* Los formularios también deben asociarse con clientLib llamado **getnextform**
* Los formularios deben utilizar el componente Paso de firma.
* El formulario también debe utilizar la variable **Firmar formulario múltiple** componente. Este componente le permite navegar al siguiente formulario para iniciar sesión en el paquete.
* El envío del formulario debe configurarse para AEM flujo de trabajo **Actualizar estado de firma**
* Asegúrese de que la ruta del archivo de datos esté configurada en **Data.xml**. Esto es muy importante, ya que el código de ejemplo busca un archivo llamado Data.xml en la carga útil para procesar el envío del formulario.

Una vez creado el formulario, incluya la variable **campos comunes** fragmento de formulario adaptable en el formulario. El fragmento se marca como oculto. Este fragmento contiene los siguientes campos.

* **signed** - El campo que contiene el estado de la firma
* **guid** - Identificador único para identificar el formulario en el paquete
* **customerEmail** - Este campo contiene el correo electrónico del cliente



>[!NOTE]
>Si desea incluir datos de un formulario a otro en el paquete, asegúrese de que los campos del formulario tengan los mismos nombres en todos los formularios.

## Formulario hecho todo

Una vez que se hayan rellenado y firmado todos los formularios del paquete, es necesario que se muestre el mensaje correspondiente. Este mensaje se muestra con la ayuda de Alldone form. El formulario permitido se incluye en los formularios de ejemplo.

## Assets

Los formularios de ejemplo, incluido el utilizado en este tutorial, pueden ser [descargado desde aquí](assets/forms-for-signing.zip)
