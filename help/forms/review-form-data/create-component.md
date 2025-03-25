---
title: Crear un componente para enumerar los datos del formulario
description: Tutorial para crear un componente de resumen para revisar los datos del formulario antes del envío.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 33
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# Crear un componente para resumir los datos del formulario

Se creó un componente simple para enumerar los datos del formulario para su revisión. La función de visita de la API [guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) se utilizó para recorrer en iteración los campos del formulario. El código de la biblioteca de cliente asociado con este componente obtiene los componentes de panel o tabla del formulario. De los elementos secundarios de este panel o tabla, los componentes de formulario de los campos title, value y la expresión SOM se extraen mediante los métodos de API GuidBridge. A continuación, se construye una tabla HTML simple con el título, el valor y la expresión SOM para que el usuario final revise/edite los datos del formulario antes de enviar el formulario.

Por ejemplo, la captura de pantalla siguiente muestra la tabla creada para enumerar los campos y sus valores de **YourDetails**. El último TD del TR se utiliza para editar el valor del campo mediante la expresión SOM de campos.

![visit-func](assets/visit-function.png)

## Siguientes pasos

[Prueba de la solución en el sistema local](./deploy-on-your-system.md)
