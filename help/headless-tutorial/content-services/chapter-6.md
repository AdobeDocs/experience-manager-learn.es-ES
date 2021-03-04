---
title: Capítulo 6 - Exposición del contenido en AEM Publish como JSON - Servicios de contenido
description: El capítulo 6 del tutorial sin encabezado de AEM cubre cómo garantizar que todos los paquetes, la configuración y el contenido necesarios estén en AEM Publish para permitir el consumo desde la aplicación móvil.
feature: Fragmentos de contenido, API
topic: Sin objetivos, Administración de contenido
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---


# Capítulo 6 - Exposición del contenido en AEM Publish para su entrega

El capítulo 6 del tutorial sin encabezado de AEM cubre cómo garantizar que todos los paquetes, la configuración y el contenido necesarios estén en AEM Publish para permitir el consumo por parte de la aplicación móvil.

## Publicación del contenido para los servicios de contenido de AEM

La configuración y el contenido creados para dirigir los eventos a través de AEM Content Services deben publicarse en AEM Publish para que la aplicación móvil pueda acceder a él.

Dado que los servicios de contenido de AEM se crean a partir de Configuración (modelos de fragmentos de contenido, plantillas editables), Recursos (fragmentos de contenido, imágenes) y Páginas, todas estas piezas disfrutan automáticamente de las funciones de administración de contenido de AEM, incluidas:

* Flujo de trabajo para revisión y procesamiento
* y activación/desactivación para extraer contenido de los puntos finales de los servicios de contenido de AEM Publish

1. Asegúrese de que los **[!DNL WKND Mobile]paquetes de aplicaciones**, enumerados en [Chapter 1](./chapter-1.md#wknd-mobile-application-packages), están instalados en **AEM Publish** mediante [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar la **[!DNL WKND Mobile Events API]plantilla editable**
   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**
   1. Seleccione la plantilla **[!DNL Event API]**
   1. Toque **[!UICONTROL Publicar]** en la barra de acciones superior
   1. Publicar la **plantilla** y **todas las referencias** (políticas de contenido, asignaciones de políticas de contenido y plantillas)

1. Publique los **[!DNL WKND Mobile Events]fragmentos de contenido**.

Tenga en cuenta que esto es necesario, ya que la API de eventos utiliza el componente Lista de fragmentos de contenido , que no hace referencia específica a los fragmentos de contenido.
1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Seleccione todos los fragmentos de contenido **[!DNL Event]**
1. Pulse **[!UICONTROL Administrar publicación]** en la barra de acciones superior
1. Deje la acción predeterminada **Publicar** tal cual, pulse **[!UICONTROL Siguiente]** en la barra de acciones superior
1. Seleccione **todos los** fragmentos de contenido
1. Pulse **[!UICONTROL Publicar]** en la barra de acciones superior
* *El modelo de fragmento de contenido [!DNL Events] y las imágenes de evento de referencias se publicarán automáticamente junto con los fragmentos de contenido.*

1. Publique la **[!DNL Events API]página**.
   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Sitios] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleccione la página **[!DNL Events]**
   1. Pulse **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Deje la acción predeterminada **Publicar** tal cual, pulse **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Seleccione la página **[!DNL Events]**
   1. Toque **[!DNL Publish]** en la barra de acciones superior

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verificación de la publicación de AEM

1. En un explorador web nuevo, asegúrese de que ha cerrado la sesión de AEM Publish y solicite las siguientes URL (sustituyendo `http://localhost:4503` por el host en el que se esté ejecutando el puerto AEM Publish).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Estas solicitudes deben devolver la misma respuesta JSON que cuando se revisaron los puntos finales correspondientes de AEM Author. Si no es así, asegúrese de que todas las publicaciones se hayan realizado correctamente (compruebe las colas de replicación), el paquete [!DNL WKND Mobile] `ui.apps` está instalado en AEM Publish y revise el `error.log` para AEM Publish.

## Paso siguiente

No hay paquetes adicionales para instalar. Asegúrese de que el contenido y la configuración descritos en esta sección se publican en AEM Publish. De lo contrario, los capítulos posteriores no funcionarán.

* [Capítulo 7 - Consumo de AEM Content Services desde una aplicación móvil](./chapter-7.md)
