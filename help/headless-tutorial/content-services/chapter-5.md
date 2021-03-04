---
title: Capítulo 5 - Creación de páginas de servicios de contenido - Servicios de contenido
description: El capítulo 5 del tutorial AEM Headless cubre la creación de páginas a partir de las plantillas definidas en el capítulo 4. Estas páginas actuarán como puntos finales HTTP JSON.
feature: '"Fragmentos de contenido, API"'
topic: '"Sin encabezado, administración de contenido"'
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 0%

---


# Capítulo 5 - Creación de páginas de servicios de contenido

El capítulo 5 del tutorial AEM Headless cubre la creación de la página a partir de las plantillas definidas en el capítulo 4. La página creada en este capítulo actuará como punto final HTTP JSON para la aplicación móvil.

>[!NOTE]
>
> La arquitectura de contenido de página de `/content/wknd-mobile/en/api` se ha creado previamente. Las páginas de base de `en` y `api` tienen un propósito arquitectónico y organizativo, pero no son estrictamente necesarias. Si el contenido de la API puede localizarse, se recomienda seguir las prácticas recomendadas habituales de organización de páginas de copia de idioma y administrador de varios sitios, ya que la página de la API puede localizarse como cualquier página de AEM Sites.

## Creación de la página de API de evento

1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Sitios] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Pulse la etiqueta de la página** de API y, a continuación, pulse el  **** botón Crear en la barra de acciones superior y cree una nueva página de API de eventos en la página de API.
   1. Toque **Crear** en la barra de acciones superior
   1. Seleccione la plantilla **Events API**
   1. En el campo **Name** introduzca **events**
   1. En el campo **Title** introduzca **Events API**
   1. Toque **Crear** en la barra de acciones superior para crear la página
   1. Toque **Listo** para volver al administrador de AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Creación de la página de API de eventos

>[!NOTE]
>
> El proyecto proporciona CSS para proporcionar algunos estilos básicos a la experiencia de autor.

1. Para editar la página **Events API**, vaya a **AEM > Sites > WKND Mobile > English > API**, seleccione la página **Events API** y pulse **Edit** en la barra de acciones superior.
1. Agregue una **imagen de logotipo** para mostrarla en la aplicación arrastrándola y soltándola desde el Buscador de recursos en el marcador de posición del componente Imagen.
   * Utilice el logotipo proporcionado que se encuentra en `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Agregue **línea de etiqueta** para que se muestre encima de los eventos.
   1. Edite el componente **Texto**
   1. Escriba el texto:
      * `The WKND is here.`

1. Seleccione los **eventos** que desea mostrar:
   1. Establezca la siguiente configuración en la pestaña **Properties**:
      * Modelo: **Evento**
      * Ruta principal: **/content/dam/wknd-mobile/en/events**
      * Etiquetas: **&lt;Dejar en blanco>**
   1. Establezca la siguiente configuración en la pestaña **Elements**:
      * Elimine los elementos de la lista, para garantizar que TODOS los elementos de los fragmentos de contenido de evento estén expuestos.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Revise la salida JSON de la página de API

La salida JSON y su formato se pueden revisar solicitando la página con el selector `.model.json`.

Los consumidores de esta API deben comprender bien esta estructura (o esquema) de JSON. Es fundamental que los consumidores de API comprendan qué aspectos de la estructura se han corregido (es decir, el logotipo (imagen) y la etiqueta en directo (texto) de la API del evento y que son fluidos (por ejemplo, los eventos enumerados en el componente Lista de fragmentos de contenido ).

Si se rompe este contrato en una API publicada, es posible que el comportamiento de las aplicaciones consumidoras sea incorrecto.

1. En las pestañas nuevas del navegador, solicite las páginas de la API de eventos con el selector `.model.json` , que invoca el exportador JSON de los servicios de contenido de AEM, y serializa la página y los componentes en una estructura JSON normalizada y bien definida.

   La estructura JSON producida por estas páginas es la estructura a la que deben alinearse las aplicaciones que consumen contenido.

1. Solicite la página **API de eventos** como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   El resultado debe aparecer de forma similar a:

![Salida JSON de AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Este JSON se puede generar de forma **tidy** (con formato) para facilitar la lectura mediante el selector `.tidy`:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Paso siguiente

Opcionalmente, instale el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en AEM Author a través del [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y los capítulos anteriores del tutorial.

* [Capítulo 6 - Exposición del contenido en AEM Publish como JSON](./chapter-6.md)
