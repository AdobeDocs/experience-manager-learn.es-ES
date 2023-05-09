---
title: Crear un componente para enumerar los datos del formulario
description: Tutorial para crear un componente de resumen para revisar los datos del formulario antes del envío.
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# Crear componente para resumir los datos del formulario

Se ha creado un componente sencillo para enumerar los datos del formulario para su revisión. La variable [función de visita de la API guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) se utilizó para iterar los campos del formulario. El código de la clientlibrary asociado a este componente obtiene los componentes de panel/tabla del formulario. A partir de los elementos secundarios de este panel/componentes de tabla, los campos de formulario title, value y la expresión SOM se extraen mediante los métodos de API GuidBridge. A continuación, se crea una tabla de HTML sencilla con el título, valor y expresión SOM para que el usuario final revise o edite los datos del formulario antes de enviarlo.

Por ejemplo, la captura de pantalla siguiente muestra la tabla creada para enumerar los campos y sus valores de la variable **YourDetails**. El último TD del TR se utiliza para editar el valor del campo utilizando la expresión SOM de campos.

![visit-func](assets/visit-function.png)

## Pasos siguientes

[Probar la solución en el sistema local](./deploy-on-your-system.md)
