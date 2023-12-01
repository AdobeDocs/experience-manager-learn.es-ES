---
title: 'AEM Capítulo 6: Exposición del contenido en la publicación de la publicación de la documentación como JSON: Servicios de contenido'
description: AEM AEM El capítulo 6 del tutorial sin encabezado de la cubre cómo garantizar que todos los paquetes, la configuración y el contenido necesarios estén en la publicación para permitir el consumo desde la aplicación móvil.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# AEM Capítulo 6: Exposición del contenido en la publicación de la publicación de la publicación para su entrega

AEM AEM El capítulo 6 del tutorial sin encabezado de la cubre cómo garantizar que todos los paquetes, la configuración y el contenido necesarios estén en la publicación de la aplicación móvil para permitir su consumo.

## AEM Publicación del contenido para los servicios de contenido de la

AEM AEM La configuración y el contenido creados para dirigir los eventos a través de servicios de contenido de deben publicarse en Publish para que la aplicación móvil pueda acceder a ellos.

AEM AEM Debido a que los servicios de contenido de la se crean a partir de la configuración (modelos de fragmentos de contenido, plantillas editables), los recursos (fragmentos de contenido, imágenes) y las páginas, todos estos elementos disfrutan automáticamente de las siguientes capacidades de administración de contenido de la, incluidas:

* Flujo de trabajo para revisión y procesamiento
* AEM AEM y activación/desactivación para insertar y extraer contenido de los puntos finales de los servicios de contenido de la publicación de la de trabajo de la aplicación

1. Asegúrese de que **[!DNL WKND Mobile]Paquetes de aplicaciones**, enumerados en [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages), están instalados en **AEM Publicación de** usando [!UICONTROL Administrador de paquetes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar el **[!DNL WKND Mobile Events API]Plantilla editable**
   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**
   1. Seleccione el **[!DNL Event API]** plantilla
   1. Tocar **[!UICONTROL Publish]** en la barra de acciones superior
   1. Publicar el **plantilla** y **todas las referencias** (directivas de contenido, asignaciones de directivas de contenido y plantillas)

1. Publicar el **[!DNL WKND Mobile Events]fragmentos de contenido**.

   Tenga en cuenta que esto es necesario, ya que la API de eventos utiliza el componente Lista de fragmentos de contenido, que no hace referencia específica a los fragmentos de contenido.

   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Seleccione todos los **[!DNL Event]** fragmentos de contenido
   1. Pulse el botón **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Si se mantiene el valor predeterminado **Publish** acción tal cual, pulse **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Seleccionar **todo** fragmentos de contenido
   1. Tocar **[!UICONTROL Publish]** en la barra de acciones superior
      * *El [!DNL Events] El modelo de fragmento de contenido y las referencias a imágenes de evento se publicarán automáticamente junto con los fragmentos de contenido.*

1. Publicar el **[!DNL Events API]página**.
   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleccione el **[!DNL Events]** página
   1. Pulse el botón **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Si se mantiene el valor predeterminado **Publish** acción tal cual, pulse **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Seleccione el **[!DNL Events]** página
   1. Tocar **[!DNL Publish]** en la barra de acciones superior

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## AEM Verificar publicación de

1. AEM En un explorador web nuevo, asegúrese de que ha cerrado la sesión de Publicación de informes y solicite las siguientes direcciones URL (sustituyendo a `http://localhost:4503` AEM para cualquier host:puerto en el que se ejecute Publicación).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   AEM Estas solicitudes deben devolver la misma respuesta JSON que cuando se revisaron los puntos finales de Autor de la correspondientes. Si no es así, asegúrese de que todas las publicaciones se hayan realizado correctamente (compruebe las colas de replicación), el [!DNL WKND Mobile] `ui.apps` AEM El paquete de está instalado en el servidor de publicación de y revise el `error.log` AEM para la publicación de.

## Siguiente paso

No hay paquetes adicionales para instalar. AEM Asegúrese de que el contenido y la configuración descritos en esta sección se publiquen en Publicación de datos, de lo contrario los capítulos posteriores no funcionarán.

* [AEM Capítulo 7: Consumo de servicios de contenido desde una aplicación móvil (en inglés)](./chapter-7.md)
