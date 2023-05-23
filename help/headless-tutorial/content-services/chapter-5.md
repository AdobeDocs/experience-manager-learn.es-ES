---
title: 'Capítulo 5: Creación de páginas de servicios de contenido: servicios de contenido'
description: AEM El capítulo 5 del tutorial sin encabezado de trata la creación de las páginas a partir de las plantillas definidas en el capítulo 4. Estas páginas actuarán como puntos finales HTTP JSON.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# Capítulo 5: Creación de páginas de servicios de contenido

AEM El capítulo 5 del tutorial sin encabezado de trata la creación de la página a partir de las plantillas definidas en el capítulo 4. La página creada en este capítulo actuará como punto final HTTP JSON para la aplicación móvil.

>[!NOTE]
>
> La arquitectura de contenido de página de `/content/wknd-mobile/en/api` se ha generado previamente. Las páginas base de `en` y `api` tienen un propósito arquitectónico y organizativo, pero no son estrictamente necesarios. Si el contenido de la API se puede localizar, se recomienda seguir las prácticas recomendadas habituales de organización de páginas de Language Copy y de Multi-site Manager, ya que la página de la API se puede localizar como cualquier página de AEM Sites.

## Creación de la página de API de eventos

1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Pulse la etiqueta en la página API** y, a continuación, pulse el botón **Crear** en la barra de acciones superior y cree una nueva página de API de eventos en la página de API.
   1. Tocar **Crear** en la barra de acciones superior
   1. Seleccionar **API de eventos** plantilla
   1. En el **Nombre** field enter **eventos**
   1. En el **Título** field enter **API de eventos**
   1. Tocar **Crear** en la barra de acciones superior para crear la página
   1. Tocar **Listo** para volver al administrador de AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Creación de la página de API de eventos

>[!NOTE]
>
> El proyecto proporciona CSS para proporcionar algunos estilos básicos para la experiencia del autor.

1. Edite el **API de eventos** navegando a **AEM > Sitios > WKND Mobile > Inglés > API**, seleccionando la **API de eventos** página y pulse **Editar** en la barra de acciones superior.
1. Añadir un **imagen del logotipo** para mostrarlo en la aplicación, arrástrelo desde el Buscador de recursos hasta el marcador de posición del componente de imagen.
   * Utilice el logotipo que encontrará en `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Añadir **línea de etiquetas** para mostrar encima de los eventos.
   1. Edite el **Texto** componente
   1. Introduzca el texto:
      * `The WKND is here.`

1. Seleccione el **eventos** para mostrar:
   1. Establezca la siguiente configuración en la **Propiedades** pestaña:
      * Modelo: **Evento**
      * Ruta principal: **/content/dam/wknd-mobile/en/events**
      * Etiquetas: **&lt;leave blank=&quot;&quot;>**
   1. Establezca la siguiente configuración en la **Elementos** pestaña:
      * Elimine cualquier elemento de la lista para garantizar que TODOS los elementos de los fragmentos de contenido de evento estén expuestos.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Revise la salida JSON de la página de la API

La salida JSON y su formato se pueden revisar solicitando la página con el `.model.json` selector.

Los consumidores de esta API deben entender bien esta estructura (o esquema) JSON. Es fundamental que los consumidores de API entiendan qué aspectos de la estructura son fijos (por ejemplo, el logotipo de la API del evento (imagen) y la etiqueta en directo (texto) y que son fluidos (por ejemplo, los eventos enumerados en el componente Lista de fragmentos de contenido ).

La ruptura de este contrato en una API publicada puede provocar un comportamiento incorrecto en las aplicaciones consumidoras.

1. En las nuevas pestañas del navegador, solicite las páginas de la API de eventos mediante el `.model.json` AEM , que invoca el Exportador JSON de los servicios de contenido, y serializa la página y los componentes en una estructura JSON normalizada y bien definida.

   La estructura JSON producida por estas páginas es la estructura a la que las aplicaciones consumidoras deben alinearse.

1. Solicite el **API de eventos** página como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   El resultado debe ser similar al siguiente:

![AEM Salida JSON de servicios de contenido](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Este JSON se puede generar en un **ordenado** (con formato) para mejorar la legibilidad humana mediante el uso del `.tidy` selector:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Siguiente paso

Si lo desea, instale [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) Paquete de contenido en AEM Author mediante [AEM Administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y en los capítulos anteriores del tutorial.

* [Capítulo 6: Exposición del contenido en AEM Publish como JSON](./chapter-6.md)
