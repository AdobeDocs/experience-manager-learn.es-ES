---
title: Capítulo 2 - Definición de modelos de fragmentos de contenido de Evento - Servicios de contenido
seo-title: Introducción a Servicios de contenido de AEM - Capítulo 2 - Definición de modelos de fragmentos de contenido de Evento
description: El capítulo 2 del tutorial sin encabezado de AEM cubre la habilitación y definición de modelos de fragmentos de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para crear Eventos.
seo-description: El capítulo 2 del tutorial sin encabezado de AEM cubre la habilitación y definición de modelos de fragmentos de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para crear Eventos.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 7%

---


# Capítulo 2 - Uso de modelos de fragmentos de contenido

Los modelos de fragmentos de contenido de AEM definen esquemas de contenido que pueden utilizarse para crear plantillas de contenido sin procesar por AEM autores. Este enfoque es similar al andamiaje o a la creación basada en formularios. El concepto clave con Fragmentos de contenido es que el contenido creado depende de la presentación, lo que significa que está pensado para un uso de varios canales, ya que la aplicación que lo consume, ya sea AEM, una aplicación de una sola página o una aplicación móvil, controla cómo se muestra el contenido al usuario.

La principal preocupación del fragmento de contenido es garantizar:

1. El contenido correcto se recopila del autor
2. El contenido se puede exponer en un formato estructurado y bien entendido a las aplicaciones que lo consumen.

Este capítulo trata la habilitación y definición de modelos de fragmento de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para modelar y crear &quot;Eventos&quot;.

## Habilitar modelos de fragmento de contenido

Los modelos de fragmento de contenido **deben** habilitarse mediante **[AEM [!UICONTROL Navegador de configuración]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)**.

Si los modelos de fragmento de contenido **no** están habilitados para una configuración, el botón **[!UICONTROL Crear] > [!UICONTROL Fragmento de contenido]** no aparecerá para la configuración de AEM pertinente.

>[!NOTE]
>
>AEM configuraciones representan un conjunto de [configuraciones de inquilinos según el contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) almacenadas en `/conf`. Normalmente, las configuraciones de AEM se correlacionan con un sitio Web en particular administrado en AEM Sites o con una unidad de negocio responsable de un subconjunto de contenido (recursos, páginas, etc.) en AEM.
>
>Para que una configuración afecte a una jerarquía de contenido, se debe hacer referencia a la configuración mediante la propiedad `cq:conf` de esa jerarquía de contenido. (Esto se logra para la configuración [!DNL WKND Mobile] en **Paso 5** a continuación).
>
>Cuando se utiliza la configuración `global`, la configuración se aplica a todo el contenido y no es necesario establecer `cq:conf`.
>
>Consulte la documentación de [[!UICONTROL Configuration Browser]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html) para obtener más información.

1. Inicie sesión en AEM Author como usuario con los permisos adecuados para modificar la configuración pertinente.
   * Para este tutorial, se puede utilizar el usuario **admin**.
1. Vaya a **[!UICONTROL Herramienta] > [!UICONTROL General] > [!UICONTROL Navegador de configuración]**
1. Toque el icono **carpeta** situado junto a **[!DNL WKND Mobile]** para seleccionarlo y, a continuación, toque el botón **[!UICONTROL Editar]** en la parte superior izquierda.
1. Seleccione **[!UICONTROL Modelos de fragmento de contenido]** y toque **[!UICONTROL Guardar y cerrar]** en la parte superior derecha.

   Esto habilita los modelos de fragmento de contenido en los árboles de contenido de la carpeta de recursos que tienen aplicada la configuración [!DNL WKND Mobile].

   >[!NOTE]
   >
   >Este cambio de configuración no es reversible desde la [!UICONTROL interfaz de usuario web de configuración de AEM]. Para deshacer esta configuración:
   >    
   >    1. Abrir [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Ir a `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Eliminar el nodo `models`

   >    
   >Se eliminarán todos los modelos de fragmento de contenido creados con esta configuración y sus definiciones se almacenarán en `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique la configuración **[!DNL WKND Mobile]** a la carpeta **[!DNL WKND Mobile]Assets** para permitir que los fragmentos de contenido de los modelos de fragmentos de contenido se creen dentro de la jerarquía de carpetas Recursos:

   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Recursos] > [!UICONTROL Archivos]**
   1. Seleccione la carpeta **[!UICONTROL WKND Mobile]**
   1. Toque el botón **[!UICONTROL Properties]** en la barra de acciones superior para abrir [!UICONTROL Folder Properties]
   1. En [!UICONTROL Propiedades de la carpeta], toque la ficha **[!UICONTROL Cloud Services]**
   1. Verifique que el campo **[!UICONTROL Configuración de la nube]** esté establecido en **/conf/wknd-mobile**
   1. Toque **[!UICONTROL Guardar y cerrar]** en la esquina superior derecha para continuar con los cambios

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## Explicación del modelo de fragmento de contenido que se va a crear

