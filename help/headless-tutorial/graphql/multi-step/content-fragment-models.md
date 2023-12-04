---
title: 'AEM Definición de modelos de fragmentos de contenido: introducción a la sin encabezado GraphQL'
description: Introducción a Adobe Experience Manager AEM () y GraphQL. AEM Aprenda a modelar contenido y a crear un esquema con modelos de fragmentos de contenido en la creación de modelos de fragmentos de contenido en la. Revise los modelos existentes y cree uno. Obtenga información sobre los distintos tipos de datos que se pueden utilizar para definir un esquema.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
duration: 315
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1110'
ht-degree: 1%

---

# Definición de los modelos de fragmento de contenido {#content-fragment-models}

En este capítulo, aprenderá a modelar contenido y a crear un esquema con **Modelos de fragmento de contenido**. Obtenga información sobre los diferentes tipos de datos que se pueden utilizar para definir un esquema como parte del modelo.

Creamos dos modelos simples, **Equipo** y **Persona**. El **Equipo** El modelo de datos de tiene nombre, nombre corto y descripción y hace referencia al **Persona** modelo de datos, que tiene nombre completo, detalles biológicos, imagen de perfil y lista de ocupaciones.

También puede crear su propio modelo siguiendo los pasos básicos y modificar los pasos respectivos, como consultas de GraphQL y código de aplicación de React, o simplemente seguir los pasos descritos en estos capítulos.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que una [AEM El entorno de autor de la página está disponible](./overview.md#prerequisites).

## Objetivos {#objectives}

* Cree un modelo de fragmento de contenido.
* Identifique los tipos de datos y las opciones de validación disponibles para crear modelos.
* Comprender cómo define el modelo de fragmento de contenido **ambos** el esquema de datos y la plantilla de creación de un fragmento de contenido.

## Crear una configuración de proyecto

Una configuración de proyecto contiene todos los modelos de fragmento de contenido asociados a un proyecto concreto y proporciona un medio para organizar los modelos. Debe crearse al menos un proyecto **antes** creando modelo de fragmento de contenido.

1. AEM Inicie sesión en la **Autor** entorno (p. ej., `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **General** > **Explorador de configuración**.

   ![Navegar al Explorador de configuración](assets/content-fragment-models/navigate-config-browser.png)
1. Clic **Crear**, en la esquina superior derecha
1. En el cuadro de diálogo resultante, introduzca:

   * Título*: **Mi proyecto**
   * Nombre*: **my-project** (prefiera utilizar todas las minúsculas usando guiones para separar las palabras. Esta cadena influye en el extremo único de GraphQL con el que las aplicaciones cliente realizan solicitudes.)
   * Marque **Modelos de fragmento de contenido**
   * Marque **Consultas persistentes de GraphQL**

   ![Configuración de Mi proyecto](assets/content-fragment-models/my-project-configuration.png)

## Crear los modelos de fragmentos de contenido

A continuación, cree dos modelos para una **Equipo** y una **Persona**.

### Creación del modelo de persona

Creación de un modelo para una **Persona**, que es el modelo de datos que representa a una persona que forma parte de un equipo.

1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **General** > **Modelos de fragmento de contenido**.

   ![Navegar a modelos de fragmentos de contenido](assets/content-fragment-models/navigate-cf-models.png)

1. Vaya a **Mi proyecto** carpeta.
1. Tocar **Crear** en la esquina superior derecha para que aparezca el **Crear modelo** asistente.
1. Entrada **Título de modelo** , introduzca **Persona** y pulse **Crear**. En el cuadro de diálogo resultante, pulse **Abrir**, para crear el modelo.

1. Arrastrar y soltar una **Texto de línea única** en el panel principal. Introduzca las siguientes propiedades en la **Propiedades** pestaña:

   * **Etiqueta de campo**: **Nombre completo**
   * **Nombre de propiedad**: `fullName`
   * Marque **Requerido**

   ![Campo de propiedad Nombre completo](assets/content-fragment-models/full-name-property-field.png)

   El **Nombre de propiedad** AEM define el nombre de la propiedad a la que se mantiene el acceso de forma. El **Nombre de propiedad** también define la variable **key** nombre para esta propiedad como parte del esquema de datos. Esta **key** se utiliza cuando los datos del fragmento de contenido se exponen a través de las API de GraphQL.

1. Pulse el botón **Tipos de datos** y arrastre y suelte una **Texto de varias líneas** campo debajo de **Nombre completo** field. Introduzca las siguientes propiedades:

   * **Etiqueta de campo**: **Biografía**
   * **Nombre de propiedad**: `biographyText`
   * **Tipo predeterminado**: **Texto enriquecido**

1. Haga clic en **Tipos de datos** y arrastre y suelte una **Referencia de contenido** field. Introduzca las siguientes propiedades:

   * **Etiqueta de campo**: **Imagen de perfil**
   * **Nombre de propiedad**: `profilePicture`
   * **Ruta raíz**: `/content/dam`

   Al configurar el **Ruta raíz**, puede hacer clic en **carpeta** para que aparezca un modal y seleccione la ruta. Esto restringe las carpetas que los autores pueden utilizar para rellenar la ruta. `/content/dam` es la raíz en la que se almacenan todos los AEM Assets (imágenes, vídeos y otros fragmentos de contenido).

1. Agregue una validación a la **Referencia de imagen** para que solo los tipos de contenido de **Imágenes** se puede utilizar para rellenar el campo.

   ![Restringir a imágenes](assets/content-fragment-models/picture-reference-content-types.png)

1. Haga clic en **Tipos de datos** y arrastre y suelte una **Enumeración**  Tipo de datos debajo de **Referencia de imagen** field. Introduzca las siguientes propiedades:

   * **Procesar como**: **Casillas**
   * **Etiqueta de campo**: **Ocupación**
   * **Nombre de propiedad**: `occupation`

1. Añadir varios **Opciones** uso del **Añadir una opción** botón. Utilice el mismo valor para **Etiqueta de opción** y **Valor de opción**:

   **Artista**, **Influenciador**, **Fotógrafo**, **Viajero**, **Escritor**, **YouTuber**

1. La final **Persona** El modelo debe tener el siguiente aspecto:

   ![Modelo de persona final](assets/content-fragment-models/final-author-model.png)

1. Clic **Guardar** para guardar los cambios.

### Crear el modelo de equipo

Creación de un modelo para una **Equipo**, que es el modelo de datos para un equipo de personas. El modelo Equipo hace referencia al modelo Persona para representar a los miembros del equipo.

1. En el **Mi proyecto** carpeta, pulse **Crear** en la esquina superior derecha para que aparezca el **Crear modelo** asistente.
1. Entrada **Título de modelo** , introduzca **Equipo** y pulse **Crear**.

   Tocar **Abrir** en el cuadro de diálogo resultante, para abrir el modelo recién creado.

1. Arrastrar y soltar una **Texto de línea única** en el panel principal. Introduzca las siguientes propiedades en la **Propiedades** pestaña:

   * **Etiqueta de campo**: **Título**
   * **Nombre de propiedad**: `title`
   * Marque **Requerido**

1. Pulse el botón **Tipos de datos** y arrastre y suelte una **Texto de línea única** en el panel principal. Introduzca las siguientes propiedades en la **Propiedades** pestaña:

   * **Etiqueta de campo**: **Nombre corto**
   * **Nombre de propiedad**: `shortName`
   * Marque **Requerido**
   * Marque **Único**
   * En, **Tipo de validación** > elegir **Personalizado**
   * En, **Régimen de validación personalizado** > introducir `^[a-z0-9\-_]{5,40}$` : esto garantiza que solo se puedan introducir valores alfanuméricos en minúsculas y guiones de 5 a 40 caracteres.

   El `shortName` proporciona una forma de consultar a un equipo individual en función de una ruta abreviada. El **Único** La configuración de garantiza que el valor siempre sea único para cada fragmento de contenido de este modelo.

1. Pulse el botón **Tipos de datos** y arrastre y suelte una **Texto de varias líneas** campo debajo de **Nombre corto** field. Introduzca las siguientes propiedades:

   * **Etiqueta de campo**: **Descripción**
   * **Nombre de propiedad**: `description`
   * **Tipo predeterminado**: **Texto enriquecido**

1. Haga clic en **Tipos de datos** y arrastre y suelte una **Referencia a fragmento** field. Introduzca las siguientes propiedades:

   * **Procesar como**: **Campo múltiple**
   * **Etiqueta de campo**: **Miembros del equipo**
   * **Nombre de propiedad**: `teamMembers`
   * **Modelos permitidos de fragmento de contenido**: utilice el icono de la carpeta para seleccionar el **Persona** modelo.

1. La final **Equipo** El modelo debe tener el siguiente aspecto:

   ![Modelo final de equipo](assets/content-fragment-models/final-team-model.png)

1. Clic **Guardar** para guardar los cambios.

1. Ahora debería tener dos modelos desde los que trabajar:

   ![Dos modelos](assets/content-fragment-models/two-new-models.png)

## Publicar configuración de proyecto y modelos de fragmentos de contenido

Tras la revisión y verificación, publique el `Project Configuration` &amp; `Content Fragment Model`

1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **General** > **Explorador de configuración**.

1. Pulse la casilla que hay junto a **Mi proyecto** y pulse **Publish**

   ![Publicar configuración de proyecto](assets/content-fragment-models/publish-project-config.png)

1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **General** > **Modelos de fragmento de contenido**.

1. Vaya a **Mi proyecto** carpeta.

1. Tocar **Persona** y **Equipo** modelos y pulse **Publish**

   ![Publicar modelos de fragmentos de contenido](assets/content-fragment-models/publish-content-fragment-model.png)

## Enhorabuena. {#congratulations}

¡Enhorabuena, acaba de crear sus primeros modelos de fragmentos de contenido!

## Pasos siguientes {#next-steps}

En el capítulo siguiente, [Creación de modelos de fragmentos de contenido](author-content-fragments.md), puede crear y editar un nuevo fragmento de contenido basado en un modelo de fragmento de contenido. También aprenderá a crear variaciones de Fragmentos de contenido.

## Documentación relacionada

* [Modelos de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

