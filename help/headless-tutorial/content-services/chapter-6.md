---
title: 'AEM Capítulo 6: Exposición del contenido en Publish como JSON de: Servicios de contenido'
description: AEM AEM El capítulo 6 del tutorial sin encabezado de la cubre cómo garantizar que todos los paquetes, la configuración y el contenido necesarios estén en Publish para permitir el consumo desde la aplicación móvil.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 196
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# AEM Capítulo 6: Exposición del contenido en Publish para su entrega en la red de distribución de contenido de la red de

AEM AEM El capítulo 6 del tutorial sin encabezado de la cubre cómo garantizar que todos los paquetes, la configuración y el contenido necesarios estén en Publish para permitir el consumo por parte de la aplicación móvil.

## AEM Publicación del contenido para los servicios de contenido de la

AEM AEM La configuración y el contenido creados para dirigir los eventos a través de los servicios de contenido de la deben publicarse en Publish para que la aplicación móvil pueda acceder a ellos.

AEM Debido a que los servicios de contenido de la se crean a partir de la configuración (modelos de fragmentos de contenido, plantillas editables), Assets AEM (fragmentos de contenido, imágenes) y las páginas, todos estos elementos disfrutan automáticamente de las funciones de administración de contenido de la aplicación, que incluyen las siguientes:

* Flujo de trabajo para revisión y procesamiento
* AEM y activación/desactivación para insertar y extraer contenido de los puntos finales de los servicios de contenido de Publish AEM que se encuentran en la interfaz de usuario de los servicios de contenido de

1. AEM Asegúrese de que los paquetes de aplicaciones **[!DNL WKND Mobile]**, enumerados en el [capítulo 1](./chapter-1.md#wknd-mobile-application-packages), estén instalados en **Publish** mediante [!UICONTROL Administrador de paquetes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publish la plantilla editable **[!DNL WKND Mobile Events API]**
   1. AEM Vaya a ** > [!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**
   1. Seleccionar la plantilla **[!DNL Event API]**
   1. Pulse **[!UICONTROL Publish]** en la barra de acciones superior
   1. Publish usa la **plantilla** y **todas las referencias** (directivas de contenido, asignaciones de directivas de contenido y plantillas)

1. Publish los **[!DNL WKND Mobile Events]fragmentos de contenido**.

   Tenga en cuenta que esto es necesario, ya que la API de eventos utiliza el componente Lista de fragmentos de contenido, que no hace referencia específica a los fragmentos de contenido.

   1. AEM Vaya a ** > [!UICONTROL Assets] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Seleccionar todos los **[!DNL Event]** fragmentos de contenido
   1. Pulse **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Dejando la acción predeterminada **Publish** tal cual, pulse **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Seleccionar **todos** los fragmentos de contenido
   1. Pulse **[!UICONTROL Publish]** en la barra de acciones superior
      * *El modelo de fragmento de contenido [!DNL Events] y las referencias a imágenes de evento se publicarán automáticamente junto con los fragmentos de contenido.*

1. Publish la página **[!DNL Events API]**.
   1. AEM Vaya a ** > [!UICONTROL Sitios] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleccionar la página **[!DNL Events]**
   1. Pulse **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Dejando la acción predeterminada **Publish** tal cual, pulse **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Seleccionar la página **[!DNL Events]**
   1. Pulse **[!DNL Publish]** en la barra de acciones superior

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## AEM Verificar Publish

1. AEM En un explorador web nuevo, asegúrese de que ha cerrado sesión en Publish AEM y solicite las siguientes direcciones URL (sustituyendo `http://localhost:4503` por cualquier host:puerto en el que se ejecute Publish).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   AEM Estas solicitudes deben devolver la misma respuesta JSON que cuando se revisaron los puntos finales de Autor de la correspondientes. AEM Si no es así, asegúrese de que todas las publicaciones se hayan realizado correctamente (compruebe las colas de replicación), de que el paquete [!DNL WKND Mobile] `ui.apps` está instalado en Publish AEM y revise `error.log` para obtener acceso a Publish de la manera más rápida y eficaz.

## Siguiente paso

No hay paquetes adicionales para instalar. AEM Asegúrese de que el contenido y la configuración descritos en esta sección se publiquen en Publish; de lo contrario, los capítulos posteriores no funcionarán.

* [AEM Capítulo 7: Consumo de servicios de contenido desde una aplicación móvil (en inglés)](./chapter-7.md)
