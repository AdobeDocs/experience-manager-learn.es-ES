---
title: Capítulo 2 - Definición de modelos de fragmentos de contenido de eventos - Servicios de contenido
seo-title: Introducción a los servicios de contenido de AEM - Capítulo 2 - Definición de modelos de fragmentos de contenido de eventos
description: El capítulo 2 del tutorial AEM Headless cubre la habilitación y definición de modelos de fragmento de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para crear eventos.
seo-description: El capítulo 2 del tutorial AEM Headless cubre la habilitación y definición de modelos de fragmento de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para crear eventos.
feature: Fragmentos de contenido, API
topic: Sin objetivos, Administración de contenido
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1004'
ht-degree: 7%

---


# Capítulo 2 - Uso de modelos de fragmento de contenido

Los modelos de fragmento de contenido de AEM definen esquemas de contenido que se pueden utilizar para crear plantillas de la creación de contenido sin procesar por parte de los autores de AEM. Este método es similar al scaffolding o a la creación basada en formularios. El concepto clave con los fragmentos de contenido es que el contenido creado no depende de la presentación, lo que significa que está pensado para un uso multicanal en el que la aplicación consumidora, ya sea AEM, una aplicación de una sola página o una aplicación móvil, controla cómo se muestra el contenido al usuario.

La principal preocupación del fragmento de contenido es garantizar:

1. El contenido correcto se recopila del autor
2. El contenido se puede exponer en un formato estructurado y bien entendido a aplicaciones consumidoras.

Este capítulo trata sobre la activación y definición de modelos de fragmento de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para modelar y crear &quot;Eventos&quot;.

## Habilitar modelos de fragmento de contenido

Los modelos de fragmento de contenido **deben** habilitarse mediante el **[Navegador de configuración [!UICONTROL AEM]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)**.

Si los modelos de fragmento de contenido **no** están habilitados para una configuración, el botón **[!UICONTROL Crear] > [!UICONTROL Fragmento de contenido]** no aparecerá para la configuración de AEM relevante.

>[!NOTE]
>
>Las configuraciones de AEM representan un conjunto de [configuraciones de inquilino según el contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) almacenadas en `/conf`. Normalmente, las configuraciones de AEM se correlacionan con un sitio web concreto administrado en AEM Sites o con una unidad empresarial responsable de un subconjunto de contenido (activos, páginas, etc.) en AEM.
>
>Para que una configuración afecte a una jerarquía de contenido, se debe hacer referencia a la configuración a través de la propiedad `cq:conf` en esa jerarquía de contenido. (Esto se logra para la configuración [!DNL WKND Mobile] en **Paso 5** a continuación).
>
>Cuando se utiliza la configuración `global`, la configuración se aplica a todo el contenido y no es necesario configurar `cq:conf`.
>
>Consulte la documentación [[!UICONTROL Configuration Browser]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html) para obtener más información.

1. Inicie sesión en AEM Author como usuario con los permisos adecuados para modificar la configuración relevante.
   * Para este tutorial, se puede utilizar el usuario **admin** .
1. Vaya a **[!UICONTROL Herramienta] > [!UICONTROL General] > [!UICONTROL Explorador de configuración]**
1. Pulse el **icono de carpeta** situado junto a **[!DNL WKND Mobile]** para seleccionarlo y, a continuación, pulse el botón **[!UICONTROL Editar]** en la parte superior izquierda.
1. Seleccione **[!UICONTROL Modelos de fragmento de contenido]** y pulse **[!UICONTROL Guardar y cerrar]** en la parte superior derecha.

   Esto habilita los modelos de fragmento de contenido en árboles de contenido de carpetas de recursos que tienen aplicada la configuración [!DNL WKND Mobile] .

   >[!NOTE]
   >
   >Este cambio de configuración no es reversible desde la interfaz de usuario web [!UICONTROL AEM Configuration]. Para deshacer esta configuración:
   >    
   >    1. Abra [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Ir a `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Eliminar el nodo `models`

   >    
   >Cualquier modelo de fragmento de contenido existente creado con esta configuración se eliminará, así como sus definiciones se almacenarán en `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique la configuración **[!DNL WKND Mobile]** a la **[!DNL WKND Mobile]carpeta de recursos** para permitir que los fragmentos de contenido de los modelos de fragmento de contenido se creen dentro de esa jerarquía de carpetas de recursos:

   1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Archivos]**
   1. Seleccione la carpeta **[!UICONTROL WKND Mobile]**
   1. Pulse el botón **[!UICONTROL Properties]** en la barra de acciones superior para abrir [!UICONTROL Folder Properties]
   1. En [!UICONTROL Propiedades de carpeta], pulse la pestaña **[!UICONTROL Cloud Services]**
   1. Compruebe que el campo **[!UICONTROL Cloud Configuration]** está configurado en **/conf/wknd-mobile**
   1. Toque **[!UICONTROL Guardar y cerrar]** en la esquina superior derecha para mantener los cambios

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## Explicación del modelo de fragmento de contenido que se va a crear

