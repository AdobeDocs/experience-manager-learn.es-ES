---
title: 'Capítulo 2: Definición de modelos de fragmentos de contenido de eventos - Servicios de contenido'
description: AEM El capítulo 2 del tutorial sin encabezado de la aplicación cubre la activación y definición de modelos de fragmentos de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para la creación de eventos.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 378
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 2%

---

# Capítulo 2: Uso de modelos de fragmentos de contenido

AEM AEM Los modelos de fragmentos de contenido definen esquemas de contenido que se pueden utilizar para crear plantillas de la creación de contenido sin procesar por parte de autores de. Este enfoque es similar al andamiaje o a la creación basada en formularios. AEM El concepto clave con los fragmentos de contenido es que el contenido creado no depende de la presentación, es decir, está diseñado para un uso multicanal, donde la aplicación consumidora, ya sea una aplicación de una sola página, o una aplicación móvil, controla cómo se muestra el contenido al usuario.

La preocupación principal del fragmento de contenido es garantizar lo siguiente:

1. Se recopila el contenido correcto del autor
2. El contenido se puede exponer en un formato estructurado y bien entendido para consumir aplicaciones.

Este capítulo cubre la activación y definición de modelos de fragmentos de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para el modelado y la creación de &quot;eventos&quot;.

## Habilitar modelos de fragmentos de contenido

