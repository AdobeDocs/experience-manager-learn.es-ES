---
title: 'Capítulo 2: Definición de modelos de fragmentos de contenido de eventos - Servicios de contenido'
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: AEM El capítulo 2 del tutorial sin encabezado de la aplicación cubre la activación y definición de modelos de fragmentos de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para la creación de eventos.
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 10%

---

# Capítulo 2: Uso de modelos de fragmentos de contenido

AEM AEM Los modelos de fragmentos de contenido definen esquemas de contenido que se pueden utilizar para crear plantillas de la creación de contenido sin procesar por parte de autores de. Este enfoque es similar al andamiaje o a la creación basada en formularios. AEM El concepto clave con los fragmentos de contenido es que el contenido creado no depende de la presentación, es decir, está diseñado para un uso multicanal, donde la aplicación consumidora, ya sea una aplicación de una sola página, o una aplicación móvil, controla cómo se muestra el contenido al usuario.

La preocupación principal del fragmento de contenido es garantizar lo siguiente:

1. Se recopila el contenido correcto del autor
2. El contenido se puede exponer en un formato estructurado y bien entendido para consumir aplicaciones.

Este capítulo cubre la activación y definición de modelos de fragmentos de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para el modelado y la creación de &quot;eventos&quot;.

## Habilitar modelos de fragmentos de contenido

Modelos de fragmento de contenido **debe** se habilitará mediante **[AEM [!UICONTROL Explorador de configuración]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=es)**.

Si los modelos de fragmento de contenido son **no** habilitada para una configuración, la variable **[!UICONTROL Crear] > [!UICONTROL Fragmento de contenido]** AEM no aparecerá para la configuración de la aplicación en cuestión.

>[!NOTE]
>
>AEM Las configuraciones de representan un conjunto de [configuraciones de inquilino según el contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) almacenado bajo `/conf`. AEM Normalmente, las configuraciones de la se correlacionan con un sitio web en particular administrado en AEM Sites o con una unidad comercial responsable de un subconjunto de contenido (recursos, páginas, etc.) AEM en la.
>
>Para que una configuración afecte a una jerarquía de contenido, se debe hacer referencia a la configuración a través del `cq:conf` en esa jerarquía de contenido. (Esto se logra para el [!DNL WKND Mobile] configuración en **Paso 5** abajo).
>
>Si la variable `global` se utiliza, la configuración se aplica a todo el contenido y `cq:conf` no es necesario configurar.
>
>Consulte la documentación del [[!UICONTROL Explorador de configuración] para obtener más información.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=es)

1. Inicie sesión en AEM Author como usuario con los permisos adecuados para modificar la configuración relevante.
   * Para este tutorial, la variable **administrador** se puede utilizar el usuario.
1. Vaya a **[!UICONTROL Herramienta] > [!UICONTROL General] > [!UICONTROL Explorador de configuración]**
1. Pulse el botón **icono de carpeta** junto a **[!DNL WKND Mobile]** para seleccionar y, a continuación, pulse el botón **[!UICONTROL Editar] botón** en la parte superior izquierda.
1. Seleccionar **[!UICONTROL Modelos de fragmento de contenido]** y pulse **[!UICONTROL Guardar y cerrar]** en la parte superior derecha.

   Esto habilita la selección de modelos de fragmentos de contenido en árboles de contenido de carpetas de recursos que tienen la variable [!DNL WKND Mobile] configuración aplicada.

   >[!NOTE]
   >
   >Este cambio de configuración no se puede deshacer desde el [!UICONTROL AEM Configuración de] IU web. Para deshacer esta configuración:
   >    
   >    1. Abrir [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Navegue hasta `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Elimine el `models` nodo

   >    
   >Cualquier modelo de fragmento de contenido existente creado con esta configuración se eliminará, así como sus definiciones se almacenarán en `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique la variable **[!DNL WKND Mobile]** configuración de a la **[!DNL WKND Mobile]Carpeta de recursos** para permitir que los fragmentos de contenido de los modelos de fragmentos de contenido se creen dentro de esa jerarquía de carpetas de recursos:

   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Archivos]**
   1. Seleccione el **[!UICONTROL WKND Mobile] carpeta**
   1. Pulse el botón **[!UICONTROL Propiedades]** botón en la barra de acciones superior para abrir [!UICONTROL Propiedades de carpeta]
   1. Entrada [!UICONTROL Propiedades de carpeta], pulse el botón **[!UICONTROL Cloud Services]** pestaña
   1. Compruebe el **[!UICONTROL Configuración de nube]** el campo está configurado como **/conf/wknd-mobile**
   1. Tocar **[!UICONTROL Guardar y cerrar]** en la esquina superior derecha para mantener los cambios

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __Modelos de fragmento de contenido__ se han movido de __Herramientas > Recursos__ hasta __Herramientas > General__.

## Explicación del modelo de fragmento de contenido para crear

Antes de definir nuestro modelo de fragmento de contenido, vamos a revisar la experiencia que vamos a impulsar para asegurarnos de capturar todos los puntos de datos necesarios. Para ello, revisaremos el diseño de las aplicaciones móviles y asignaremos los elementos de diseño al contenido que desea recopilar.