Antes de definir el modelo de fragmento de contenido, vamos a revisar la experiencia que vamos a generar para asegurarnos de que estamos capturando todos los puntos de datos necesarios. Para ello, analizaremos el diseño de las aplicaciones móviles y asignaremos los elementos de diseño al contenido que se va a recopilar.

Podemos dividir los puntos de datos que definen un evento de la siguiente manera:

![Creación del modelo de fragmento de contenido](assets/chapter-2/design-to-model-mapping.png)

Con la asignación podemos definir el fragmento de contenido que se utilizará para recopilar y, en última instancia, exponer los datos de evento.

## Creación del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL Recursos] > [!UICONTROL Modelos de fragmento de contenido]**.
1. Pulse la carpeta **[!DNL WKND Mobile]** para abrirla.
1. Pulse **[!UICONTROL Crear]** para abrir el asistente de creación del modelo de fragmento de contenido.
1. Introduzca **[!DNL Event]** como **[!UICONTROL Model Title]** *(la descripción es opcional)* y pulse **[!UICONTROL Create]** para guardar.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definición de la estructura del modelo de fragmento de contenido

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL Recursos] > [!UICONTROL Modelos de fragmento de contenido] >[!DNL WKND]**.
1. Seleccione el Modelo de fragmento de contenido **[!DNL Event]** y pulse **[!UICONTROL Editar]** en la barra de acciones superior.
1. En la pestaña **[!UICONTROL Data Types]** de la derecha, arrastre el **[!UICONTROL Single line text input]** a la zona desplegable izquierda para definir el campo **[!DNL Question]**.
1. Asegúrese de que la nueva **[!UICONTROL Entrada de texto de una sola línea]** está seleccionada a la izquierda y la **[!UICONTROL pestaña ]** está seleccionada a la derecha. Rellene los campos Propiedades como se indica a continuación:

   * [!UICONTROL Procesar como] : `textfield`
   * [!UICONTROL Etiqueta de campo] : `Event Title`
   * [!UICONTROL Nombre de propiedad] : `eventTitle`
   * [!UICONTROL Longitud]  máxima: 25
   * [!UICONTROL Requerido] : `Yes`

Repita estos pasos utilizando las definiciones de entrada definidas a continuación para crear el resto del Modelo de fragmento de contenido de evento.

>[!NOTE]
>
> Los campos **Property Name** DEBEN coincidir exactamente, ya que la aplicación de Android está programada para eliminar la clave de estos nombres.

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
>El **[!UICONTROL Nombre de propiedad]** denota el **ambos** nombre de propiedad JCR donde se almacenará este valor, así como la clave en el archivo JSON . Debe ser un nombre semántico que no cambie durante la vida del modelo de fragmento de contenido.

Después de completar la creación del modelo de fragmento de contenido, debe terminar con una definición similar a:


![Modelo de fragmento de contenido de evento](assets/chapter-2/event-content-fragment-model.png)

## Paso siguiente

Opcionalmente, instale el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en AEM Author a través del [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en esta parte del tutorial.

* [Capítulo 3 - Creación de fragmentos de contenido de eventos](./chapter-3.md)