AEM Los modelos de fragmento de contenido **deben** habilitarse a través de **[explorador de configuración]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=es)** que se está usando para la configuración de [!UICONTROL .

AEM Si los modelos de fragmento de contenido **no** están habilitados para una configuración, el botón **[!UICONTROL Crear] > [!UICONTROL Fragmento de contenido]** no aparecerá para la configuración de la correspondiente.

>[!NOTE]
>
>AEM Las configuraciones de la representan un conjunto de [configuraciones de inquilino según el contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) almacenadas en `/conf`. AEM Normalmente, las configuraciones de la se correlacionan con un sitio web en particular administrado en AEM Sites o con una unidad comercial responsable de un subconjunto de contenido (recursos, páginas, etc.) AEM en la.
>
>Para que una configuración afecte a una jerarquía de contenido, se debe hacer referencia a la configuración a través de la propiedad `cq:conf` en esa jerarquía de contenido. (Esto se logra para la configuración de [!DNL WKND Mobile] en el **Paso 5** a continuación).
>
>Cuando se usa la configuración `global`, la configuración se aplica a todo el contenido y no es necesario establecer `cq:conf`.
>
>Consulte la [[!UICONTROL documentación del Explorador de configuración]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=es) para obtener más información.

1. AEM Inicie sesión en el autor de la como un usuario con los permisos adecuados para modificar la configuración pertinente.
   * Para este tutorial, se puede usar el usuario **admin**.
1. Vaya a **[!UICONTROL Herramienta] > [!UICONTROL General] > [!UICONTROL Explorador de configuración]**
1. Pulse el **icono de carpeta** junto a **[!DNL WKND Mobile]** para seleccionar y, a continuación, pulse el botón **[!UICONTROL Editar]** en la parte superior izquierda.
1. Seleccione **[!UICONTROL Modelos de fragmentos de contenido]** y pulse **[!UICONTROL Guardar y cerrar]** en la parte superior derecha.

   Esto habilita la selección de modelos de fragmento de contenido en árboles de contenido de carpeta de recursos que tienen aplicada la configuración [!DNL WKND Mobile].

   >[!NOTE]
   >
   >AEM Este cambio de configuración no es reversible desde la interfaz de usuario web de [!UICONTROL Configuración de la configuración ]. Para deshacer esta configuración:
   >    
   >    1. Abrir [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Navegue hasta `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Eliminar el nodo `models`
   >    
   >Se eliminará cualquier modelo de fragmento de contenido existente creado con esta configuración, así como sus definiciones se almacenarán en `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique la configuración **[!DNL WKND Mobile]** a la carpeta **[!DNL WKND Mobile]de Assets** para permitir que se creen fragmentos de contenido de modelos de fragmentos de contenido dentro de esa jerarquía de carpetas de Assets:

   1. AEM Vaya a ** > [!UICONTROL Assets] > [!UICONTROL Archivos]**
   1. Seleccione la carpeta **[!UICONTROL WKND Mobile]**
   1. Pulse el botón **[!UICONTROL Propiedades]** en la barra de acciones superior para abrir [!UICONTROL Propiedades de carpeta]
   1. En [!UICONTROL Propiedades de carpeta], pulse la pestaña **[!UICONTROL Cloud Service]**
   1. Compruebe que el campo **[!UICONTROL Configuración de nube]** esté establecido en **/conf/wknd-mobile**
   1. Pulse **[!UICONTROL Guardar y cerrar]** en la esquina superior derecha para mantener los cambios

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __Los modelos de fragmentos de contenido__ se han movido de __Herramientas > Assets__ a __Herramientas > General__.

## Explicación del modelo de fragmento de contenido para crear

Antes de definir nuestro modelo de fragmento de contenido, vamos a revisar la experiencia que vamos a impulsar para asegurarnos de capturar todos los puntos de datos necesarios. Para ello, revisaremos el diseño de las aplicaciones móviles y asignaremos los elementos de diseño al contenido que desea recopilar.

Podemos desglosar los puntos de datos que definen un evento de la siguiente manera:

![Creando el modelo de fragmento de contenido](assets/chapter-2/design-to-model-mapping.png)

Armados con la asignación, podemos definir los fragmentos de contenido que se utilizan para recopilar y, en última instancia, exponer los datos de evento.

## Creación del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General]  >[!UICONTROL Modelos de fragmento de contenido]**.
1. Pulse la carpeta **[!DNL WKND Mobile]** para abrirla.
1. Pulse **[!UICONTROL Crear]** para abrir el asistente de creación del modelo de fragmento de contenido.
1. Escriba **[!DNL Event]** como **[!UICONTROL Título de modelo]** *(la descripción es opcional)* y pulse **[!UICONTROL Crear]** para guardar.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## Definición de la estructura del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Modelos de fragmentos de contenido] >[!DNL WKND]**.
1. Seleccione el modelo de fragmento de contenido **[!DNL Event]** y pulse **[!UICONTROL Editar]** en la barra de acciones superior.
1. En la ficha **[!UICONTROL Tipos de datos]** de la derecha, arrastre **[!UICONTROL Entrada de texto de una línea]** a la zona desplegable izquierda para definir el campo **[!DNL Question]**.
1. Asegúrese de que la nueva **[!UICONTROL entrada de texto de una sola línea]** esté seleccionada a la izquierda y que la ficha **[!UICONTROL Propiedades]** esté seleccionada a la derecha. Rellene los campos Propiedades como se indica a continuación:

   * [!UICONTROL Procesar Como] : `textfield`
   * [!UICONTROL Etiqueta de campo] : `Event Title`
   * [!UICONTROL Nombre de propiedad] : `eventTitle`
   * [!UICONTROL Longitud máxima]: 25
   * [!UICONTROL Requerido] : `Yes`

Repita estos pasos utilizando las definiciones de entrada definidas a continuación para crear el resto del modelo de fragmento de contenido de evento.

>[!NOTE]
>
> Los campos **Nombre de propiedad** DEBEN coincidir exactamente, ya que la aplicación Android está programada para escribir estos nombres.

### Descripción del evento

* [!UICONTROL Tipo de datos] : `Multi-line text`
* [!UICONTROL Etiqueta de campo] : `Event Description`
* [!UICONTROL Nombre de propiedad] : `eventDescription`
* [!UICONTROL Tipo predeterminado]: `Rich text`

### Fecha y hora del evento

* [!UICONTROL Tipo de datos] : `Date and time`
* [!UICONTROL Etiqueta de campo] : `Event Date and Time`
* [!UICONTROL Nombre de propiedad] : `eventDateAndTime`
* [!UICONTROL Requerido] : `Yes`

### Tipo de evento

* [!UICONTROL Tipo de datos] : `Enumeration`
* [!UICONTROL Etiqueta de campo] : `Event Type`
* [!UICONTROL Nombre de propiedad] : `eventType`
* [!UICONTROL Opciones] : `Art,Music,Performance,Photography`

### Precio del billete

* [!UICONTROL Tipo de datos] : `Number`
* [!UICONTROL Procesar Como] : `numberfield`
* [!UICONTROL Etiqueta de campo] : `Ticket Price`
* [!UICONTROL Nombre de propiedad] : `eventPrice`
* [!UICONTROL Tipo] : `Integer`
* [!UICONTROL Requerido] : `Yes`

### Imagen del evento

* [!UICONTROL Tipo de datos] : `Content Reference`
* [!UICONTROL Procesar Como] : `contentreference`
* [!UICONTROL Etiqueta de campo] : `Event Image`
* [!UICONTROL Nombre de propiedad] : `eventImage`
* [!UICONTROL Ruta raíz] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Requerido] : `Yes`

### Nombre del lugar

* [!UICONTROL Tipo de datos] : `Single-line text`
* [!UICONTROL Procesar Como] : `textfield`
* [!UICONTROL Etiqueta de campo] : `Venue Name`
* [!UICONTROL Nombre de propiedad] : `venueName`
* [!UICONTROL Longitud máxima]: 20
* [!UICONTROL Requerido] : `Yes`

### Ciudad del lugar

* [!UICONTROL Tipo de datos] : `Enumeration`
* [!UICONTROL Etiqueta de campo] : `Venue City`
* [!UICONTROL Nombre de propiedad] : `venueCity`
* [!UICONTROL Opciones] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL Nombre de propiedad]** indica **tanto** el nombre de propiedad JCR donde se almacena este valor como la clave en el archivo JSON Debe ser un nombre semántico que no cambie durante la vida del modelo de fragmento de contenido.

Después de completar la creación del modelo de fragmento de contenido, debe terminar con una definición que tenga el siguiente aspecto:


![Modelo de fragmento de contenido de evento](assets/chapter-2/event-content-fragment-model.png)

## Siguiente paso

AEM AEM De forma opcional, instale el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en Author o a través del Administrador de paquetes de [](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en esta parte del tutorial.

* [Capítulo 3: Creación de fragmentos de contenido de eventos](./chapter-3.md)
