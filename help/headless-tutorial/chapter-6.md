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

1. Asegúrese de que los **[!DNL WKND Mobile]paquetes** de aplicación, enumerados en el [capítulo 1](./chapter-1.md#wknd-mobile-application-packages), están instalados en **AEM Publish** mediante el Administrador [!UICONTROL de]paquetes.
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar la plantilla editable **[!DNL WKND Mobile Events API]**
   1. Vaya a **[!UICONTROL AEM]>[!UICONTROL Herramientas]>[!UICONTROL General]>[!UICONTROL Plantillas]>[!DNL WKND Mobile]**
   1. Select the **[!DNL Event API]** template
   1. Toque **[!UICONTROL Publicar]** en la barra de acciones superior
   1. Publique la **plantilla** y **todas las referencias** (políticas de contenido, asignaciones de políticas de contenido y plantillas)

1. Publique los **[!DNL WKND Mobile Events]fragmentos** de contenido.

Tenga en cuenta que esto es necesario, ya que la API de Eventos utiliza el componente de Lista de fragmentos de contenido, que no hace referencia específica a los fragmentos de contenido.
1. Vaya a **[!UICONTROL AEM]>[!UICONTROL Recursos]>[!UICONTROL Archivos]>[!DNL WKND Mobile]>[!DNL English]>[!DNL Events]** 1. Seleccione todos los fragmentos de **[!DNL Event]** contenido1. Toque **[!UICONTROL Administrar publicación]** en la barra de acciones superior1. Si deja la acción **Publicar** predeterminada tal cual, toque **[!UICONTROL Siguiente]** en la barra de acciones superior1. Seleccione **todos los** fragmentos de contenido1. Toque **[!UICONTROL Publicar]** en la barra de acciones superior* *[!DNL Events]El modelo de fragmento de contenido y las imágenes de Evento de referencia se publicarán automáticamente junto con los fragmentos de contenido.*

1. Publique la **[!DNL Events API]página**.
   1. Vaya a **[!UICONTROL AEM]>[!UICONTROL Sitios]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**
   1. Seleccione la **[!DNL Events]** página
   1. Puntee en **[!UICONTROL Administrar publicación]** en la barra de acciones superior
   1. Si deja la acción **Publicar** predeterminada tal cual, toque **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Seleccione la **[!DNL Events]** página
   1. Puntee **[!DNL Publish]** en la barra de acciones superior

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verificación de AEM Publish

1. En un navegador web nuevo, asegúrese de que ha cerrado la sesión de AEM Publish y solicita las URL siguientes (sustituyendo `http://localhost:4503` el host:puerto en el que se ejecuta AEM Publish).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Estas solicitudes deben devolver la misma respuesta JSON que cuando se revisaron los puntos finales correspondientes de AEM Author. Si no es así, asegúrese de que todas las publicaciones se han realizado correctamente (compruebe las colas de replicación), el [!DNL WKND Mobile] paquete se ha instalado en AEM Publish y revise la `ui.apps` `error.log` opción de publicación de AEM.

## Paso siguiente

No hay paquetes adicionales para instalar. Asegúrese de que el contenido y la configuración descritos en esta sección se publiquen en AEM Publish. De lo contrario, los capítulos posteriores no funcionarán.

* [Capítulo 7 - Uso de AEM Content Services desde una aplicación móvil](./chapter-7.md)
