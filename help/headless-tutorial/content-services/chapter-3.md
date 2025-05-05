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

AEM El capítulo 3 del tutorial sin encabezado de la trata la creación y creación de eventos y fragmentos de contenido a partir del modelo de fragmento de contenido creado en [Capítulo 2](./chapter-2.md).

## Creación de un fragmento de contenido de evento

AEM Con un modelo de fragmento de contenido [!DNL Event] creado y la configuración de la para WKND aplicada a la carpeta de recursos `/content/dam/wknd-mobile` (mediante la propiedad `cq:conf`), se puede crear un fragmento de contenido [!DNL Event].

Los fragmentos de contenido, que son un tipo de recurso, deben organizarse y administrarse en AEM Assets igual que otros recursos.

* Utilice carpetas de configuración regional en la estructura de carpetas de Assets si la traducción es necesaria o puede serlo
* Organizar lógicamente los fragmentos de contenido para que sean fáciles de localizar y administrar

En este paso, cree un nuevo [!DNL Event] para `Punkrock Fest` en la carpeta de recursos `/content/dam/wknd-mobile/en/events`.

1. AEM Vaya a **&#x200B; > [!UICONTROL Assets] > [!UICONTROL Archivos] > [!DNL WKND Mobile] >[!DNL English]** y cree carpetas de recursos **[!DNL Events]**.
1. En **[!UICONTROL Assets] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**, cree un nuevo fragmento de contenido de tipo **[!DNL Event]** con el título **[!DNL Punkrock Fest]**.
1. Cree el fragmento de contenido [!DNL Event] recién creado.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;Escriba unas líneas de descripción...>**
   * [!DNL Event Date] : **&lt;Seleccione una fecha futura>**
   * [!DNL Event Type] : **Música**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **La casa de los reptiles**
   * [!DNL Venue City]: **Nueva York**

   Pulse **[!UICONTROL Guardar]** en la barra de acciones superior para guardar los cambios.

1. AEM AEM Usando [Administrador de paquetes de](http://localhost:4502/crx/packmgr/index.jsp), instale el siguiente paquete en Author. Este paquete contiene una serie de fragmentos de contenido de evento.

   [Obtener archivo: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisión de la estructura JCR del fragmento de contenido

*Esta sección es solo informativa y está pensada para socializar la estructura JCR subyacente de los fragmentos de contenido creados a partir de modelos de fragmentos de contenido.*

1. Abra **[CRXDE Lite AEM](http://localhost:4502/crx/de/index.jsp)** en Autor de la.
1. En el CRXDE Lite, en el menú de jerarquía de la izquierda, navegue hasta [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content), que es el nodo que representa el fragmento de contenido [!DNL Punkrock Fest] [!DNL Event] en el JCR.
1. Expanda el nodo [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Revise en el **panel Propiedades** que tiene una propiedad `cq:model` que apunta a la definición del modelo de fragmento de contenido [!DNL Event].
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Debajo del nodo `data`, seleccione el nodo [maestro](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) y revise las propiedades. Este nodo contiene el contenido recopilado durante la creación de un modelo de fragmento de contenido de [!DNL Event]. Los nombres de propiedad JCR corresponden a los nombres de propiedad del modelo de fragmento de contenido y los valores corresponden a los valores creados del fragmento de contenido [!DNL Event] &quot;[!DNL Punkrock Fest]&quot;.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Siguiente paso

Se recomienda que instale el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en el Autor de la a través de [Administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp) que se va a AEM AEM. Este paquete contiene las configuraciones y el contenido descritos en este y en los capítulos anteriores del tutorial.

* [AEM Capítulo 4: Definición de plantillas de servicios de contenido de la](./chapter-4.md)
