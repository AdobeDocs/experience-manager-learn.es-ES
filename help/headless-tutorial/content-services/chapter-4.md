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

AEM Tradicionalmente, las plantillas editables se utilizan para definir páginas web, aunque este uso es simplemente convencional. Las plantillas editables se pueden utilizar para componer **cualquiera** conjunto de contenido; cómo se accede a ese contenido: como HTML AEM SPA en un explorador, como JSON consumido por JavaScript (Editor de) o una aplicación móvil es una función del modo en que se solicita esa página.

AEM En los servicios de contenido de la, se utilizan plantillas editables para definir cómo se exponen los datos JSON.

Para el [!DNL WKND Mobile] Crearemos una sola plantilla editable que se utiliza para dirigir un único extremo de API. AEM Aunque este ejemplo es sencillo de ilustrar los conceptos de sin encabezado, puede crear varias páginas (o puntos finales), cada una de las cuales expone diferentes conjuntos de contenido para crear una API más compleja y mejor organizada.

## Explicación del punto final de la API

Para comprender cómo componer nuestro punto final de API y qué contenido debe exponerse a nuestras [!DNL WKND Mobile] App, vamos a revisar el diseño.

![Descomposición de página de API de eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, tenemos tres conjuntos lógicos de contenido para proporcionar a la aplicación móvil.

1. El **Logotipo**
2. El **Línea de etiqueta**
3. La lista de **Eventos**

AEM AEM Para ello, podemos asignar estos requisitos a Componentes de la (y, en nuestro caso, a Componentes principales de WCM) para exponer el contenido requerido como JSON.

1. El **Logotipo** se muestra mediante un **Componente de imagen**
2. El **Línea de etiqueta** aparece a través de un **Componente Texto**
3. La lista de **Eventos** aparece a través de un **Componente Lista de fragmentos de contenido** que, a su vez, hace referencia a un conjunto de Fragmentos de contenido de eventos.

>[!NOTE]
>
>AEM Para admitir la exportación de páginas y componentes JSON del servicio de contenido de la, las páginas y los componentes deben **AEM derivar de componentes principales de WCM de la**.
>
>[AEM Componentes principales de WCM](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) tienen funcionalidad integrada para admitir un esquema JSON normalizado de páginas y componentes exportados. AEM Todos los componentes de WKND Mobile utilizados en este tutorial (página, imagen, texto y lista de fragmentos de contenido) se derivan de los componentes principales de WCM de la.

## Definición de la plantilla de API de eventos

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**.

1. Cree el **[!DNL Events API]** plantilla:

   1. Tocar **[!UICONTROL Crear]** en la barra de acciones superior
   1. Seleccione el **[!DNL WKND Mobile - Empty Page]** plantilla
   1. Tocar **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Entrar **[!DNL Events API]** en el [!UICONTROL Título de plantilla] campo
   1. Tocar **[!UICONTROL Crear]** en la barra de acciones superior
   1. Tocar **[!UICONTROL Abrir]** abra la nueva plantilla para editarla

1. AEM En primer lugar, permitimos que los tres componentes identificados en la lista de componentes que necesitamos modelen el contenido editando el [!UICONTROL Política] de la raíz [!UICONTROL Contenedor de diseño]. Asegúrese de que **[!UICONTROL Estructura]** El modo está activo, seleccione la opción **[!DNL Layout Container \[Root\]]** y pulse el botón **[!UICONTROL Política]** botón.
1. En **[!UICONTROL Propiedades] > [!UICONTROL Componentes permitidos]** buscar **[!DNL WKND Mobile]**. Permitir los siguientes componentes de [!DNL WKND Mobile] grupo de componentes para que se puedan utilizar en [!DNL Events] página de API.

   * **[!DNL WKND Mobile > Image]**

      * El logotipo de la aplicación

   * **[!DNL WKND Mobile > Text]**

      * Texto introductorio de la aplicación

   * **[!DNL WKND Mobile > Content Fragment List]**

      * La lista de categorías de eventos disponibles para su visualización en la aplicación

1. Pulse el botón **[!UICONTROL Listo]** marca de verificación en la esquina superior derecha cuando esté completo.
1. **Actualizar** la ventana del explorador para ver los [!UICONTROL Componentes permitidos] en el carril izquierdo.
1. AEM En el buscador Componentes, en el carril izquierdo, arrastre los siguientes componentes de la:
   1. **[!DNL Image]** para el logotipo
   2. **[!DNL Text]** para la línea de etiquetas
   3. **[!DNL Content Fragment List]** para los eventos
1. **Para cada uno de los componentes anteriores**, selecciónelos y pulse el botón **desbloquear** botón.
1. Sin embargo, asegúrese de que **contenedor de diseño** es **bloqueado** para evitar que se agreguen otros componentes o que se eliminen estos tres componentes.
1. Tocar **[!UICONTROL Información de página] > [!UICONTROL Ver en administración]** para volver a la [!DNL WKND Mobile] listado de plantillas. Seleccione el recién creado **[!DNL Events API]** plantilla y pulse **[!UICONTROL Activar]** en la barra de acciones superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> Observe que los componentes utilizados para mostrar el contenido se añaden a la propia plantilla y se bloquean. Esto sirve para permitir a los autores editar los componentes predefinidos, pero no añadir ni eliminar componentes arbitrariamente, ya que el cambio de la propia API podría romper las suposiciones sobre la estructura JSON y romper las aplicaciones consumidoras. Todas las API deben ser estables.

## Pasos siguientes

Si lo desea, instale [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM paquete de contenido en el autor de la mediante [AEM Administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y en los capítulos anteriores del tutorial.

* [Capítulo 5: Creación de páginas de servicios de contenido](./chapter-5.md)
