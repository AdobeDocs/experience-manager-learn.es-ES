---
title: Almacenar datos de formularios adaptables en Azure Storage
description: Crear y configurar formularios adaptables para almacenar datos en Azure Storage.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 2%

---

# En resumen

Ahora tenemos todas las configuraciones e integraciones necesarias para el caso de uso. El último paso es crear un formulario adaptable basado en el modelo de datos de formulario respaldado por el almacenamiento de Azure.

Cree un formulario adaptable adaptable y asegúrese de que se basa en la plantilla de formulario adaptable correcta. Esto garantizará que el código personalizado asociado con la plantilla se ejecute cada vez que se represente un formulario adaptable.

## Formulario adaptable de ejemplo

En el formulario se han añadido 2 campos ocultos

* ID de blob: este campo se rellena con un GUID cuando se inicializa el campo. El valor de este campo se utiliza como blobid para identificar de forma exclusiva el almacenamiento blob de los datos del formulario. Este blobid se utiliza para identificar los datos del formulario.
* ID de blob devuelto: este campo se rellena con el resultado de la llamada de servicio para almacenar datos en Azure Storage. Este valor va a ser el mismo que el ID de blob del paso anterior.

El formulario tiene las siguientes reglas empresariales

* El botón Guardar formulario se muestra cuando el usuario introduce una dirección de correo electrónico. Al hacer clic en el botón Guardar formulario, los datos del formulario se almacenan en Azure Storage mediante la operación de servicio de invocación del modelo de datos de formulario.
* El ID de blob devuelto por el servicio de invocación se almacena en el campo ID de blob. Cuando este valor cambia, se envía un mensaje de correo electrónico al solicitante mediante SendGrid. El correo electrónico contiene el vínculo para abrir el formulario parcialmente completado identificado por el ID del blob.
* Se muestra un texto de confirmación al usuario cuando los datos se almacenan correctamente en Azure Storage

### Pasos siguientes

[Implementar los recursos de muestra](./deploy-sample-assets.md)