Antes de definir el modelo de fragmento de contenido, vamos a revisar la experiencia que vamos a generar para asegurarnos de que estamos capturando todos los puntos de datos necesarios. Para ello, analizaremos el diseño de las aplicaciones móviles y asignaremos los elementos de diseño al contenido que se va a recopilar.

Podemos desglosar los puntos de datos que definen un Evento de la siguiente manera:

![Creación del modelo de fragmento de contenido](assets/chapter-2/design-to-model-mapping.png)

Con la asignación podemos definir el fragmento de contenido que se utilizará para recopilar y, en última instancia, exponer los datos de Evento.

## Creación del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL Recursos] > [!UICONTROL Modelos de fragmento de contenido]**.
1. Toque la carpeta **[!DNL WKND Mobile]** para abrirla.
1. Toque **[!UICONTROL Crear]** para abrir el asistente para la creación del modelo de fragmento de contenido.
1. Escriba **[!DNL Event]** como **[!UICONTROL Título del modelo]** *(la descripción es opcional)* y toque **[!UICONTROL Crear]** para guardar.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definición de la estructura del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL Recursos] > [!UICONTROL Modelos de fragmento de contenido] >[!DNL WKND]**.
1. Seleccione el modelo de fragmento de contenido **[!DNL Event]** y toque **[!UICONTROL Editar]** en la barra de acciones superior.
1. En la ficha **[!UICONTROL Tipos de datos]** de la derecha, arrastre la **[!UICONTROL entrada de texto de una sola línea]** a la zona desplegable izquierda para definir el campo **[!DNL Question]**.
1. Asegúrese de que la nueva **[!UICONTROL entrada de texto de una sola línea]** está seleccionada a la izquierda y la ficha **[!UICONTROL Propiedades]** está seleccionada a la derecha. Rellene los campos Propiedades como se indica a continuación:

   * [!UICONTROL Procesar como] : `textfield`
   * [!UICONTROL Etiqueta de campo] : `Event Title`
   * [!UICONTROL Nombre de propiedad] : `eventTitle`
   * [!UICONTROL Longitud]  máxima: 25
   * [!UICONTROL Requerido] : `Yes`

Repita estos pasos utilizando las definiciones de entrada definidas a continuación para crear el resto del modelo de fragmento de contenido de Evento.

>[!NOTE]
>
> Los campos **Nombre de propiedad** DEBEN coincidir exactamente, ya que la aplicación de Android se programa para eliminar estos nombres.

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
* [!UICONTROL Opciones] :  `Art,Music,Performance,Photography`

### Precio de entrada

* [!UICONTROL Tipo de datos] : `Number`
* [!UICONTROL Procesar como] : `numberfield`
* [!UICONTROL Etiqueta de campo] : `Ticket Price`
* [!UICONTROL Nombre de propiedad] : `eventPrice`
* [!UICONTROL Tipo] : `Integer`
* [!UICONTROL Requerido] : `Yes`

### Imagen de evento

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
* [!UICONTROL Longitud]  máxima: 20
* [!UICONTROL Requerido] : `Yes`

### Ciudad del lugar

* [!UICONTROL Tipo de datos] : `Enumeration`
* [!UICONTROL Etiqueta de campo] : `Venue City`
* [!UICONTROL Nombre de propiedad] : `venueCity`
* [!UICONTROL Opciones] :  `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>El **[!UICONTROL Nombre de propiedad]** denota el **tanto** nombre de propiedad JCR donde se almacenará este valor como la clave en el archivo JSON. Debe ser un nombre semántico que no cambie durante la vida del modelo de fragmento de contenido.

Después de completar la creación del modelo de fragmento de contenido, debe terminar con una definición similar a:


![Modelo de fragmento de contenido de evento](assets/chapter-2/event-content-fragment-model.png)

## Paso siguiente

Opcionalmente, instale el paquete de contenido [com.adobe.aem.guide.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en AEM Author mediante [Administrador de paquetes AEM](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido que se describen en esta parte del tutorial.

* [Capítulo 3 - Creación de fragmentos de contenido de Evento](./chapter-3.md)
