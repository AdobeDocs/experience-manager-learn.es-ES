---
title: 'Crear fragmentos de contenido: conceptos avanzados de AEM sin encabezado, GraphQL'
description: En este capítulo de Conceptos avanzados de Adobe Experience Manager (AEM) sin encabezado, aprenda a trabajar con pestañas, fecha y hora, objetos JSON y referencias de fragmentos en fragmentos de contenido. Configure directivas de carpeta para limitar qué modelos de fragmentos de contenido se pueden incluir.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 998d3678-7aef-4872-bd62-0e6ea3ff7999
duration: 609
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2931'
ht-degree: 1%

---

# Crear fragmentos de contenido

En el [capítulo anterior](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md), creó cinco modelos de fragmento de contenido: persona, equipo, ubicación, dirección e información de contacto. Este capítulo le guiará por los pasos para crear fragmentos de contenido basados en esos modelos. También explora cómo crear directivas de carpeta para limitar qué modelos de fragmentos de contenido se pueden utilizar en la carpeta.

## Requisitos previos {#prerequisites}

Este documento forma parte de un tutorial de varias partes. Asegúrese de que el [capítulo anterior](create-content-fragment-models.md) se haya completado antes de continuar con este capítulo.

## Objetivos {#objectives}

En este capítulo, aprenderá a:

* Crear carpetas y establecer límites mediante directivas de carpetas
* Cree referencias de fragmento directamente desde el editor de fragmentos de contenido
* Uso de tipos de datos Tab, Date y JSON Object
* Inserte referencias de contenido y fragmentos en el editor de texto multilínea
* Añadir varias referencias de fragmento
* Anidar fragmentos de contenido

## Instalación de contenido de muestra {#sample-content}

Instale un paquete de AEM que contenga varias carpetas e imágenes de muestra utilizadas para acelerar el tutorial.

1. Descargar [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip)
1. En AEM, vaya a **Herramientas** > **Implementación** > **Paquetes** para acceder al **Administrador de paquetes**.
1. Cargue e instale el paquete (archivo zip) descargado en el paso anterior.

   ![Paquete cargado a través del administrador de paquetes](assets/author-content-fragments/install-starter-package.png)

## Crear carpetas y establecer límites mediante directivas de carpetas

En la página de inicio de AEM, seleccione **Assets** > **Archivos** > **WKND Compartido** > **Inglés**. Aquí puede ver las distintas categorías de fragmentos de contenido, incluidas Aventuras y Colaboradores.

### Crear carpetas {#create-folders}

Vaya a la carpeta **Adventures**. Puede ver que ya se han creado carpetas para Equipos y ubicaciones para almacenar fragmentos de contenido de Equipos y ubicaciones.

Cree una carpeta para los fragmentos de contenido de los instructores basada en el modelo de fragmento de contenido de la persona.

1. En la página Aventuras, selecciona **Crear** > **Carpeta** en la esquina superior derecha.

   ![Crear carpeta](assets/author-content-fragments/create-folder.png)

1. En el modal Crear carpeta que aparece, escriba &quot;Instructores&quot; en el campo **Title**. Observe la &#39;s&#39; al final. Los títulos de las carpetas que contienen muchos fragmentos deben ser plural. Seleccione **Crear**.

   ![Crear modal de carpeta](assets/author-content-fragments/create-folder-modal.png)

   Ahora ha creado una carpeta para almacenar los instructores de aventura.

### Establecer límites mediante directivas de carpeta

AEM le permite definir permisos y directivas para las carpetas de fragmentos de contenido. Con los permisos, puede conceder acceso a determinadas carpetas solo a determinados usuarios (autores) o grupos de autores. Mediante directivas de carpeta, puede limitar lo que los autores de modelos de fragmentos de contenido pueden utilizar en esas carpetas. En este ejemplo, limitemos una carpeta a los modelos Person e Contact Info. Para configurar una directiva de carpeta:

