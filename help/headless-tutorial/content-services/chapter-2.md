---
title: Capítulo 2 - Definición de modelos de fragmentos de contenido de eventos - Servicios de contenido
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: El capítulo 2 del tutorial AEM sin encabezado trata la habilitación y definición de los modelos de fragmento de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para la creación de eventos.
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: cfb7ed39ecb85998192ba854b34161f7e1dba19a
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 10%

---

# Capítulo 2 - Uso de modelos de fragmento de contenido

AEM modelos de fragmento de contenido definen esquemas de contenido que se pueden usar para crear plantillas de la creación de contenido sin procesar por parte de AEM autores. Este método es similar al scaffolding o a la creación basada en formularios. El concepto clave con los fragmentos de contenido es que el contenido creado no depende de la presentación, lo que significa que está pensado para un uso multicanal en el que la aplicación que lo consume, ya sea AEM, una aplicación de una sola página o una aplicación móvil, controle cómo se muestra el contenido al usuario.

La principal preocupación del fragmento de contenido es garantizar:

1. El contenido correcto se recopila del autor
2. El contenido se puede exponer en un formato estructurado y bien entendido a aplicaciones consumidoras.

Este capítulo trata sobre la activación y definición de modelos de fragmento de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para modelar y crear &quot;Eventos&quot;.

## Habilitar modelos de fragmento de contenido

Modelos de fragmento de contenido **must** se habilita mediante **[AEM [!UICONTROL Explorador de configuración]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=es)**.

Si los modelos de fragmento de contenido son **not** habilitado para una configuración, la variable **[!UICONTROL Crear] > [!UICONTROL Fragmento de contenido]** no aparece para la configuración de AEM correspondiente.

>[!NOTE]
>
>AEM configuraciones representan un conjunto de [configuraciones de inquilino según el contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) almacenado en `/conf`. Normalmente, las configuraciones de AEM se correlacionan con un sitio web concreto administrado en AEM Sites o con una unidad de negocio responsable de un subconjunto de contenido (activos, páginas, etc.) en AEM.
>
>Para que una configuración afecte a una jerarquía de contenido, se debe hacer referencia a la configuración a través de la variable `cq:conf` en esa jerarquía de contenido. (Esto se logra para el [!DNL WKND Mobile] configuración en **Paso 5** más abajo).
>
>Cuando la variable `global` se utiliza la configuración, se aplica a todo el contenido y `cq:conf` no es necesario configurarlo.
>
>Consulte la [[!UICONTROL Explorador de configuración] documentación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) para obtener más información.

1. Inicie sesión en AEM Author como usuario con los permisos adecuados para modificar la configuración relevante.
   * Para este tutorial, la variable **admin** se puede utilizar.
1. Vaya a **[!UICONTROL Herramienta] > [!UICONTROL General] > [!UICONTROL Explorador de configuración]**
1. Toque . **icono de carpeta** junto a **[!DNL WKND Mobile]** para seleccionar y, a continuación, toque el **[!UICONTROL Editar] botón** en la parte superior izquierda.
1. Select **[!UICONTROL Modelos de fragmento de contenido]** y toque **[!UICONTROL Guardar y cerrar]** en la parte superior derecha.

   Esto habilita los modelos de fragmento de contenido en los árboles de contenido de carpetas de recursos que tienen la variable [!DNL WKND Mobile] configuración aplicada.

   >[!NOTE]
   >
   >Este cambio de configuración no es reversible desde la variable [!UICONTROL Configuración AEM] Interfaz de usuario web. Para deshacer esta configuración:
   >    
   >    1. Apertura [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Vaya a `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Elimine el `models` node

   >    
   >Cualquier modelo de fragmento de contenido existente creado con esta configuración se eliminará, así como sus definiciones se almacenarán en `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique la variable **[!DNL WKND Mobile]** para **[!DNL WKND Mobile]Carpeta de recursos** para permitir que los fragmentos de contenido de los modelos de fragmento de contenido se creen dentro de esa jerarquía de carpetas de recursos:

   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Recursos] > [!UICONTROL Archivos]**
   1. Seleccione el **[!UICONTROL WKND Mobile] carpeta**
   1. Toque . **[!UICONTROL Propiedades]** en la barra de acciones superior para abrir [!UICONTROL Propiedades de carpeta]
   1. En [!UICONTROL Propiedades de carpeta], toque el **[!UICONTROL Cloud Services]** ficha
   1. Compruebe el **[!UICONTROL Configuración de nube]** el campo está definido como **/conf/wknd-mobile**
   1. Toque **[!UICONTROL Guardar y cerrar]** en la esquina superior derecha para mantener los cambios

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

>[!WARNING]
>
> __Modelos de fragmento de contenido__ se han movido de __Herramientas > Assets__ a __Herramientas > General__.

## Explicación del modelo de fragmento de contenido que se va a crear

