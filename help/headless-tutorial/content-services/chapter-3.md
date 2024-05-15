---
title: 'Capítulo 3: Creación de fragmentos de contenido de eventos - Servicios de contenido'
description: AEM El capítulo 3 del tutorial sin encabezado de la trata la creación y el diseño de fragmentos de contenido de evento a partir del modelo de fragmento de contenido creado en el capítulo 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 167
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 1%

---

# Capítulo 3: Creación de fragmentos de contenido de eventos

AEM El capítulo 3 del tutorial sin encabezado de trata la creación de eventos y los fragmentos de contenido a partir del modelo de fragmentos de contenido creado en [Capítulo 2](./chapter-2.md).

## Creación de un fragmento de contenido de evento

Con un [!DNL Event] AEM Se ha creado el modelo de fragmento de contenido y se ha aplicado la configuración de la para WKND a `/content/dam/wknd-mobile` Carpeta de recursos (a través de ) `cq:conf` property), a [!DNL Event] Se puede crear el fragmento de contenido.

Los fragmentos de contenido, que son un tipo de recurso, deben organizarse y administrarse en AEM Assets igual que otros recursos.

* Utilice carpetas de configuración regional en la estructura de carpetas de recursos si la traducción es necesaria (o puede serlo)
* Organizar lógicamente los fragmentos de contenido para que sean fáciles de localizar y administrar

En este paso, cree un nuevo [!DNL Event] para `Punkrock Fest` en el `/content/dam/wknd-mobile/en/events` carpeta de recursos.

1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Archivos] > [!DNL WKND Mobile] >[!DNL English]** y crear carpetas de recursos **[!DNL Events]**.
1. En **[!UICONTROL Assets] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** crear un nuevo fragmento de contenido de tipo **[!DNL Event]** con un título de **[!DNL Punkrock Fest]**.
1. Crear el recién creado [!DNL Event] Fragmento de contenido.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Música**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **La Casa de los Reptiles**
   * [!DNL Venue City] : **Nueva York**

   Tocar **[!UICONTROL Guardar]** en la barra de acciones superior para guardar los cambios.

1. Uso de [AEM Administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)AEM , instale el paquete siguiente en Autor de la. Este paquete contiene una serie de fragmentos de contenido de evento.

   [Obtener archivo: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisión de la estructura JCR del fragmento de contenido

*Esta sección es solo informativa y está pensada para socializar la estructura JCR subyacente de los fragmentos de contenido creados a partir de modelos de fragmentos de contenido.*

1. Abrir **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** AEM en el autor de la.
1. En CRXDE Lite, en el menú de jerarquía de la izquierda, navegue hasta [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) que es el nodo que representa al [!DNL Punkrock Fest] [!DNL Event] Fragmento de contenido en el JCR.
1. Expanda el [datos](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) nodo.
Revise en la **Panel Propiedades** que tiene una propiedad `cq:model` que señala a la variable [!DNL Event] Definición del modelo de fragmento de contenido.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Debajo del `data` nodo, seleccione el [principal](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) y revise las propiedades. Este nodo contiene el contenido recopilado durante la creación de un [!DNL Event] Modelo de fragmento de contenido. Los nombres de las propiedades JCR corresponden a los nombres de las propiedades del modelo de fragmento de contenido y los valores corresponden a los valores creados de &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Fragmento de contenido.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Siguiente paso

Se recomienda instalar el [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM paquete de contenido en el autor de la mediante [AEM [!UICONTROL Administrador de paquetes]](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y en los capítulos anteriores del tutorial.

* [AEM Capítulo 4: Definición de plantillas de servicios de contenido de la](./chapter-4.md)