Podemos desglosar los puntos de datos que definen un evento de la siguiente manera:

![Creación del modelo de fragmento de contenido](assets/chapter-2/design-to-model-mapping.png)

Armados con la asignación, podemos definir los fragmentos de contenido que se utilizan para recopilar y, en última instancia, exponer los datos de evento.

## Creación del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Modelos de fragmento de contenido]**.
1. Pulse el botón **[!DNL WKND Mobile]** carpeta para abrir.
1. Tocar **[!UICONTROL Crear]** para abrir el asistente de creación del Modelo de fragmento de contenido.
1. Entrar **[!DNL Event]** como el **[!UICONTROL Título de modelo]** *(la descripción es opcional)* y pulse **[!UICONTROL Crear]** para guardar.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## Definición de la estructura del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Modelos de fragmento de contenido] >[!DNL WKND]**.
1. Seleccione el **[!DNL Event]** Modelo de fragmento de contenido y pulse **[!UICONTROL Editar]** en la barra de acciones superior.
1. Desde el **[!UICONTROL Tipos de datos] pestaña** a la derecha, arrastre el **[!UICONTROL Entrada de texto de línea única]** en la zona de colocación izquierda para definir la variable **[!DNL Question]** field.
1. Asegúrese de que el nuevo **[!UICONTROL Entrada de texto de línea única]** se selecciona a la izquierda y la variable **[!UICONTROL Propiedades] pestaña** está seleccionado a la derecha. Rellene los campos Propiedades como se indica a continuación:

   * [!UICONTROL Procesar como] : `textfield`
   * [!UICONTROL Etiqueta de campo] : `Event Title`
   * [!UICONTROL Nombre de la propiedad] : `eventTitle`
   * [!UICONTROL Longitud máxima] : 25
   * [!UICONTROL Requerido] : `Yes`

Repita estos pasos utilizando las definiciones de entrada definidas a continuación para crear el resto del modelo de fragmento de contenido de evento.

>[!NOTE]
>
> El **Nombre de propiedad** Los campos DEBEN coincidir exactamente, ya que la aplicación de Android está programada para eliminar estos nombres.

### Descripción del evento

* [!UICONTROL Tipo de datos] : `Multi-line text`
* [!UICONTROL Etiqueta de campo] : `Event Description`
* [!UICONTROL Nombre de la propiedad] : `eventDescription`
* [!UICONTROL Tipo predeterminado] : `Rich text`

### Fecha y hora del evento

* [!UICONTROL Tipo de datos] : `Date and time`
* [!UICONTROL Etiqueta de campo] : `Event Date and Time`
* [!UICONTROL Nombre de la propiedad] : `eventDateAndTime`
* [!UICONTROL Requerido] : `Yes`

### Tipo de evento

* [!UICONTROL Tipo de datos] : `Enumeration`
* [!UICONTROL Etiqueta de campo] : `Event Type`
* [!UICONTROL Nombre de la propiedad] : `eventType`
* [!UICONTROL Opciones] : `Art,Music,Performance,Photography`

### Precio del billete

* [!UICONTROL Tipo de datos] : `Number`
* [!UICONTROL Procesar como] : `numberfield`
* [!UICONTROL Etiqueta de campo] : `Ticket Price`
* [!UICONTROL Nombre de la propiedad] : `eventPrice`
* [!UICONTROL Tipo] : `Integer`
* [!UICONTROL Requerido] : `Yes`

### Imagen del evento

* [!UICONTROL Tipo de datos] : `Content Reference`
* [!UICONTROL Procesar como] : `contentreference`
* [!UICONTROL Etiqueta de campo] : `Event Image`
* [!UICONTROL Nombre de la propiedad] : `eventImage`
* [!UICONTROL Ruta de acceso raíz] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Requerido] : `Yes`

### Nombre del lugar

* [!UICONTROL Tipo de datos] : `Single-line text`
* [!UICONTROL Procesar como] : `textfield`
* [!UICONTROL Etiqueta de campo] : `Venue Name`
* [!UICONTROL Nombre de la propiedad] : `venueName`
* [!UICONTROL Longitud máxima] : 20
* [!UICONTROL Requerido] : `Yes`

### Ciudad del lugar

* [!UICONTROL Tipo de datos] : `Enumeration`
* [!UICONTROL Etiqueta de campo] : `Venue City`
* [!UICONTROL Nombre de la propiedad] : `venueCity`
* [!UICONTROL Opciones] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>El **[!UICONTROL Nombre de propiedad]** indica el **ambos** el nombre de la propiedad JCR donde se almacena este valor, así como la clave en el archivo JSON Debe ser un nombre semántico que no cambie durante la vida del modelo de fragmento de contenido.

Después de completar la creación del modelo de fragmento de contenido, debe terminar con una definición que tenga el siguiente aspecto:


![Modelo de fragmento de contenido de evento](assets/chapter-2/event-content-fragment-model.png)

## Siguiente paso

Si lo desea, instale [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) Paquete de contenido en AEM Author mediante [AEM Administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en esta parte del tutorial.

* [Capítulo 3: Creación de fragmentos de contenido de eventos](./chapter-3.md)