1. Seleccione la carpeta **Instructores** que ha creado y, a continuación, seleccione **Propiedades** en la barra de navegación superior.

   ![Propiedades](assets/author-content-fragments/properties.png)

1. Seleccione la ficha **Políticas** y, a continuación, anule la selección de **Heredadas de /content/dam/wknd-shared**. En el campo **Modelos de fragmento de contenido permitidos por ruta**, seleccione el icono de carpeta.

   ![Icono de carpeta](assets/author-content-fragments/folder-icon.png)

1. En el cuadro de diálogo Seleccionar ruta que se abre, siga la ruta **conf** > **WKND compartido**. El modelo de fragmento de contenido de persona, creado en el capítulo anterior, contiene una referencia al modelo de fragmento de contenido de información de contacto. Los modelos de persona e información de contacto deben estar permitidos en la carpeta Instructores para crear un fragmento de contenido de instructor. Seleccione **Persona** y **Información de contacto**; a continuación, presione **Seleccionar** para cerrar el cuadro de diálogo.

   ![Seleccionar ruta](assets/author-content-fragments/select-path.png)

1. Seleccione **Guardar y cerrar** y seleccione **Aceptar** en el cuadro de diálogo de éxito que aparece.

1. Ahora ha configurado una directiva de carpeta para la carpeta Instructores. Vaya a la carpeta **Instructores** y seleccione **Crear** > **Fragmento de contenido**. Los únicos modelos que puedes seleccionar ahora son **Persona** y **Información de contacto**.

   ![Directivas de carpeta](assets/author-content-fragments/folder-policies.png)

## Crear fragmentos de contenido para instructores

Vaya a la carpeta **Instructores**. A partir de aquí, vamos a crear una carpeta anidada para almacenar la información de contacto de los instructores.

