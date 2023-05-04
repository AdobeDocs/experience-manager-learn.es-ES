---
title: Bandeja de entrada de AEM
description: Personalización de la bandeja de entrada añadiendo nuevas columnas basadas en datos de flujo de trabajo
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 29%

---

# Bandeja de entrada de AEM

AEM Bandeja de entrada consolida las notificaciones y tareas de varios componentes de AEM, incluidos los flujos de trabajo de Forms. Cuando se activa un flujo de trabajo de Forms que contiene una Etapa de tarea de asignación, la aplicación asociada aparece como una tarea en la bandeja de entrada del usuario asignado.

La interfaz de usuario de la bandeja de entrada ofrece vistas de lista y calendario para ver las tareas. También puede configurar la configuración de vista. Puede filtrar las tareas en función de varios parámetros.

Puede personalizar la bandeja de entrada de un Experience Manager para cambiar el título predeterminado de una columna, reordenar la posición de una columna y mostrar columnas adicionales basadas en los datos de un flujo de trabajo.

>[!NOTE]
>
>Debe ser miembro de los administradores o de los administradores del flujo de trabajo para personalizar las columnas de la bandeja de entrada

## Personalización de columnas

[Iniciar AEM bandeja de entrada](http://localhost:4502/aem/inbox)
Abra el Control de administración haciendo clic en el botón _Vista de lista_ y, a continuación, seleccione _Control de administración_ como se muestra en la captura de pantalla siguiente

![admin-control](assets/open-customization.png)

En la interfaz de usuario de personalización de columnas, puede realizar las siguientes operaciones

* Eliminar columnas
* Reordenar las columnas
* Cambiar el nombre de las columnas

## Personalizar la promoción de la marca

En la personalización de la marca puede hacer lo siguiente

* Añadir el logotipo de la organización
* Personalizar el texto del encabezado
* Personalización del vínculo de ayuda
* Ocultar opciones de navegación

![promoción de la marca en la bandeja de entrada](assets/branding-customization.PNG)

## Pasos siguientes

[Agregar columna Casada](./add-married-column.md)
