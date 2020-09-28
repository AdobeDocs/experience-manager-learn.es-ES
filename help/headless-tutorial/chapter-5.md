---
title: Capítulo 5 - Creación de páginas de servicios de contenido
description: El capítulo 5 del tutorial sin encabezado de AEM cubre la creación de páginas a partir de las plantillas definidas en el capítulo 4. Estas páginas actuarán como puntos finales HTTP JSON.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---


# Capítulo 5 - Creación de páginas de servicios de contenido

El capítulo 5 del tutorial sin encabezado de AEM cubre la creación de la página a partir de las plantillas definidas en el capítulo 4. La página creada en este capítulo actuará como punto final HTTP JSON para la aplicación móvil.

>[!NOTE]
>
> La arquitectura de contenido de página de `/content/wknd-mobile/en/api` se ha creado previamente. Las páginas de base `en` y `api` sirven a un propósito arquitectónico y organizativo, pero no son estrictamente necesarias. Si se puede localizar el contenido de la API, se recomienda seguir las prácticas recomendadas habituales de la copia de idioma y la organización de la página del administrador de varios sitios, ya que la página de la API se puede localizar como cualquier otra página de AEM Sites.

## Creación de la página de API de Evento

1. Vaya a **[!UICONTROL AEM]>[!UICONTROL Sitios]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**.
1. **Toque la etiqueta de la página** de API, luego toque el botón **Crear** en la barra de acciones superior y cree una nueva página de API de Eventos en la página de API.
   1. Toque **Crear** en la barra de acciones superior
   1. Seleccionar plantilla de API **de** Eventos
   1. En el campo **Nombre** , introduzca **eventos**
   1. En el campo **Título** , introduzca la API de **Eventos**
   1. Toque **Crear** en la barra de acciones superior para crear la página
   1. Toque **Listo** para volver al administrador de AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Creación de la página de la API de Eventos

>[!NOTE]
>
> El proyecto proporciona CSS para proporcionar algunos estilos básicos a la experiencia de creación.

1. Para editar la página de la API **de** Eventos, vaya a **AEM > Sitios > WKND Mobile > Inglés > API**, seleccione la página de la API **de** Eventos y toque **Editar** en la barra de acciones superior.
1. Añada una imagen **de** logotipo para mostrarla en la aplicación arrastrándola y soltándola desde el Buscador de recursos hasta el marcador de posición del componente Imagen.
   * Utilice el logotipo proporcionado que se encuentra en `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Añada la línea **** de etiquetas para que se muestre encima de los eventos.
   1. Edición del componente **Texto**
   1. Escriba el texto:
      1. `The WKND is here.`

1. Seleccione los **eventos** que desea mostrar:
   1. Defina la siguiente configuración en la ficha **Propiedades** :
      * Model: **Event**
      * Ruta principal: **/content/dam/wknd-mobile/es/eventos**
      * Etiquetas: **&lt;Dejar en blanco>**
   1. Defina la siguiente configuración en la ficha **Elementos** :
      * Elimine los elementos de la lista para asegurarse de que TODOS los elementos de los fragmentos de contenido de Evento están expuestos.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Revise la salida JSON de la página API

La salida JSON y su formato se pueden revisar solicitando la página con el `.model.json` selector.

Los consumidores de esta API deben comprender bien esta estructura (o esquema) JSON. Es fundamental que los consumidores de API entiendan qué aspectos de la estructura son fijos (por ejemplo: el logotipo (imagen) de la API de Evento y la etiqueta en directo (texto) y que son fluidos (por ejemplo, los eventos enumerados en el componente Lista de fragmentos de contenido).

Si se infringe este contrato en una API publicada, puede producirse un comportamiento incorrecto en las aplicaciones que lo consumen.

1. En las fichas nuevas del navegador, solicite las páginas de la API de Eventos mediante el `.model.json` selector, que invoca el exportador JSON de AEM Content Services y serializa la página y los componentes en una estructura JSON normalizada y bien definida.

   La estructura JSON que generan estas páginas es la estructura a la que deben alinearse las aplicaciones que consumen contenido.

1. Solicite la página de la API **de** Eventos como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   El resultado debería ser similar a:

![Salida JSON de AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Este JSON se puede obtener de forma **ordenada** (con formato) para que sea legible por el usuario mediante el `.tidy` selector:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Paso siguiente

Opcionalmente, instale el paquete de contenido [com.adobe.aem.guide.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) en AEM Author mediante [AEM administrador](http://localhost:4502/crx/packmgr/index.jsp)de paquetes. Este paquete contiene las configuraciones y el contenido que se describen en este y en los capítulos anteriores del tutorial.

* [Capítulo 6 - Exposición del contenido en AEM Publish como JSON](./chapter-6.md)