Siga los pasos descritos en la sección de [creación de carpetas](#create-folders) para crear una carpeta denominada &quot;Información de contacto&quot;. La carpeta anidada hereda las directivas de carpeta de la carpeta principal. No dude en configurar directivas más específicas para que la carpeta recién creada solo permita utilizar el modelo Información de contacto.

### Crear un fragmento de contenido de instructor

Vamos a crear cuatro personas que se puedan añadir a un equipo de instructores de aventura.

1. Desde la carpeta Instructores, cree un fragmento de contenido basado en el modelo de fragmento de contenido de persona y asígnele el título &quot;Jacob Wester&quot;.

   El fragmento de contenido recién creado tiene el siguiente aspecto:

   ![Fragmento de contenido de persona](assets/author-content-fragments/person-content-fragment.png)

1. Introduzca el siguiente contenido en los campos:

   * **Nombre completo**: Jacob Wester
   * **Biografía**: Jacob Wester ha sido instructor de caminatas por diez años y le ha encantado cada minuto de ella. Jacob es un buscador de aventuras con un talento para la escalada en roca y el mochilero. Jacob es el ganador de las competiciones de escalada, incluyendo la competición de escalada de la Batalla de la Bahía. Jacob vive actualmente en California.
   * **Nivel de experiencia del instructor**: experto
   * **Habilidades**: Escalada en roca, Surf, Mochilero
   * **Detalles del administrador**: Jacob Wester ha estado coordinando aventuras de mochilero durante tres años.

1. En el campo **Imagen de perfil**, agregue una referencia de contenido a una imagen. Vaya a **WKND compartido** > **Inglés** > **Colaboradores** > **jacob_wester.jpg** para crear una ruta a la imagen.

### Crear una referencia de fragmento desde el editor de fragmentos de contenido {#fragment-reference-from-editor}

AEM le permite crear una referencia de fragmento directamente desde el editor de fragmentos de contenido. Vamos a crear una referencia a la información de contacto de Jacob.

1. Seleccione **Nuevo fragmento de contenido** debajo del campo **Información de contacto**.

   ![Nuevo fragmento de contenido](assets/author-content-fragments/new-content-fragment.png)

1. Se abre el modal Nuevo fragmento de contenido. En la ficha Seleccionar destino, siga la ruta **Aventuras** > **Instructores** y marque la casilla que hay junto a la carpeta **Información de contacto**. Seleccione **Siguiente** para continuar a la ficha Propiedades.

   ![Nuevo modal de fragmento de contenido](assets/author-content-fragments/new-content-fragment-modal.png)

1. En la ficha Propiedades, escriba &quot;Jacob Wester Contact Info&quot; en el campo **Título**. Seleccione **Crear** y, a continuación, presione **Abrir** en el cuadro de diálogo de éxito que aparece.

   ![Pestaña Propiedades](assets/author-content-fragments/properties-tab.png)

   Aparecen nuevos campos que le permiten editar el fragmento de contenido de información de contacto.

   ![Fragmento de contenido de información de contacto](assets/author-content-fragments/contact-info-content-fragment.png)

1. Introduzca el siguiente contenido en los campos:

   * **Teléfono**: 209-888-0000
   * **Correo electrónico**: jwester@wknd.com

   Cuando termine, seleccione **Guardar**. Ahora ha creado un fragmento de contenido de información de contacto.

1. Para volver al fragmento de contenido del instructor, seleccione **Jacob Wester** en la esquina superior izquierda del editor.

   ![Volver al fragmento de contenido original](assets/author-content-fragments/back-to-jacob-wester.png)

   El campo **Información de contacto** ahora contiene la ruta al fragmento de información de contacto al que se hace referencia. Es una referencia de fragmento anidada. El fragmento de contenido del instructor terminado tiene este aspecto:

   ![Fragmento de contenido Jacob Wester](assets/author-content-fragments/jacob-wester-content-fragment.png)

1. Seleccione **Guardar y cerrar** para guardar el fragmento de contenido. Ahora tiene un nuevo fragmento de contenido de instructor.

### Creación de fragmentos adicionales

Siga el mismo proceso que se describe en la [sección anterior](#fragment-reference-from-editor) para crear tres fragmentos de contenido de instructores más y tres fragmentos de contenido de información de contacto para estos instructores. Añada el siguiente contenido en los fragmentos de Instructores:

**Stacey Roswells**

| Campos | Valores |
| --- | --- |
| Título del fragmento de contenido | Stacey Roswells |
| Nombre completo | Stacey Roswells |
| Información de contacto | /content/dam/wknd-shared/en/adventures/instructors/contact-info/stacey-roswells-contact-info |
| Imagen de perfil | /content/dam/wknd-shared/en/contributors/stacey-roswells.jpg |
| Biografía | Stacey Roswells es una exitosa escaladora de rocas y aventurera alpina. Nacida en Baltimore, Maryland, Stacey es la menor de seis hijos. El padre de Stacey era teniente coronel de la Armada de los Estados Unidos y su madre era instructora de danza moderna. La familia de Stacey se mudaba con frecuencia con las tareas del padre, y tomaba las primeras fotos cuando su padre estaba destinado en Tailandia. Aquí es también donde Stacey aprendió a escalar. |
| Nivel de experiencia del instructor | Avanzado  |
| Habilidades | Escalada en roca | Esquí | Mochilero |

**Kumar Selvaraj**

| Campos | Valores |
| --- | --- |
| Título del fragmento de contenido | Kumar Selvaraj |
| Nombre completo | Kumar Selvaraj |
| Información de contacto | /content/dam/wknd-shared/en/adventures/instructors/contact-info/kumar-selvaraj-contact-info |
| Imagen de perfil | /content/dam/wknd-shared/en/contributors/kumar-selvaraj.jpg |
| Biografía | Kumar Selvaraj es un experimentado instructor profesional certificado AMGA cuyo objetivo principal es ayudar a los estudiantes a mejorar sus habilidades de escalada y senderismo. |
| Nivel de experiencia del instructor | Avanzado  |
| Habilidades | Escalada en roca | Mochilero |

**Ayo Ogunseinde**

| Campos | Valores |
| --- | --- |
| Título del fragmento de contenido | Ayo Ogunseinde |
| Nombre completo | Ayo Ogunseinde |
| Información de contacto | /content/dam/wknd-shared/en/adventures/instructors/contact-info/ayo-ogunseinde-contact-info |
| Imagen de perfil | /content/dam/wknd-shared/en/contributors/ayo-ogunseinde-237739.jpg |
| Biografía | Ayo Ogunseinde es un escalador profesional e instructor de mochileros que vive en Fresno, California Central. El objetivo de Ayo es guiar a los excursionistas en sus aventuras más épicas del parque nacional. |
| Nivel de experiencia del instructor | Avanzado  |
| Habilidades | Escalada en roca | Ciclismo | Mochilero |

Deje vacío el campo **Información adicional**.

Agregue la siguiente información en los fragmentos de Información de contacto:

| Título del fragmento de contenido | Teléfono | Correo electrónico |
| ------- | -------- | -------- |
| Stacey Roswells Información de contacto | 209-888-0011 | sroswells@wknd.com |
| Kumar Selvaraj Información de contacto | 209-888-0002 | kselvaraj@wknd.com |
| Información de contacto de Ayo Ogunseinde | 209-888-0304 | aogunseinde@wknd.com |

Ya está listo para crear un equipo.

## Crear fragmentos de contenido para ubicaciones

Vaya a la carpeta **Ubicaciones**. Aquí puede ver dos carpetas anidadas que ya se han creado: Yosemite National Park y Yosemite Valley Lodge.

![Carpeta de ubicaciones](assets/author-content-fragments/locations-folder.png)

Ignora la carpeta Yosemite Valley Lodge por ahora. Volvemos más adelante en esta sección cuando creamos una ubicación que actúa como base principal para nuestro equipo de instructores.

Vaya a la carpeta **Parque Nacional Yosemite**. Actualmente, solo contiene una foto del Parque Nacional Yosemite. Vamos a crear un fragmento de contenido utilizando el Modelo de fragmento de contenido de ubicación y ponerle el título &quot;Parque nacional Yosemite&quot;.

### Marcadores de pestaña

AEM le permite utilizar marcadores de posición de pestañas para agrupar diferentes tipos de contenido y facilitar la lectura y administración de los fragmentos de contenido. En el capítulo anterior, agregó marcadores de posición de tabulación al modelo Ubicación. Como resultado, el fragmento de contenido de ubicación ahora tiene dos secciones de pestañas: **Detalles de ubicación** y **Dirección de ubicación**.

![Marcadores de posición de ficha](assets/author-content-fragments/tabs.png)

La pestaña **Detalles de ubicación** contiene los campos **Nombre**, **Descripción**, **Información de contacto**, **Imagen de ubicación** y **Tiempo por temporada**, mientras que la pestaña **Dirección de ubicación** contiene una referencia a un fragmento de contenido de dirección. Las pestañas dejan claro qué tipos de contenido se deben rellenar, por lo que la creación de contenido es más fácil de administrar.

### Tipo de datos de objeto JSON

El campo **Tiempo por temporada** es un tipo de datos de objeto JSON, lo que significa que acepta datos en formato JSON. Este tipo de datos es flexible y se puede utilizar para cualquier dato que desee incluir en el contenido.

Puede ver la descripción del campo que se creó en el capítulo anterior pasando el puntero sobre el icono de información a la derecha del campo.

![Icono de información de objeto JSON](assets/author-content-fragments/json-object-info.png)

En este caso, tenemos que proporcionar el tiempo promedio para la ubicación. Introduzca los datos siguientes:

```json
{
    "summer": "81 / 89°F",
    "fall": "56 / 83°F",
    "winter": "46 / 51°F",
    "spring": "57 / 71°F"
}
```

El campo **Tiempo por temporada** debería tener este aspecto:

![Objeto JSON](assets/author-content-fragments/json-object.png)

### Añadir contenido

Añadamos el resto del contenido al fragmento de contenido de ubicación para consultar la información con GraphQL en el capítulo siguiente.

1. En la ficha **Detalles de ubicación**, escriba la siguiente información en los campos:

   * **Nombre**: Parque Nacional Yosemite
   * **Descripción**: El Parque Nacional Yosemite está en las montañas de Sierra Nevada en California. Es famosa por sus hermosas cascadas, secuoyas gigantes y vistas icónicas de El Capitán y los acantilados de Media Cúpula. Caminar y acampar son las mejores formas de experimentar Yosemite. Numerosos senderos ofrecen infinitas oportunidades para la aventura y la exploración.

1. En el campo **Información de contacto**, cree un fragmento de contenido basado en el modelo Información de contacto y asígnele el título &quot;Información de contacto del Parque nacional Yosemite&quot;. Siga el mismo proceso como se describe en la sección anterior sobre [creación de una referencia de fragmento desde el editor](#fragment-reference-from-editor) e introduzca los siguientes datos en los campos:

   * **Teléfono**: 209-999-0000
   * **Correo electrónico**: yosemite@wknd.com

1. En el campo **Imagen de ubicación**, vaya a **Aventuras** > **Ubicaciones** > **Parque Nacional Yosemite** > **yosemite-national-park.jpeg** para crear una ruta a la imagen.

   Recuerde, en el capítulo anterior configuró la validación de imágenes, por lo que las dimensiones de la imagen de ubicación deben ser inferiores a 2560 x 1800 y su tamaño de archivo debe ser inferior a 3 MB.

1. Con toda la información agregada, la ficha **Detalles de ubicación** ahora tiene este aspecto:

   ![Ficha Detalles de ubicación completada](assets/author-content-fragments/location-details-tab-completed.png)

1. Vaya a la ficha **Dirección de ubicación**. En el campo **Dirección**, cree un fragmento de contenido titulado &quot;Dirección del parque nacional de Yosemite&quot; usando el modelo de fragmento de contenido de dirección que creó en el capítulo anterior. Siga el mismo proceso que se describe en la sección de [creación de una referencia de fragmento desde el editor](#fragment-reference-from-editor) e introduzca los siguientes datos en los campos:

   * **Dirección**: 9010 Curry Village Drive
   * **Ciudad**: Yosemite Valley
   * **Estado**: CA
   * **Código postal**: 95389
   * **País**: Estados Unidos

1. La ficha **Dirección de ubicación** del fragmento del Parque Nacional Yosemite tiene el siguiente aspecto:

   ![Ficha Dirección de ubicación completada](assets/author-content-fragments/location-address-tab-completed.png)

1. Seleccione **Guardar y cerrar**.

### Crear un fragmento más

1. Vaya a la carpeta **Yosemite Valley Lodge**. Cree un fragmento de contenido con el modelo de fragmento de contenido de ubicación y asígnele el título &quot;Yosemite Valley Lodge&quot;.

1. En la ficha **Detalles de ubicación**, escriba la siguiente información en los campos:

   * **Nombre**: Yosemite Valley Lodge
   * **Descripción**: Yosemite Valley Lodge es un centro para reuniones de grupos y todo tipo de actividades, como ir de compras, cenar, pescar, hacer caminatas y mucho más.

1. En el campo **Información de contacto**, cree un fragmento de contenido basado en el modelo Información de contacto y asígnele el título &quot;Información de contacto de Yosemite Valley Lodge&quot;. Siga el mismo proceso que se describe en la sección de [creación de una referencia de fragmento desde el editor](#fragment-reference-from-editor) e introduzca los siguientes datos en los campos del nuevo fragmento de contenido:

   * **Teléfono**: 209-992-0000
   * **Correo electrónico**: yosemitelodge@wknd.com

   Guarde el fragmento de contenido recién creado.

1. Vuelva a **Yosemite Valley Lodge** y vaya a la ficha **Dirección de ubicación**. En el campo **Dirección**, cree un fragmento de contenido titulado &quot;Dirección de alojamiento de Yosemite Valley&quot; utilizando el modelo de fragmento de contenido de dirección que creó en el capítulo anterior. Siga el mismo proceso que se describe en la sección de [creación de una referencia de fragmento desde el editor](#fragment-reference-from-editor) e introduzca los siguientes datos en los campos:

   * **Dirección**: 9006 Yosemite Lodge Drive
   * **Ciudad**: Parque Nacional Yosemite
   * **Estado**: CA
   * **Código postal**: 95389
   * **País**: Estados Unidos

   Guarde el fragmento de contenido recién creado.

1. Vaya de nuevo a **Yosemite Valley Lodge** y luego seleccione **Guardar y cerrar**. La carpeta **Yosemite Valley Lodge** ahora contiene tres fragmentos de contenido: Yosemite Valley Lodge, Información de contacto de Yosemite Valley Lodge y Dirección de Yosemite Valley Lodge.

   ![Carpeta del albergue Yosemite Valley](assets/author-content-fragments/yosemite-valley-lodge-folder.png)

## Crear un fragmento de contenido de equipo

Examine carpetas para **Equipos** > **Equipo de Yosemite**. Puede ver que la carpeta Equipo de Yosemite actualmente solo contiene el logotipo del equipo.

![Carpeta del equipo de Yosemite](assets/author-content-fragments/yosemite-team-folder.png)

Vamos a crear un fragmento de contenido utilizando el Modelo de fragmento de contenido de equipo y ponerle el título &quot;Equipo de Yosemite&quot;.

### Referencias de contenido y fragmentos en el editor de texto multilínea

AEM le permite añadir contenido y referencias de fragmento directamente en el editor de texto multilínea y recuperarlas más adelante mediante consultas de GraphQL. Vamos a agregar referencias de contenido y fragmento al campo **Descripción**.

1. Primero, agregue el siguiente texto al campo **Descripción**: &quot;El equipo de aventureros profesionales e instructores de senderismo que trabajan en el Parque Nacional Yosemite&quot;.

1. Para agregar una referencia de contenido, seleccione el icono **Insertar recurso** en la barra de herramientas del editor de texto multilínea.

   ![Insertar icono de recurso](assets/author-content-fragments/insert-asset-icon.png)

1. En el modal que aparece, selecciona **team-yosemite-logo.png** y pulsa **Seleccionar**.

   ![Seleccionar imagen](assets/author-content-fragments/select-image.png)

   La referencia de contenido ahora se agrega al campo **Descripción**.

Recuerde que en el capítulo anterior permitió que se agregaran referencias de fragmento al campo **Descripción**. Vamos a añadir uno aquí.

1. Seleccione el icono **Insertar fragmento de contenido** en la barra de herramientas del editor de texto multilínea.

   ![Icono Insertar fragmento de contenido](assets/author-content-fragments/insert-content-fragment-icon.png)

1. Vaya a **WKND Shared** > **Inglés** > **Aventuras** > **Ubicaciones** > **Yosemite Valley Lodge** > **Yosemite Valley Lodge**. Pulse **Seleccionar** para insertar el fragmento de contenido.

   ![Insertar modal de fragmento de contenido](assets/author-content-fragments/insert-content-fragment-modal.png)

   El campo **Descripción** tiene el siguiente aspecto:

   ![Campo de descripción](assets/author-content-fragments/description-field.png)

Ahora ha agregado las referencias de contenido y fragmento directamente al editor de texto multilínea.

### Tipo de datos de fecha y hora

Veamos el tipo de datos Fecha y hora. Seleccione el icono **Calendario** a la derecha del campo **Fecha de fundación del equipo** para abrir la vista de calendario.

![Vista de calendario de fecha](assets/author-content-fragments/date-calendar-view.png)

Las fechas pasadas o futuras se pueden establecer utilizando las flechas hacia delante y hacia atrás a ambos lados del mes. Digamos que el equipo de Yosemite fue fundado el 24 de mayo de 2016, así que fijaremos la fecha para entonces.

### Añadir varias referencias de fragmento

Añadamos Instructores a la referencia de fragmento de miembros del equipo.

1. Seleccione **Agregar** en el campo **Integrantes del equipo**.

   ![Botón Añadir](assets/author-content-fragments/add-button.png)

1. En el nuevo campo que aparece, seleccione el icono de carpeta para abrir el modal Select Path. Examine las carpetas para **WKND Shared** > **English** > **Adventures** > **Instructores** y, a continuación, seleccione la casilla de verificación que hay junto a **jacob-wester**. Pulse **Seleccionar** para guardar la ruta.

   ![Ruta de referencia de fragmento](assets/author-content-fragments/fragment-reference-path.png)

1. Seleccione el botón **Agregar** tres veces más. Utilice los nuevos campos para añadir los tres instructores restantes al equipo. El campo **Miembros del equipo** tiene este aspecto:

   ![Campo de miembros del equipo](assets/author-content-fragments/team-members-field.png)

1. Seleccione **Guardar y cerrar** para guardar el fragmento de contenido del equipo.

### Añadir referencias de fragmento a un fragmento de contenido de aventura

Por último, vamos a añadir los fragmentos de contenido recién creados a una aventura.

1. Vaya a **Aventuras** > **Mochilero Yosemite** y abra el fragmento de contenido de mochilero Yosemite. En la parte inferior del formulario, puede ver los tres campos que creó en el capítulo anterior: **Ubicación**, **Equipo del instructor** y **Administrador**.

1. Agregue la referencia de fragmento en el campo **Ubicación**. La ruta de ubicación debe hacer referencia al fragmento de contenido del Parque Nacional Yosemite que ha creado: `/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park`.

1. Agregue la referencia de fragmento en el campo **Equipo de instructores**. La ruta de acceso del equipo debe hacer referencia al fragmento de contenido de Yosemite Team que ha creado: `/content/dam/wknd-shared/en/adventures/teams/yosemite-team/yosemite-team`. Es una referencia de fragmento anidada. El fragmento de contenido de equipo contiene una referencia al modelo de persona que hace referencia a los modelos de dirección e información de contacto. Por lo tanto, tiene fragmentos de contenido anidados tres niveles por debajo.

1. Agregue la referencia de fragmento en el campo **Administrador**. Digamos que Jacob Wester es administrador de la aventura de mochileros de Yosemite. La ruta debe llevar al fragmento de contenido Jacob Wester y aparecer de la siguiente manera: `/content/dam/wknd-shared/en/adventures/instructors/jacob-wester`.

1. Ahora ha agregado tres referencias de fragmento a un fragmento de contenido de aventura. Los campos tienen este aspecto:

   ![Referencias de fragmento de aventura](assets/author-content-fragments/adventure-fragment-references.png)

1. Seleccione **Guardar y cerrar** para guardar el contenido.

## Enhorabuena.

¡Enhorabuena! Ahora ha creado fragmentos de contenido basados en los modelos de fragmentos de contenido avanzados creados en el capítulo anterior. También ha creado una directiva de carpeta para limitar qué modelos de fragmentos de contenido se pueden seleccionar dentro de una carpeta.

## Pasos siguientes

En el [capítulo siguiente](/help/headless-tutorial/graphql/advanced-graphql/explore-graphql-api.md), aprenderá a enviar consultas de GraphQL avanzadas mediante el entorno de desarrollo integrado (IDE) de GraphiQL. Estas consultas nos permiten ver los datos creados en este capítulo y, finalmente, añadir estas consultas a la aplicación WKND.
