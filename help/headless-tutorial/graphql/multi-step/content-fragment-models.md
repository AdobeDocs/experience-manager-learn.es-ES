---
title: 'Definición de modelos de fragmentos de contenido: Introducción a AEM Headless - GraphQL'
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Aprenda a modelar contenido y a crear un esquema con los modelos de fragmentos de contenido en AEM. Revise los modelos existentes y cree uno. Obtenga información sobre los distintos tipos de datos que se pueden utilizar para definir un esquema.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
duration: 228
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1110'
ht-degree: 2%

---

# Definición de los modelos de fragmento de contenido {#content-fragment-models}

En este capítulo, aprenderá a modelar contenido y a crear un esquema con **Modelos de fragmentos de contenido**. Obtenga información sobre los diferentes tipos de datos que se pueden utilizar para definir un esquema como parte del modelo.

Creamos dos modelos simples, **Equipo** y **Persona**. El modelo de datos **Equipo** tiene nombre, nombre corto y descripción y hace referencia al modelo de datos **Persona**, que tiene nombre completo, detalles biológicos, imagen de perfil y lista de ocupaciones.

También puede crear su propio modelo siguiendo los pasos básicos y modificar los pasos respectivos, como consultas de GraphQL y código de aplicación de React, o simplemente seguir los pasos descritos en estos capítulos.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se supone que hay un [entorno de creación de AEM disponible](./overview.md#prerequisites).

## Objetivos {#objectives}

* Cree un modelo de fragmento de contenido.
* Identifique los tipos de datos y las opciones de validación disponibles para crear modelos.
* Comprenda cómo el modelo de fragmento de contenido define **tanto** el esquema de datos como la plantilla de creación de un fragmento de contenido.

## Crear una configuración de proyecto

Una configuración de proyecto contiene todos los modelos de fragmento de contenido asociados a un proyecto concreto y proporciona un medio para organizar los modelos. Debe crearse al menos un proyecto **antes de que** cree el modelo de fragmento de contenido.

1. Inicie sesión en el entorno de AEM **Author** (p. ej. `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. En la pantalla de inicio de AEM, vaya a **Herramientas** > **General** > **Explorador de configuración**.

   ![Navegar al Explorador de configuración](assets/content-fragment-models/navigate-config-browser.png)
1. Haga clic en **Crear**, en la esquina superior derecha
1. En el cuadro de diálogo resultante, introduzca:

   * Título*: **Mi proyecto**
   * Nombre*: **my-project** (prefiera usar todas las minúsculas usando guiones para separar las palabras). Esta cadena influye en el extremo único de GraphQL con el que las aplicaciones cliente realizan solicitudes.)
   * Comprobar **modelos de fragmentos de contenido**
   * Comprobar **consultas persistentes de GraphQL**

   ![Configuración de mi proyecto](assets/content-fragment-models/my-project-configuration.png)

## Crear los modelos de fragmentos de contenido

A continuación, crea dos modelos para un **equipo** y una **persona**.

### Creación del modelo de persona

Cree un modelo para una **persona**, que es el modelo de datos que representa a una persona que forma parte de un equipo.

1. En la pantalla de inicio de AEM, vaya a **Herramientas** > **General** > **Modelos de fragmentos de contenido**.

   ![Navegar a los modelos de fragmentos de contenido](assets/content-fragment-models/navigate-cf-models.png)

1. Vaya a la carpeta **Mi proyecto**.
1. Pulse **Crear** en la esquina superior derecha para que aparezca el asistente **Crear modelo**.
1. En el campo **Título de modelo**, escriba **Persona** y pulse **Crear**. En el cuadro de diálogo resultante, pulse **Abrir** para generar el modelo.

1. Arrastre y suelte un elemento **Texto de una sola línea** en el panel principal. Escriba las siguientes propiedades en la ficha **Propiedades**:

   * **Etiqueta de campo**: **Nombre completo**
   * **Nombre de propiedad**: `fullName`
   * Comprobación **Requerida**

   ![Campo de propiedad Nombre completo](assets/content-fragment-models/full-name-property-field.png)

   **Nombre de propiedad** define el nombre de la propiedad que se mantiene en AEM. El **Nombre de propiedad** también define el nombre de **clave** para esta propiedad como parte del esquema de datos. Esta clave **key** se usa cuando los datos del fragmento de contenido se exponen a través de las API de GraphQL.

1. Pulse la pestaña **Tipos de datos** y arrastre y suelte un campo de **Texto de varias líneas** debajo del campo **Nombre completo**. Introduzca las siguientes propiedades:

   * **Etiqueta de campo**: **Biografía**
   * **Nombre de propiedad**: `biographyText`
   * **Tipo predeterminado**: **Texto enriquecido**

1. Haga clic en la pestaña **Tipos de datos** y arrastre y suelte un campo de **Referencia de contenido**. Introduzca las siguientes propiedades:

   * **Etiqueta de campo**: **Imagen de perfil**
   * **Nombre de propiedad**: `profilePicture`
   * **Ruta raíz**: `/content/dam`

   Al configurar la **Ruta raíz**, puede hacer clic en el icono **carpeta** para que aparezca un modal y seleccione la ruta. Esto restringe las carpetas que los autores pueden utilizar para rellenar la ruta. `/content/dam` es la raíz en la que se almacenan todos los AEM Assets (imágenes, vídeos y otros fragmentos de contenido).

1. Agregue una validación a la **Referencia de imagen** para que solo se puedan usar los tipos de contenido de **Imágenes** para rellenar el campo.

   ![Restringir a imágenes](assets/content-fragment-models/picture-reference-content-types.png)

1. Haga clic en la ficha **Tipos de datos** y arrastre y suelte un tipo de datos **Enumeración** debajo del campo **Referencia de imagen**. Introduzca las siguientes propiedades:

   * **Procesar Como**: **Casillas**
   * **Etiqueta de campo**: **Ocupación**
   * **Nombre de propiedad**: `occupation`

1. Agregar varias **Opciones** mediante el botón **Agregar una opción**. Use el mismo valor para **Etiqueta de opción** y **Valor de opción**:

   **Artista**, **Influencer**, **Fotógrafo**, **Viajero**, **Escritor**, **YouTuber**

1. El modelo final de **persona** debe tener el siguiente aspecto:

   ![Modelo de persona final](assets/content-fragment-models/final-author-model.png)

1. Haga clic en **Guardar** para guardar los cambios.

### Crear el modelo de equipo

Cree un modelo para un **equipo**, que es el modelo de datos para un equipo de personas. El modelo Equipo hace referencia al modelo Persona para representar a los miembros del equipo.

1. En la carpeta **Mi proyecto**, pulsa **Crear** en la esquina superior derecha para que aparezca el asistente **Crear modelo**.
1. En el campo **Título de modelo**, escribe **Equipo** y pulsa **Crear**.

   Pulse **Abrir** en el cuadro de diálogo resultante para abrir el modelo recién creado.

1. Arrastre y suelte un elemento **Texto de una sola línea** en el panel principal. Escriba las siguientes propiedades en la ficha **Propiedades**:

   * **Etiqueta de campo**: **Título**
   * **Nombre de propiedad**: `title`
   * Comprobación **Requerida**

1. Pulse la pestaña **Tipos de datos** y arrastre y suelte un elemento de **Texto de una sola línea** en el panel principal. Escriba las siguientes propiedades en la ficha **Propiedades**:

   * **Etiqueta de campo**: **Nombre corto**
   * **Nombre de propiedad**: `shortName`
   * Comprobación **Requerida**
   * Seleccionar **Único**
   * En, **Tipo de validación** > elija **Personalizado**
   * En, **Expresión regular de validación personalizada** > escriba `^[a-z0-9\-_]{5,40}$`: esto garantiza que solo se puedan escribir valores alfanuméricos en minúsculas y guiones de 5 a 40 caracteres.

   La propiedad `shortName` proporciona una forma de consultar a un equipo individual en función de una ruta de acceso abreviada. La configuración **Único** garantiza que el valor siempre sea único por fragmento de contenido de este modelo.

1. Pulse la pestaña **Tipos de datos** y arrastre y suelte un campo de **Texto de varias líneas** debajo del campo **Nombre corto**. Introduzca las siguientes propiedades:

   * **Etiqueta de campo**: **Descripción**
   * **Nombre de propiedad**: `description`
   * **Tipo predeterminado**: **Texto enriquecido**

1. Haga clic en la pestaña **Tipos de datos** y arrastre y suelte un campo **Referencia de fragmento**. Introduzca las siguientes propiedades:

   * **Procesar Como**: **Campo Múltiple**
   * **Etiqueta de campo**: **Miembros del equipo**
   * **Nombre de propiedad**: `teamMembers`
   * **Modelos de fragmento de contenido permitidos**: use el icono de carpeta para seleccionar el modelo **Persona**.

1. El modelo final de **equipo** debería tener el siguiente aspecto:

   ![Modelo final de equipo](assets/content-fragment-models/final-team-model.png)

1. Haga clic en **Guardar** para guardar los cambios.

1. Ahora debería tener dos modelos desde los que trabajar:

   ![Dos modelos](assets/content-fragment-models/two-new-models.png)

## Publicar configuración de proyecto y modelos de fragmentos de contenido

Tras la revisión y verificación, publique `Project Configuration` y `Content Fragment Model`

1. En la pantalla de inicio de AEM, vaya a **Herramientas** > **General** > **Explorador de configuración**.

1. Puntee en la casilla que está junto a **Mi proyecto** y luego en **Publicar**

   ![Publicar configuración de proyecto](assets/content-fragment-models/publish-project-config.png)

1. En la pantalla de inicio de AEM, vaya a **Herramientas** > **General** > **Modelos de fragmentos de contenido**.

1. Vaya a la carpeta **Mi proyecto**.

1. Pulse **Persona** y **Equipo** modelos y pulse **Publicar**

   ![Publicar modelos de fragmentos de contenido](assets/content-fragment-models/publish-content-fragment-model.png)

## Enhorabuena. {#congratulations}

¡Enhorabuena, acaba de crear sus primeros modelos de fragmentos de contenido!

## Siguientes pasos {#next-steps}

En el capítulo siguiente, [Creación de modelos de fragmentos de contenido](author-content-fragments.md), crea y edita un nuevo fragmento de contenido basado en un modelo de fragmento de contenido. También aprenderá a crear variaciones de Fragmentos de contenido.

## Documentación relacionada

* [Modelos de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

