---
title: Capítulo 4 - Definición de plantillas de servicios de contenido - Servicios de contenido
description: El capítulo 4 del tutorial AEM sin encabezado cubre el papel de AEM plantillas editables en el contexto de los servicios de contenido de AEM. Las plantillas editables se utilizan para definir la estructura de contenido JSON AEM los servicios de contenido se exponen finalmente.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# Capítulo 4 - Definición de plantillas de servicios de contenido

El capítulo 4 del tutorial AEM sin encabezado cubre el papel de AEM plantillas editables en el contexto de los servicios de contenido de AEM. Las plantillas editables se utilizan para definir la estructura de contenido JSON AEM los servicios de contenido exponen a los clientes a través de la composición de los servicios de contenido habilitados AEM componentes.

## Explicación de la función de las plantillas en los servicios de contenido AEM

AEM Se utilizan plantillas editables para definir los puntos finales HTTP a los que se accede para exponer el contenido del evento como JSON.

Tradicionalmente, AEM plantillas editables se utilizan para definir páginas web, aunque este uso es simplemente una convención. Las plantillas editables se pueden usar para componer **any** conjunto de contenidos; cómo se accede a ese contenido: como HTML en un explorador, como JSON consumido por JavaScript (AEM SPA Editor) o una aplicación móvil es una función del modo en que se solicita esa página.

En AEM Content Services, se utilizan plantillas editables para definir cómo se exponen los datos JSON.

Para la variable [!DNL WKND Mobile] En la aplicación, crearemos una única plantilla editable que se utilizará para dirigir un único extremo de API. Aunque este ejemplo es sencillo de ilustrar los conceptos de AEM sin encabezado, puede crear varias páginas (o puntos finales) cada una con diferentes conjuntos de contenido para crear una API más compleja y mejor organizada.

## Explicación del punto final de la API

Para comprender cómo componer nuestro punto final de API y qué contenido debe exponerse a nuestra [!DNL WKND Mobile] App, volvamos a revisar el diseño.

![Descomposición de página de la API de eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, tenemos tres conjuntos lógicos de contenido para proporcionar a la aplicación móvil.

1. La variable **Logotipo**
2. La variable **Línea de etiquetas**
3. La lista de **Eventos**

Para ello, podemos asignar estos requisitos a AEM componentes (y, en nuestro caso, AEM componentes principales de WCM) para exponer el contenido necesario como JSON.

1. La variable **Logotipo** se muestra a través de un **Componente de imagen**
2. La variable **Línea de etiquetas** se muestra a través de una **Componente de texto**
3. La lista de **Eventos** se muestra a través de una **Componente Lista de fragmentos de contenido** que, a su vez, hace referencia a un conjunto de fragmentos de contenido de evento.

>[!NOTE]
>
>Para admitir AEM exportación JSON de páginas y componentes del servicio de contenido, las páginas y los componentes deben **derivar de AEM componentes principales de WCM**.
>
>[Componentes principales AEM WCM](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) tienen una funcionalidad integrada para admitir un esquema JSON normalizado de páginas y componentes exportados. Todos los componentes de WKND Mobile utilizados en este tutorial (Página, Imagen, Texto y Lista de fragmentos de contenido) se derivan de AEM componentes principales de WCM.

## Definición de la plantilla de API de eventos

1. Vaya a **[!UICONTROL Herramientas] > [!UICONTROL General] > [!UICONTROL Plantillas] >[!DNL WKND Mobile]**.

1. Cree la variable **[!DNL Events API]** plantilla:

   1. Toque **[!UICONTROL Crear]** en la barra de acciones superior
   1. Seleccione el **[!DNL WKND Mobile - Empty Page]** plantilla
   1. Toque **[!UICONTROL Siguiente]** en la barra de acciones superior
   1. Entrar **[!DNL Events API]** en el [!UICONTROL Título de plantilla] field
   1. Toque **[!UICONTROL Crear]** en la barra de acciones superior
   1. Toque **[!UICONTROL Apertura]** abra la nueva plantilla para editarla

1. En primer lugar, se permiten los tres componentes AEM identificados que necesitamos para modelar el contenido editando el [!UICONTROL Política] de la raíz [!UICONTROL Contenedor de diseño]. Asegúrese de que la variable **[!UICONTROL Estructura]** está activo, seleccione la opción **[!DNL Layout Container \[Root\]]** y pulse el botón **[!UICONTROL Política]** botón.
1. En **[!UICONTROL Propiedades] > [!UICONTROL Componentes permitidos]** buscar **[!DNL WKND Mobile]**. Permitir los siguientes componentes desde el [!DNL WKND Mobile] grupo de componentes para que se puedan usar en la variable [!DNL Events] página API.

   * **[!DNL WKND Mobile > Image]**

      * El logotipo de la aplicación
   * **[!DNL WKND Mobile > Text]**

      * Texto introductorio de la aplicación
   * **[!DNL WKND Mobile > Content Fragment List]**

      * La lista de categorías de eventos disponibles para mostrar en la aplicación



1. Toque . **[!UICONTROL Listo]** marca de verificación en la esquina superior derecha cuando se complete.
1. **Actualizar** la ventana del navegador para ver [!UICONTROL Componentes permitidos] en el carril izquierdo.
1. Desde el buscador Componentes en el carril izquierdo, arrastre los siguientes componentes AEM:
   1. **[!DNL Image]** para el logotipo
   2. **[!DNL Text]** para la línea de etiquetas
   3. **[!DNL Content Fragment List]** para los eventos
1. **Para cada uno de los componentes anteriores**, selecciónelos y presione la tecla **desbloquear** botón.
1. Sin embargo, asegúrese de que **contenedor de diseño** es **bloqueado** para evitar que se agreguen otros componentes o que se eliminen estos tres componentes.
1. Toque **[!UICONTROL Información de la página] > [!UICONTROL Ver en Administración]** para volver a la [!DNL WKND Mobile] lista de plantillas. Seleccione el **[!DNL Events API]** plantilla y toque **[!UICONTROL Habilitar]** en la barra de acciones superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> Observe que los componentes utilizados para mostrar el contenido se añaden a la plantilla misma y se bloquean. Esto permite a los autores editar los componentes predefinidos, pero no añadir ni eliminar componentes de forma arbitraria, ya que cambiar la propia API podría alterar las suposiciones sobre la estructura de JSON y dejar de consumir aplicaciones. Todas las API deben ser estables.

## Pasos siguientes

De forma opcional, instale la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) paquete de contenido en AEM Author a través de [Administrador de paquetes AEM](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y los capítulos anteriores del tutorial.

* [Capítulo 5 - Creación de páginas de servicios de contenido](./chapter-5.md)
