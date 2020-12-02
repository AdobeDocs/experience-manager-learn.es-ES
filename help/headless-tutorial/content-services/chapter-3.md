---
title: Capítulo 3 - Creación de fragmentos de contenido de Evento - Servicios de contenido
seo-title: Introducción a Servicios de contenido de AEM - Capítulo 3 - Creación de fragmentos de contenido de Evento
description: El capítulo 3 del tutorial sin encabezado de AEM trata la creación y creación de fragmentos de contenido de Evento a partir del modelo de fragmentos de contenido creado en el capítulo 2.
seo-description: El capítulo 3 del tutorial sin encabezado de AEM trata la creación y creación de fragmentos de contenido de Evento a partir del modelo de fragmentos de contenido creado en el capítulo 2.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 1%

---


# Capítulo 3 - Creación de fragmentos de contenido de Evento

El capítulo 3 del tutorial sin encabezado de AEM trata la creación y creación de Eventos Fragmentos de contenido a partir del modelo de fragmentos de contenido creado en [Capítulo 2](./chapter-2.md).

## Creación de un fragmento de contenido de Evento

Con la creación de un modelo de fragmento de contenido [!DNL Event] y la aplicación de la configuración de AEM para WKND a la carpeta de recursos `/content/dam/wknd-mobile` (mediante la propiedad `cq:conf`), se puede crear un fragmento de contenido [!DNL Event].

Los fragmentos de contenido, que son un tipo de recurso, deben organizarse y gestionarse en AEM Assets, al igual que otros recursos.

* Utilice carpetas de configuración regional en la estructura de carpetas Recursos si es necesaria la traducción (o puede ser)
* Organice lógicamente los fragmentos de contenido para que sean fáciles de localizar y administrar

En este paso, cree un nuevo [!DNL Event] para `Punkrock Fest` en la carpeta `/content/dam/wknd-mobile/en/events` assets.

1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Recursos] > [!UICONTROL Archivos] > [!DNL WKND Mobile] >[!DNL English]** y cree carpetas de recursos **[!DNL Events]**.
1. Dentro de **[!UICONTROL Recursos] > [!UICONTROL Archivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**, cree un nuevo fragmento de contenido de tipo **[!DNL Event]** con un título de **[!DNL Punkrock Fest]**.
1. Cree el fragmento de contenido [!DNL Event] recién creado.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] ::  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] ::  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] ::  **Música**
   * [!DNL Ticket Price] ::  **10**
   * [!DNL Event Image] ::  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] ::  **La casa del reptil**
   * [!DNL Venue City] : **Nueva York**

   Toque **[!UICONTROL Guardar]** en la barra de acciones superior para guardar los cambios.

1. Con [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp), instale el paquete siguiente en AEM Author. Este paquete contiene varios fragmentos de contenido de Evento.

   [Obtener archivo: GitHub > Recursos > com.adobe.aem.guide.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Revisión de la estructura de JCR del fragmento de contenido

*Esta sección es solo informativa y está destinada a socializar la estructura JCR subyacente de los fragmentos de contenido creados a partir de modelos de fragmentos de contenido.*

1. Abra **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** en AEM Author.
1. En CRXDE Lite, en el menú de jerarquía de la izquierda, navegue a [/content/dam/wknd-mobile/en/eventos/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content), que es el nodo que representa el [!DNL Punkrock Fest] [!DNL Event] fragmento de contenido en el JCR.
1. Expanda el nodo [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Revise en el **panel Propiedades** que tiene una propiedad `cq:model` que apunta a la definición del modelo de fragmento de contenido [!DNL Event].
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Debajo del nodo `data`, seleccione el nodo [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) y revise las propiedades. Este nodo contiene el contenido recopilado durante la creación de un modelo de fragmento de contenido [!DNL Event]. Los nombres de propiedad de JCR corresponden a los nombres de propiedad del Modelo de fragmentos de contenido y los valores corresponden a los valores creados del fragmento de contenido &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Paso siguiente

Se recomienda instalar el paquete de contenido [com.adobe.aem.guide.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en AEM Author mediante [AEM [!UICONTROL Administrador de paquetes]](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido que se describen en este y en los capítulos anteriores del tutorial.

* [Capítulo 4 - Definición de plantillas de servicios de contenido de AEM](./chapter-4.md)
