---
title: Flujos de trabajo de inicio automático
description: Los flujos de trabajo de inicio automático amplían el procesamiento de recursos invocando automáticamente el flujo de trabajo personalizado al cargar o al volver a procesar.
feature: Asset Compute Microservices, Workflow
version: Experience Manager as a Cloud Service
jira: KT-4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
doc-type: Feature Video
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
duration: 385
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Flujos de trabajo de inicio automático

Los flujos de trabajo de inicio automático amplían el procesamiento de recursos en AEM as a Cloud Service invocando automáticamente el flujo de trabajo personalizado al cargar o al volver a procesar una vez completado el procesamiento del recurso.

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)

>[!NOTE]
>
>Utilice flujos de trabajo de inicio automático para personalizar el posprocesamiento de los recursos en lugar de utilizar iniciadores de flujo de trabajo. Los flujos de trabajo de inicio automático de _solo_ se invocan una vez que un recurso finaliza el procesamiento, en lugar de los iniciadores que se pueden invocar varias veces durante el procesamiento del recurso.

## Personalización del flujo de trabajo de posprocesamiento

Para personalizar el flujo de trabajo de posprocesamiento, copie el modelo predeterminado de flujo de trabajo de posprocesamiento de Assets Cloud [1}.](../../foundation/workflow/use-the-workflow-editor.md)

1. Comience en la pantalla Modelos de flujo de trabajo navegando hasta _Herramientas_ > _Flujo de trabajo_ > _Modelos_
2. Busque y seleccione el modelo de flujo de trabajo _Procesamiento posterior de Assets Cloud_<br/>
   ![Seleccione el modelo de flujo de trabajo de procesamiento posterior de Assets Cloud](assets/auto-start-workflow-select-workflow.png)
3. Seleccione el botón _Copiar_ para crear su flujo de trabajo personalizado
4. Seleccione su modelo de flujo de trabajo actual (que se llamará _Assets Cloud Post-Processing1_) y haga clic en el botón _Editar_ para editar el flujo de trabajo
5. En Propiedades del flujo de trabajo, asigne un nombre significativo a su flujo de trabajo de procesamiento posterior personalizado<br/>
   ![Cambiando el nombre](assets/auto-start-workflow-change-name.png)
6. Añada los pasos para satisfacer sus necesidades comerciales, en este caso añadiendo una tarea cuando los recursos se hayan completado el procesamiento. Asegúrese de que el último paso del flujo de trabajo sea siempre el _Flujo de trabajo completado_ paso<br/>
   ![Agregar pasos de flujo de trabajo](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >Los flujos de trabajo de inicio automático se ejecutan con cada carga o reprocesamiento de recursos. Tenga en cuenta cuidadosamente la escala que implican los pasos del flujo de trabajo, especialmente en el caso de operaciones por lotes como [importaciones por lotes](../../cloud-service/migration/bulk-import.md) o migraciones.

7. Seleccione el botón _Sincronizar_ para guardar los cambios y sincronizar el modelo de flujo de trabajo

## Uso de un flujo de trabajo de posprocesamiento personalizado

El procesamiento posterior personalizado se configura en carpetas. Para configurar un flujo de trabajo de posprocesamiento personalizado en una carpeta:

1. Seleccione la carpeta para la que desea configurar el flujo de trabajo y edite las propiedades de la carpeta
2. Cambiar a la ficha _Procesamiento de recursos_
3. Seleccione su flujo de trabajo de procesamiento posterior personalizado en el _flujo de trabajo de inicio automático_ cuadro de selección<br/>
   ![Establecer el flujo de trabajo de procesamiento posterior](assets/auto-start-workflow-set-workflow.png)
4. Guarde los cambios

Ahora el flujo de trabajo de procesamiento posterior personalizado se ejecutará para todos los recursos cargados o reprocesados en esa carpeta.
