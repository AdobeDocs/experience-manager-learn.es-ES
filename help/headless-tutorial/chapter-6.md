---
title: Introducción a AEM sin encabezado - Capítulo 6 - Exposición del contenido en AEM Publish como JSON
description: El capítulo 6 del tutorial sin encabezado de AEM cubre la seguridad de que todos los paquetes, la configuración y el contenido necesarios están en AEM Publish para permitir el consumo desde la aplicación móvil.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---


# Capítulo 6 - Exposición del contenido en AEM Publish for Envío

El capítulo 6 del tutorial sin encabezado de AEM cubre la seguridad de que todos los paquetes, la configuración y el contenido necesarios están en AEM Publish para permitir el consumo por parte de la aplicación móvil.

## Publicación del contenido para AEM Content Services

La configuración y el contenido creados para dirigir los Eventos a través de AEM Content Services deben publicarse en AEM Publish para que la aplicación móvil pueda acceder a ellos.

Debido a que AEM Content Services se crea a partir de Configuración (modelos de fragmentos de contenido, plantillas editables), Recursos (fragmentos de contenido, imágenes) y Páginas, todas estas piezas disfrutan automáticamente de AEM funciones de gestor de contenido, entre las que se incluyen:

* Flujo de trabajo para revisión y procesamiento
* y activación/desactivación para insertar y extraer contenido de los puntos finales de los servicios de contenido AEM de AEM Publish

1. Asegúrese de que los **[!DNL WKND Mobile]paquetes de aplicaciones**, enumerados en [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages), están instalados en **AEM Publish** mediante [!UICONTROL Administrador de paquetes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar la **[!DNL WKND Mobile Events API]plantilla editable**
   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**
   1. Seleccione la plantilla **[!DNL Event API]**
   1. Toque **[!UICONTROL Publicar]** en la barra de acciones superior
   1. Publique la **plantilla** y **todas las referencias** (políticas de contenido, asignaciones de políticas de contenido y plantillas)

1. Publique los fragmentos de contenido **[!DNL WKND Mobile Events]**.

Tenga en cuenta que esto es necesario, ya que la API de Eventos utiliza el componente de Lista de fragmentos de contenido, que no hace referencia específica a los fragmentos de contenido.
1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Recursos] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Seleccionar todos los fragmentos de contenido **[!DNL Event]**
1. Toque **[!UICONTROL Administrar publicación]** en la barra de acciones superior
1. Si deja la acción **Publicar** predeterminada tal cual, toque **[!UICONTROL Siguiente]** en la barra de acciones superior
1. Seleccionar **todos los** fragmentos de contenido
1. Toque **[!UICONTROL Publicar]** en la barra de acciones superior
* *El modelo de fragmento de contenido [!DNL Events] y las imágenes de Evento de referencias se publicarán automáticamente junto con los fragmentos de contenido.*

1. Publique la **[!DNL Events API]página**.
   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Sitios] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Seleccione la página **[!DNL Events]**
   1. Toque **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Si deja la acción predeterminada **Publicar** tal cual, toque **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Seleccione la página **[!DNL Events]**
   1. Puntee **[!DNL Publish]** en la barra de acciones superior

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verificación de AEM Publish

1. En un nuevo navegador web, asegúrese de que ha cerrado la sesión de AEM Publish y solicite las siguientes URL (sustituyendo `http://localhost:4503` por el host:puerto en el que se esté ejecutando AEM Publish).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Estas solicitudes deben devolver la misma respuesta JSON que cuando se revisaron los puntos finales correspondientes de AEM Author. Si no es así, asegúrese de que todas las publicaciones se han realizado correctamente (compruebe las colas de replicación), el paquete [!DNL WKND Mobile] `ui.apps` está instalado en AEM Publish y revise el `error.log` para AEM Publish.

## Paso siguiente

No hay paquetes adicionales para instalar. Asegúrese de que el contenido y la configuración descritos en esta sección se publiquen en AEM Publish. De lo contrario, los capítulos posteriores no funcionarán.

* [Capítulo 7 - Uso de AEM Content Services desde una aplicación móvil](./chapter-7.md)