Antes de definir el modelo de fragmento de contenido, vamos a revisar la experiencia que vamos a generar para asegurarnos de que estamos capturando todos los puntos de datos necesarios. Para ello, analizaremos el diseño de las aplicaciones móviles y asignaremos los elementos de diseño al contenido que se va a recopilar.

Podemos dividir los puntos de datos que definen un evento de la siguiente manera:

![Creación del modelo de fragmento de contenido](assets/chapter-2/design-to-model-mapping.png)

Con la asignación podemos definir el fragmento de contenido que se utilizará para recopilar y, en última instancia, exponer los datos de evento.

## Creación del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Modelos de fragmento de contenido]**.
1. Toque . **[!DNL WKND Mobile]** para abrir.
1. Toque **[!UICONTROL Crear]** para abrir el asistente de creación del modelo de fragmento de contenido.
1. Entrar **[!DNL Event]** como el **[!UICONTROL Título de modelo]** *(la descripción es opcional)* y toque **[!UICONTROL Crear]** para guardar.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definición de la estructura del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Modelos de fragmento de contenido] >[!DNL WKND]**.
1. Seleccione el **[!DNL Event]** Pulse y haga clic en el modelo de fragmento de contenido **[!UICONTROL Editar]** en la barra de acciones superior.
1. En el **[!UICONTROL Tipos de datos] ficha** a la derecha, arrastre el **[!UICONTROL Entrada de texto de una sola línea]** en la zona desplegable izquierda para definir la variable **[!DNL Question]** campo .
1. Asegúrese de que **[!UICONTROL Entrada de texto de una sola línea]** se selecciona a la izquierda y la variable **[!UICONTROL Propiedades] ficha** está seleccionado a la derecha. Rellene los campos Propiedades como se indica a continuación:

   * [!UICONTROL Procesar como] : `textfield`
   * [!UICONTROL Etiqueta de campo] : `Event Title`
   * [!UICONTROL Nombre de propiedad] : `eventTitle`
   * [!UICONTROL Longitud máxima] : 25
   * [!UICONTROL Requerido] : `Yes`

Repita estos pasos utilizando las definiciones de entrada definidas a continuación para crear el resto del Modelo de fragmento de contenido de evento.

>[!NOTE]
>
> La variable **Nombre de propiedad** Los campos DEBEN coincidir exactamente, ya que la aplicación de Android está programada para eliminar estos nombres.

### Descripción del evento

* [!UICONTROL Tipo de datos] : `Multi-line text`
* [!UICONTROL Etiqueta de campo] : `Event Description`
* [!UICONTROL Nombre de propiedad] : `eventDescription`
* [!UICONTROL Tipo predeterminado] : `Rich text`

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

### Precio de entrada

* [!UICONTROL Tipo de datos] : `Number`
* [!UICONTROL Procesar como] : `numberfield`
* [!UICONTROL Etiqueta de campo] : `Ticket Price`
* [!UICONTROL Nombre de propiedad] : `eventPrice`
* [!UICONTROL Tipo] : `Integer`
* [!UICONTROL Requerido] : `Yes`

### Imagen del evento

* [!UICONTROL Tipo de datos] : `Content Reference`
* [!UICONTROL Procesar como] : `contentreference`
* [!UICONTROL Etiqueta de campo] : `Event Image`
* [!UICONTROL Nombre de propiedad] : `eventImage`
* [!UICONTROL Ruta de acceso raíz] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Requerido] : `Yes`

### Nombre del lugar

* [!UICONTROL Tipo de datos] : `Single-line text`
* [!UICONTROL Procesar como] : `textfield`
* [!UICONTROL Etiqueta de campo] : `Venue Name`
* [!UICONTROL Nombre de propiedad] : `venueName`
* [!UICONTROL Longitud máxima] : 20
* [!UICONTROL Requerido] : `Yes`

### Ciudad del lugar

* [!UICONTROL Tipo de datos] : `Enumeration`
* [!UICONTROL Etiqueta de campo] : `Venue City`
* [!UICONTROL Nombre de propiedad] : `venueCity`
* [!UICONTROL Opciones] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>La variable **[!UICONTROL Nombre de propiedad]** denota que **both** el nombre de la propiedad JCR donde se almacenará este valor, así como la clave en el archivo JSON . Debe ser un nombre semántico que no cambie durante la vida del modelo de fragmento de contenido.

Después de completar la creación del modelo de fragmento de contenido, debe terminar con una definición similar a:


![Modelo de fragmento de contenido de evento](assets/chapter-2/event-content-fragment-model.png)

## Paso siguiente

De forma opcional, instale la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) paquete de contenido en AEM Author a través de [Administrador de paquetes AEM](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en esta parte del tutorial.

* [Capítulo 3 - Creación de fragmentos de contenido de eventos](./chapter-3.md)
