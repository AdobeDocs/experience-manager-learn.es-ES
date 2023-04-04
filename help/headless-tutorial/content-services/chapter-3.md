---
title: Capítulo 3 - Creación de fragmentos de contenido de eventos - Servicios de contenido
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: El capítulo 3 del tutorial AEM sin encabezado cubre la creación y autoría de fragmentos de contenido de evento a partir del modelo de fragmento de contenido creado en el capítulo 2.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 4%

---

# Capítulo 3 - Creación de fragmentos de contenido de eventos

El capítulo 3 del tutorial AEM sin encabezado cubre la creación y autoría de fragmentos de contenido de eventos a partir del modelo de fragmento de contenido creado en [Capítulo 2](./chapter-2.md).

## Creación de un fragmento de contenido de evento

Con un [!DNL Event] Creación del modelo de fragmento de contenido y aplicación de la configuración AEM para WKND a la variable `/content/dam/wknd-mobile` Carpeta de recursos (a través de la `cq:conf` propiedad), a [!DNL Event] Se puede crear el fragmento de contenido.

Los fragmentos de contenido, que son un tipo de recurso, se deben organizar y administrar en AEM Assets como otros recursos.

* Utilice carpetas de configuración regional en la estructura de carpetas de Assets si se requiere (o puede ser) la traducción
* Organice lógicamente los fragmentos de contenido para que sean fáciles de localizar y administrar

En este paso, cree un nuevo [!DNL Event] para `Punkrock Fest` en el `/content/dam/wknd-mobile/en/events` carpeta de recursos.

1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Recursos] > [!UICONTROL Archivos] > [!DNL WKND Mobile] >[!DNL English]** y crear carpetas de recursos **[!DNL Events]**.
1. Within **[!UICONTROL Recursos] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** crear un nuevo fragmento de contenido de tipo **[!DNL Event]** con un título de **[!DNL Punkrock Fest]**.
1. Cree el [!DNL Event] Fragmento de contenido.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Música**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **La casa del reptil**
   * [!DNL Venue City] : **Nueva York**

   Toque **[!UICONTROL Guardar]** en la barra de acciones superior para guardar los cambios.

1. Uso [Administrador de paquetes AEM](http://localhost:4502/crx/packmgr/index.jsp), instale el paquete siguiente en AEM Author. Este paquete contiene varios fragmentos de contenido de eventos.

   [Obtener archivo: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisión de la estructura JCR del fragmento de contenido

*Esta sección es solo informativa y está diseñada para socializar la estructura JCR subyacente de los fragmentos de contenido creados a partir de los modelos de fragmentos de contenido.*

1. Apertura **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** en AEM Author.
1. En CRXDE Lite, en el menú de jerarquía de la izquierda, vaya a [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) que es el nodo que representa la variable [!DNL Punkrock Fest] [!DNL Event] Fragmento de contenido en el JCR.
1. Expanda el [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) nodo .
Consulte la **Panel Propiedades** que tiene una propiedad `cq:model` que señala a la variable [!DNL Event] Definición del modelo de fragmento de contenido.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Debajo de la variable `data` seleccione el nodo [maestro](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) y revise las propiedades. Este nodo contiene el contenido recopilado durante la creación de un [!DNL Event] Modelo de fragmento de contenido. Los nombres de propiedad de JCR corresponden a los nombres de propiedad del Modelo de fragmento de contenido y los valores corresponden a los valores creados de la propiedad &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Fragmento de contenido.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Siguiente paso

Se recomienda que instale la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) paquete de contenido en AEM Author a través de [AEM [!UICONTROL Administrador de paquetes]](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y los capítulos anteriores del tutorial.

* [Capítulo 4 - Definición de plantillas de servicios de contenido de AEM](./chapter-4.md)
