---
title: Capítulo 6 - Exposición del contenido en AEM Publish como JSON - Servicios de contenido
description: El capítulo 6 del tutorial AEM sin encabezado cubre cómo garantizar que todos los paquetes, la configuración y el contenido necesarios estén en AEM Publish para permitir el consumo desde la aplicación móvil.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Capítulo 6 - Exposición del contenido en AEM Publish para su entrega

El capítulo 6 del tutorial AEM sin encabezado cubre cómo garantizar que todos los paquetes, la configuración y el contenido necesarios estén en AEM Publish para permitir el consumo por parte de la aplicación móvil.

## Publicación del contenido para AEM servicios de contenido

La configuración y el contenido creados para dirigir los eventos a través de AEM Content Services deben publicarse en AEM Publish para que la aplicación móvil pueda acceder a él.

Como los servicios de contenido de AEM se crean a partir de Configuración (modelos de fragmentos de contenido, plantillas editables), Recursos (fragmentos de contenido, imágenes) y Páginas, todas estas piezas disfrutan automáticamente de AEM capacidades de administración de contenido, incluidas:

* Flujo de trabajo para revisión y procesamiento
* y activación/desactivación para extraer contenido de los puntos finales de los servicios de contenido AEM de AEM Publish

1. Asegúrese de que la variable **[!DNL WKND Mobile]Paquetes de aplicaciones**, enumerados en [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages), están instalados en **AEM Publish** using [!UICONTROL Administrador de paquetes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar el **[!DNL WKND Mobile Events API]Plantilla editable**
   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**
   1. Seleccione el **[!DNL Event API]** plantilla
   1. Toque **[!UICONTROL Publicación]** en la barra de acciones superior
   1. Publicar el **plantilla** y **todas las referencias** (políticas de contenido, asignaciones de políticas de contenido y plantillas)

1. Publicar el **[!DNL WKND Mobile Events]fragmentos de contenido**.

   Tenga en cuenta que esto es necesario, ya que la API de eventos utiliza el componente Lista de fragmentos de contenido , que no hace referencia específica a los fragmentos de contenido.

   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Recursos] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Seleccione todos los **[!DNL Event]** fragmentos de contenido
   1. Toque . **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Dejar el valor predeterminado **Publicación** acción tal cual, toque **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Select **all** fragmentos de contenido
   1. Toque **[!UICONTROL Publicación]** en la barra de acciones superior
      * *La variable [!DNL Events] El modelo de fragmento de contenido y las referencias de imágenes de evento se publicarán automáticamente junto con los fragmentos de contenido.*

1. Publicar el **[!DNL Events API]página**.
   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Sitios] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleccione el **[!DNL Events]** página
   1. Toque . **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Dejar el valor predeterminado **Publicación** acción tal cual, toque **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Seleccione el **[!DNL Events]** página
   1. Toque **[!DNL Publish]** en la barra de acciones superior

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verificación de la publicación de AEM

1. En un explorador web nuevo, asegúrese de que ha cerrado la sesión de AEM Publish y solicite las siguientes direcciones URL (sustituyendo `http://localhost:4503` para el host que sea: puerto en el que se está ejecutando AEM Publish).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Estas solicitudes deben devolver la misma respuesta JSON que cuando se revisaron los puntos finales correspondientes de AEM Author. Si no es así, asegúrese de que todas las publicaciones se hayan realizado correctamente (compruebe las colas de replicación), la variable [!DNL WKND Mobile] `ui.apps` está instalado en AEM Publish y revise la `error.log` para AEM Publish.

## Paso siguiente

No hay paquetes adicionales para instalar. Asegúrese de que el contenido y la configuración descritos en esta sección se publican en AEM Publish. De lo contrario, los capítulos posteriores no funcionarán.

* [Capítulo 7 - Consumo de AEM Content Services desde una aplicación móvil](./chapter-7.md)
