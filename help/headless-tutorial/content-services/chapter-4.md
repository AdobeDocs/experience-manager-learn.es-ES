---
title: Capítulo 4 - Definición de plantillas de servicios de contenido - Servicios de contenido
seo-title: Introducción a AEM Headless - Capítulo 4 - Definición de plantillas de servicios de contenido
description: El capítulo 4 del tutorial sin encabezado de AEM cubre el papel de las plantillas editables de AEM en el contexto de los servicios de contenido de AEM. Las plantillas editables se utilizan para definir la estructura de contenido JSON que los servicios de contenido de AEM expondrán en última instancia.
seo-description: El capítulo 4 del tutorial sin encabezado de AEM cubre el papel de las plantillas editables de AEM en el contexto de los servicios de contenido de AEM. Las plantillas editables se utilizan para definir la estructura de contenido JSON que los servicios de contenido de AEM expondrán en última instancia.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '837'
ht-degree: 0%

---


# Capítulo 4 - Definición de plantillas de servicios de contenido

El capítulo 4 del tutorial sin encabezado de AEM cubre el papel de las plantillas editables de AEM en el contexto de los servicios de contenido de AEM. Las plantillas editables se utilizan para definir la estructura de contenido JSON que los servicios de contenido de AEM exponen a los clientes mediante la composición de los componentes de AEM habilitados para los servicios de contenido.

## Explicación de la función de las plantillas en los servicios de contenido de AEM

Las plantillas editables de AEM se utilizan para definir los puntos finales HTTP a los que se accederá para exponer el contenido del evento como JSON.

Tradicionalmente, las plantillas editables de AEM se utilizan para definir páginas web, aunque este uso es simplemente una convención. Las plantillas editables se pueden utilizar para componer **cualquier** conjunto de contenido; cómo se accede a ese contenido: como HTML en un explorador, como JSON consumido por JavaScript (AEM SPA Editor) o una aplicación móvil es una función del modo en que se solicita esa página.

En los servicios de contenido de AEM, se utilizan plantillas editables para definir cómo se exponen los datos JSON.

Para la aplicación [!DNL WKND Mobile], crearemos una única plantilla editable que se utilizará para dirigir un único extremo de API. Aunque este ejemplo es sencillo de ilustrar los conceptos de AEM sin encabezado, puede crear varias páginas (o puntos finales) cada una que exponga diferentes conjuntos de contenido para crear una API más compleja y mejor organizada.

## Explicación del punto final de la API

Para comprender cómo componer nuestro punto final de API y comprender qué contenido debe exponerse a nuestra aplicación [!DNL WKND Mobile], volvamos a examinar el diseño.

![Descomposición de página de la API de eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, tenemos tres conjuntos lógicos de contenido para proporcionar a la aplicación móvil.

1. El **logotipo**
2. La **Línea de etiqueta**
3. La lista de **Eventos**

Para ello, podemos asignar estos requisitos a los componentes AEM (y, en nuestro caso, a los componentes principales de WCM de AEM) para exponer el contenido necesario como JSON.

1. El **Logotipo** se mostrará a través de un **Componente de imagen**
2. La **Línea de etiqueta** se mostrará a través de un **Componente de texto**
3. La lista de **Eventos** se mostrará a través de un **componente Lista de fragmentos de contenido** que, a su vez, hace referencia a un conjunto de fragmentos de contenido de eventos.

>[!NOTE]
>
>Para admitir la exportación JSON de páginas y componentes del servicio de contenido de AEM, las páginas y los componentes deben **derivarse de los componentes principales de WCM de AEM**.
>
>[Los componentes principales de WCM de AEM ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) tienen una funcionalidad integrada que admite un esquema JSON normalizado de páginas y componentes exportados. Todos los componentes de WKND Mobile utilizados en este tutorial (Página, Imagen, Texto y Lista de fragmentos de contenido) se derivan de los componentes principales de WCM de AEM.

## Definición de la plantilla de API de eventos

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**.

1. Cree la plantilla **[!DNL Events API]**:

   1. Toque **[!UICONTROL Crear]** en la barra de acciones superior
   1. Seleccione la plantilla **[!DNL WKND Mobile - Empty Page]**
   1. Toque **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Introduzca **[!DNL Events API]** en el campo [!UICONTROL Template Title]
   1. Toque **[!UICONTROL Crear]** en la barra de acciones superior
   1. Toque **[!UICONTROL Abrir]** para abrir la nueva plantilla y editarla

1. En primer lugar, permitimos los tres componentes de AEM identificados que necesitamos para modelar el contenido editando la [!UICONTROL Política] del [!UICONTROL Contenedor de diseño] raíz. Asegúrese de que el modo **[!UICONTROL Structure]** está activo, seleccione **[!DNL Layout Container \[Root\]]** y pulse el botón **[!UICONTROL Policy]**.
1. En **[!UICONTROL Propiedades] > [!UICONTROL Componentes permitidos]** busque **[!DNL WKND Mobile]**. Habilite los siguientes componentes del grupo de componentes [!DNL WKND Mobile] para que se puedan utilizar en la página de API [!DNL Events].

   * **[!DNL WKND Mobile > Image]**

      * El logotipo de la aplicación
   * **[!DNL WKND Mobile > Text]**

      * Texto introductorio de la aplicación
   * **[!DNL WKND Mobile > Content Fragment List]**

      * La lista de categorías de eventos disponibles para mostrar en la aplicación



1. Toque la marca de verificación **[!UICONTROL Listo]** en la esquina superior derecha cuando termine.
1. **** Actualice la ventana del explorador para ver la nueva  [!UICONTROL lista ] Componentes permitidos en el carril izquierdo.
1. En el buscador de componentes del carril izquierdo, arrastre los siguientes componentes de AEM:
   1. **[!DNL Image]** para el logotipo
   2. **[!DNL Text]** para la línea de etiquetas
   3. **[!DNL Content Fragment List]** para los eventos
1. **Para cada uno de los componentes** anteriores, selecciónelos y pulse el  **** botón de desbloqueo.
1. Sin embargo, asegúrese de que el **contenedor de diseño** esté **bloqueado** para evitar que se agreguen otros componentes o que se eliminen estos tres componentes.
1. Pulse **[!UICONTROL Información de página] > [!UICONTROL Ver en Administración]** para volver a la lista de plantillas [!DNL WKND Mobile]. Seleccione la plantilla **[!DNL Events API]** recién creada y pulse **[!UICONTROL Habilitar]** en la barra de acciones superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> Observe que los componentes utilizados para mostrar el contenido se añaden a la plantilla misma y se bloquean. Esto permite a los autores editar los componentes predefinidos, pero no añadir ni eliminar componentes de forma arbitraria, ya que cambiar la propia API podría alterar las suposiciones sobre la estructura de JSON y dejar de consumir aplicaciones. Todas las API deben ser estables.

## Pasos siguientes

Opcionalmente, instale el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en AEM Author a través del [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y los capítulos anteriores del tutorial.

* [Capítulo 5 - Creación de páginas de servicios de contenido](./chapter-5.md)
