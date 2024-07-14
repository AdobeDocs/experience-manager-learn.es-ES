---
title: 'Capítulo 5: Creación de páginas de servicios de contenido: servicios de contenido'
description: AEM El capítulo 5 del tutorial sin encabezado de trata la creación de las páginas a partir de las plantillas definidas en el capítulo 4. Estas páginas actuarán como puntos finales HTTP JSON.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 0%

---

# Capítulo 5: Creación de páginas de servicios de contenido

AEM El capítulo 5 del tutorial sin encabezado de trata la creación de la página a partir de las plantillas definidas en el capítulo 4. La página creada en este capítulo actuará como punto final HTTP JSON para la aplicación móvil.

>[!NOTE]
>
> La arquitectura de contenido de página de `/content/wknd-mobile/en/api` se ha generado previamente. Las páginas base de `en` y `api` tienen un propósito arquitectónico y organizativo, pero no son estrictamente necesarias. Si el contenido de la API se puede localizar, se recomienda seguir las prácticas recomendadas habituales de organización de páginas de Language Copy y de Multi-site Manager, ya que la página de la API se puede localizar como cualquier página de AEM Sites.

## Creación de la página de API de eventos

1. AEM Vaya a ** > [!UICONTROL Sitios] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Puntee en la etiqueta de la página API** y, a continuación, pulse el botón **Crear** en la barra de acciones superior y cree una nueva página de API de eventos en la página de la API.
   1. Pulse **Crear** en la barra de acciones superior
   1. Seleccione la plantilla **API de eventos**
   1. En el campo **Nombre** escriba **eventos**
   1. En el campo **Título**, introduzca **API de eventos**
   1. Pulse **Crear** en la barra de acciones superior para crear la página
   1. Pulse **Listo** para regresar al administrador de AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Creación de la página de API de eventos

>[!NOTE]
>
> El proyecto proporciona CSS para proporcionar algunos estilos básicos para la experiencia del autor.

1. AEM Edite la página **API de eventos**. Para ello, vaya a **> Sitios > WKND Mobile > Inglés > API**, seleccione la página **API de eventos** y pulse **Editar** en la barra de acciones superior.
1. Agregue una **imagen de logotipo** para mostrarla en la aplicación; para ello, arrástrela desde el Buscador de recursos al marcador de posición del componente Imagen.
   * Utilice el logotipo proporcionado que se encuentra en `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Agregue **línea de etiqueta** para mostrar encima de los eventos.
   1. Editar el componente **Texto**
   1. Introduzca el texto:
      * `The WKND is here.`

1. Seleccione los **eventos** que se mostrarán:
   1. Establezca la siguiente configuración en la ficha **Propiedades**:
      * Modelo: **Event**
      * Ruta principal: **/content/dam/wknd-mobile/en/events**
      * Etiquetas: **&lt;Dejar en blanco>**
   1. Establezca la siguiente configuración en la ficha **Elementos**:
      * Elimine cualquier elemento de la lista para garantizar que TODOS los elementos de los fragmentos de contenido de evento estén expuestos.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Revise la salida JSON de la página de la API

La salida JSON y su formato se pueden revisar solicitando la página con el selector `.model.json`.

Los consumidores de esta API deben entender bien esta estructura (o esquema) JSON. Es fundamental que los consumidores de API entiendan qué aspectos de la estructura son fijos (por ejemplo, el logotipo de la API del evento (imagen) y la etiqueta en directo (texto) y que son fluidos (por ejemplo, los eventos enumerados en el componente Lista de fragmentos de contenido ).

La ruptura de este contrato en una API publicada puede provocar un comportamiento incorrecto en las aplicaciones consumidoras.

1. AEM En las nuevas pestañas del explorador, solicite las páginas de la API de eventos mediante el selector `.model.json`, que invoca el exportador JSON de los servicios de contenido, y serializa la página y los componentes en una estructura JSON normalizada y bien definida.

   La estructura JSON producida por estas páginas es la estructura a la que las aplicaciones consumidoras deben alinearse.

1. Solicite la página **API de eventos** como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   El resultado debe ser similar al siguiente:

AEM ![Salida JSON de servicios de contenido de](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Este JSON se puede generar de manera **ordenada** (con formato) para que sea legible en lenguaje natural mediante el selector `.tidy`:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## Siguiente paso

AEM AEM De forma opcional, instale el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en Author o a través del Administrador de paquetes de [](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y en los capítulos anteriores del tutorial.

* [AEM Capítulo 6: Exposición del contenido en Publish como JSON de la interfaz de usuario de la interfaz de usuario de](./chapter-6.md)
