---
title: Capítulo 5 - Creación de páginas de servicios de contenido - Servicios de contenido
description: El capítulo 5 del tutorial AEM sin encabezado cubre la creación de páginas a partir de las plantillas definidas en el capítulo 4. Estas páginas actuarán como puntos finales HTTP JSON.
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

# Capítulo 5 - Creación de páginas de servicios de contenido

El capítulo 5 del tutorial AEM sin encabezado cubre la creación de la página a partir de las plantillas definidas en el capítulo 4. La página creada en este capítulo actuará como punto final HTTP JSON para la aplicación móvil.

>[!NOTE]
>
> La arquitectura del contenido de la página de `/content/wknd-mobile/en/api` ha sido precompilado. Las páginas base de `en` y `api` tienen un propósito arquitectónico y organizativo, pero no son estrictamente necesarios. Si el contenido de la API puede localizarse, se recomienda seguir las prácticas recomendadas habituales de organización de páginas de copia de idioma y administrador de varios sitios, ya que la página de la API puede localizarse como cualquier otra página de AEM Sites.

## Creación de la página de API de evento

1. Vaya a **[!UICONTROL AEM] > [!UICONTROL Sitios] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Pulse la etiqueta de la página API** y, a continuación, pulse **Crear** en la barra de acciones superior y cree una nueva página de API de eventos en la página de API.
   1. Toque **Crear** en la barra de acciones superior
   1. Select **API de eventos** plantilla
   1. En el **Nombre** field enter **events**
   1. En el **Título** field enter **API de eventos**
   1. Toque **Crear** en la barra de acciones superior para crear la página
   1. Toque **Listo** para volver al administrador de AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Creación de la página de API de eventos

>[!NOTE]
>
> El proyecto proporciona CSS para proporcionar algunos estilos básicos a la experiencia de autor.

1. Edite el **API de eventos** navegando a **AEM > Sitios > WKND Mobile > Inglés > API**, seleccionando **API de eventos** y pulsando **Editar** en la barra de acciones superior.
1. Agregue un **imagen del logotipo** para mostrarlo en la aplicación arrastrándolo y soltándolo desde el Buscador de recursos al marcador de posición del componente Imagen .
   * Utilice el logotipo proporcionado que se encuentra en `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Agregar **línea de etiqueta** para mostrar encima de los eventos.
   1. Edite el **Texto** componente
   1. Introduzca el texto:
      * `The WKND is here.`

1. Seleccione el **events** para mostrar:
   1. Establezca la siguiente configuración en el **Propiedades** pestaña:
      * Modelo: **Evento**
      * Ruta principal: **/content/dam/wknd-mobile/en/events**
      * Etiquetas: **&lt;leave blank=&quot;&quot;>**
   1. Establezca la siguiente configuración en el **Elementos** pestaña:
      * Elimine los elementos de la lista, para garantizar que TODOS los elementos de los fragmentos de contenido de evento estén expuestos.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Revise la salida JSON de la página de API

La salida JSON y su formato se pueden revisar solicitando la página con la variable `.model.json` selector.

Los consumidores de esta API deben comprender bien esta estructura (o esquema) de JSON. Es fundamental que los consumidores de API comprendan qué aspectos de la estructura se han corregido (es decir, el logotipo (imagen) y la etiqueta en directo (texto) de la API del evento y que son fluidos (por ejemplo, los eventos enumerados en el componente Lista de fragmentos de contenido ).

Si se rompe este contrato en una API publicada, es posible que el comportamiento de las aplicaciones consumidoras sea incorrecto.

1. En las pestañas nuevas del navegador, solicite las páginas de la API de eventos utilizando la variable `.model.json` , que invoca AEM exportador JSON de Content Services y serializa la página y los componentes en una estructura JSON normalizada y bien definida.

   La estructura JSON producida por estas páginas es la estructura a la que deben alinearse las aplicaciones que consumen contenido.

1. Solicite el **API de eventos** page como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   El resultado debe aparecer de forma similar a:

![Salida JSON de los servicios de contenido de AEM](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Este JSON se puede generar en un **tidy** (formateado) para facilitar la lectura mediante el uso de `.tidy` selector:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Siguiente paso

De forma opcional, instale la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) paquete de contenido en AEM Author a través de [Administrador de paquetes AEM](http://localhost:4502/crx/packmgr/index.jsp). Este paquete contiene las configuraciones y el contenido descritos en este y los capítulos anteriores del tutorial.

* [Capítulo 6 - Exposición del contenido en AEM Publish como JSON](./chapter-6.md)
