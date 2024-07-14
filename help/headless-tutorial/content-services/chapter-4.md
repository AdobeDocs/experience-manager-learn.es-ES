---
title: 'Capítulo 4: Definición de plantillas de servicios de contenido: servicios de contenido'
description: AEM AEM AEM El capítulo 4 del tutorial sin encabezado de la cubre la función de las plantillas editables de la aplicación en el contexto de los servicios de contenido de la. AEM Las plantillas editables se utilizan para definir la estructura de contenido JSON que, en última instancia, exponen los servicios de contenido.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 245
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# Capítulo 4: Definición de plantillas de servicios de contenido

AEM AEM AEM El capítulo 4 del tutorial sin encabezado de la cubre la función de las plantillas editables de la aplicación en el contexto de los servicios de contenido de la. AEM AEM Las plantillas editables se utilizan para definir la estructura de contenido JSON que expone Content Services a los clientes a través de la composición de los componentes de contenido habilitados para los servicios de contenido.

## AEM Explicación de la función que desempeñan las plantillas en los servicios de contenido de

AEM Las plantillas editables se utilizan para definir los puntos finales HTTP a los que se accede para exponer el contenido del evento como JSON.

AEM Tradicionalmente, las plantillas editables se utilizan para definir páginas web, aunque este uso es simplemente una convención. Las plantillas editables se pueden usar para componer **cualquier** conjunto de contenido; cómo se accede a ese contenido: como HTML en un explorador, como JSON consumido por JavaScript AEM SPA (Editor de) o como aplicación móvil es una función del modo en que se solicita esa página.

AEM En los servicios de contenido de la, se utilizan plantillas editables para definir cómo se exponen los datos JSON.

Para la aplicación [!DNL WKND Mobile], crearemos una sola plantilla editable que se utilizará para dirigir un único extremo de API. AEM Aunque este ejemplo es sencillo de ilustrar los conceptos de sin encabezado, puede crear varias páginas (o puntos finales), cada una de las cuales expone diferentes conjuntos de contenido para crear una API más compleja y mejor organizada.

## Explicación del punto final de la API

Para comprender cómo componer el extremo de la API y qué contenido debería exponerse en la aplicación [!DNL WKND Mobile], volvamos a revisar el diseño.

![Descomposición de página de API de eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, tenemos tres conjuntos lógicos de contenido para proporcionar a la aplicación móvil.

1. El **logotipo**
2. La **Línea De Etiquetas**
3. La lista de **eventos**

AEM AEM Para ello, podemos asignar estos requisitos a Componentes de la (y, en nuestro caso, a Componentes principales de WCM) para exponer el contenido requerido como JSON.

1. El **logotipo** aparece a través de un **componente de imagen**
2. La **línea de etiqueta** aparece a través de un **componente de texto**
3. La lista de **Eventos** aparece a través de un **componente Lista de fragmentos de contenido** que, a su vez, hace referencia a un conjunto de fragmentos de contenido de eventos.

>[!NOTE]
>
>AEM AEM Para admitir la exportación de páginas y componentes JSON del servicio de contenido de la, las páginas y los componentes deben **derivarse de los componentes principales de WCM de la página de destino**.
>
>AEM [Los componentes principales de WCM de ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) tienen funcionalidad integrada para admitir un esquema JSON normalizado de páginas y componentes exportados. AEM Todos los componentes de WKND Mobile utilizados en este tutorial (página, imagen, texto y lista de fragmentos de contenido) se derivan de los componentes principales de WCM en uso para la creación de la aplicación de la manera más sencilla posible.

## Definición de la plantilla de API de eventos

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**.

1. Crear la plantilla **[!DNL Events API]**:

   1. Pulse **[!UICONTROL Crear]** en la barra de acciones superior
   1. Seleccionar la plantilla **[!DNL WKND Mobile - Empty Page]**
   1. Pulse **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Escriba **[!DNL Events API]** en el campo [!UICONTROL Título de plantilla]
   1. Pulse **[!UICONTROL Crear]** en la barra de acciones superior
   1. Pulse **[!UICONTROL Abrir]** para abrir la nueva plantilla y editarla

1. AEM En primer lugar, permitimos que los tres componentes de diseño identificados que necesitamos modelen el contenido editando la [!UICONTROL directiva] del [!UICONTROL contenedor de diseño raíz]. Asegúrese de que el modo **[!UICONTROL Estructura]** esté activo, seleccione **[!DNL Layout Container \[Root\]]** y pulse el botón **[!UICONTROL Directiva]**.
1. En **[!UICONTROL Propiedades] > [!UICONTROL Componentes permitidos]**, busque **[!DNL WKND Mobile]**. Permitir los siguientes componentes del grupo de componentes [!DNL WKND Mobile] para que se puedan usar en la página de API [!DNL Events].

   * **[!DNL WKND Mobile > Image]**

      * El logotipo de la aplicación

   * **[!DNL WKND Mobile > Text]**

      * Texto introductorio de la aplicación

   * **[!DNL WKND Mobile > Content Fragment List]**

      * La lista de categorías de eventos disponibles para su visualización en la aplicación

1. Pulse la marca de verificación **[!UICONTROL Listo]** en la esquina superior derecha cuando haya terminado.
1. **Actualice** la ventana del explorador para ver los [!UICONTROL componentes permitidos] nuevos en el carril izquierdo.
1. AEM En el buscador Componentes, en el carril izquierdo, arrastre los siguientes componentes de la:
   1. **[!DNL Image]** para el logotipo
   2. **[!DNL Text]** para la línea de etiqueta
   3. **[!DNL Content Fragment List]** para los eventos
1. **Para cada uno de los componentes anteriores**, selecciónelos y presione el botón **desbloquear**.
1. Sin embargo, asegúrese de que el **contenedor de diseño** esté **bloqueado** para evitar que se agreguen otros componentes o que se quiten estos tres componentes.
1. Pulse **[!UICONTROL Información de la página] > [!UICONTROL Ver en el administrador]** para volver a la lista de plantillas [!DNL WKND Mobile]. Seleccione la plantilla **[!DNL Events API]** recién creada y pulse **[!UICONTROL Habilitar]** en la barra de acciones superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> Observe que los componentes utilizados para mostrar el contenido se añaden a la propia plantilla y se bloquean. Esto sirve para permitir a los autores editar los componentes predefinidos, pero no añadir ni eliminar componentes arbitrariamente, ya que el cambio de la propia API podría romper las suposiciones sobre la estructura JSON y romper las aplicaciones consumidoras. Todas las API deben ser estables.

## Pasos siguientes

AEM AEM De forma opcional, instale el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en Author o a través del Administrador de paquetes de [](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y en los capítulos anteriores del tutorial.

* [Capítulo 5: Creación de páginas de servicios de contenido](./chapter-5.md)
