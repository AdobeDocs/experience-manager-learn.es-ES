---
title: Crear esquema
description: Cree un esquema basado en los datos que deben importarse en el formulario adaptable
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 1%

---

# Introducción

El primer paso es crear un esquema basado en los datos que se utilizarán para rellenar el formulario adaptable.

## XFA se basa en un esquema

Utilice el esquema para crear el formulario adaptable

## XFA no se basa en un esquema

* Abra el XDP en AEM Forms Designer.
* Haga clic en Archivo | Propiedades del formulario | Vista previa.
* Haga clic en Generar datos de vista previa.
* Haga clic en Generar.
* Proporcione un nombre de archivo significativo como `form-data.xml`

Puede utilizar cualquiera de las herramientas gratuitas en línea para [generar XSD](https://www.freeformatter.com/xsd-generator.html) a partir de los datos xml generados en el paso anterior.

Cree un formulario adaptable basado en el esquema del paso anterior.

>[!NOTE]
>Siempre se recomienda examinar los datos generados al enviar el formulario adaptable. Esto le dará una buena idea del formato XML de los datos que deben combinarse con el formulario adaptable.

Datos enviados desde un formulario adaptable
![submitted-data](./assets/af-submitted-data.png)

Datos exportados desde el PDF
![exportar-datos](./assets/exported-data.png)

A partir de los datos exportados, tendrá que extraer el **_topMostSubform_** con los espacios de nombres adecuados que se conservan para combinar correctamente los datos con el formulario adaptable.

## Siguientes pasos

[Crear servicio OSGi](./create-osgi-service.md)
