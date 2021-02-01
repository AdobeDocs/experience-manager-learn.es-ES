---
title: Definición de modelos de fragmentos de contenido - Introducción a AEM sin encabezado - GraphQL
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Aprenda a modelar contenido y a crear un esquema con los modelos de fragmentos de contenido en AEM. Revise los modelos existentes y cree un nuevo modelo. Obtenga información sobre los distintos tipos de datos que se pueden utilizar para definir un esquema.
sub-product: activos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
translation-type: tm+mt
source-git-commit: 8c5b425e6dcf23cbef042097f17db9e51bdf63c9
workflow-type: tm+mt
source-wordcount: '950'
ht-degree: 1%

---


# Definición de modelos de fragmento de contenido {#content-fragment-models}

En este capítulo aprenderá a modelar contenido y a crear un esquema con **Modelos de fragmento de contenido**. Revisará los modelos existentes y creará un nuevo modelo. También aprenderá los diferentes tipos de datos que se pueden utilizar para definir un esquema como parte del modelo.

En este capítulo, creará un nuevo modelo para un **colaborador**, que es el modelo de datos para los usuarios que crean contenido de revista y aventura como parte de la marca WKND.

## Requisitos previos {#prerequisites}

Se trata de un tutorial en varias partes y se da por hecho que se han completado los pasos descritos en [Configuración rápida](./setup.md).

## Objetivos {#objectives}

* Crear un nuevo modelo de fragmento de contenido.
* Identifique los tipos de datos disponibles y las opciones de validación para crear modelos.
* Comprenda cómo el modelo de fragmento de contenido define **tanto** el esquema de datos como la plantilla de creación para un fragmento de contenido.

## Información general del modelo de fragmento de contenido {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

El vídeo anterior proporciona información general de alto nivel sobre el trabajo con modelos de fragmentos de contenido.

## Inspect: modelo de fragmento de contenido de aventura

En el capítulo anterior, se editaron varios fragmentos de contenido de aventuras y se mostraban en una aplicación externa. Inspeccionemos el modelo de fragmento de contenido de aventura para comprender el esquema de datos subyacente de estos fragmentos.

1. En el menú **AEM Inicio** vaya a **Herramientas** > **Recursos** > **Modelos de fragmento de contenido**.

   ![Navegar a los modelos de fragmento de contenido](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Vaya a la carpeta **WKND Site** y pase el ratón por encima del **Modelo de fragmento de contenido de aventura** y haga clic en el icono **Editar** (lápiz) para abrir el modelo.

   ![Abrir el modelo de fragmento de contenido de aventura](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Se abre el **Editor del modelo de fragmento de contenido**. Observe que los campos definen el modelo de Aventura e incluyen distintos **tipos de datos** como **texto de una sola línea**, **texto de varias líneas**, **Lista desglosada** y **Referencia de contenido**.

1. La columna derecha del editor lista los **tipos de datos** disponibles, que definen los campos de formulario utilizados para crear fragmentos de contenido.

1. Seleccione el campo **Título** en el panel principal. En la columna de la derecha, haga clic en la ficha **Propiedades**:

   ![Propiedades del título de aventura](assets/content-fragment-models/adventure-title-properties-tab.png)

   Observe que el campo **Nombre de propiedad** está establecido en `adventureTitle`. Define el nombre de la propiedad que se mantiene en AEM. El **Nombre de propiedad** también define el nombre **key** de esta propiedad como parte del esquema de datos. Esta **clave** se utilizará cuando los datos del fragmento de contenido se expongan mediante las API de GraphQL.

   >[!CAUTION]
   >
   > La modificación del **Nombre de propiedad** de un campo **después de que** fragmentos de contenido se deriven del Modelo, tiene efectos descendentes. Ya no se hará referencia a los valores de campo de los fragmentos existentes y el esquema de datos expuesto por GraphQL cambiará, lo que afectará a las aplicaciones existentes.

1. Desplácese hacia abajo en la ficha **Propiedades** y vista la lista desplegable **Tipo de validación**.

   ![Opciones de validación disponibles](assets/content-fragment-models/validation-options-available.png)

   Las validaciones de formulario predeterminadas están disponibles para **Correo electrónico** y **URL**. También es posible definir una validación **personalizada** mediante una expresión regular.

1. Haga clic en **Cancelar** para cerrar el Editor del modelo de fragmento de contenido.

## Creación de un modelo de colaborador

A continuación, cree un nuevo modelo para un **colaborador**, que es el modelo de datos para los usuarios que crean contenido de revista y aventura como parte de la marca WKND.

1. Haga clic en **Crear** en la esquina superior derecha para que aparezca el asistente para **Crear modelo**.
1. Para **Título del modelo** introduzca: **Colaborador** y haga clic en **Crear**

   ![Asistente para el modelo de fragmento de contenido](assets/content-fragment-models/content-fragment-model-wizard.png)

   Haga clic en **Abrir** para abrir el modelo recién creado.

1. Arrastre y suelte un elemento **Texto de una sola línea** en el panel principal. Introduzca las siguientes propiedades en la ficha **Propiedades**:

   * **Etiqueta** de campo:  **Nombre completo**
   * **Nombre de propiedad**: `fullName`
   * Marque **Requerido**

   ![Campo de propiedad Nombre completo](assets/content-fragment-models/full-name-property-field.png)

1. Haga clic en la ficha **Tipos de datos** y arrastre y suelte un campo **Texto de varias líneas** debajo del campo **Nombre completo**. Introduzca las siguientes propiedades:

   * **Etiqueta** de campo:  **Biografía**
   * **Nombre de propiedad**: `biographyText`
   * **Tipo** predeterminado:  **Texto enriquecido**

1. Haga clic en la ficha **Tipos de datos** y arrastre y suelte un campo **Referencia de contenido**. Introduzca las siguientes propiedades:

   * **Etiqueta** de campo:  **Referencia de imagen**
   * **Nombre de propiedad**: `pictureReference`
   * **Ruta de acceso raíz**: `/content/dam/wknd`

   Al configurar la **Ruta de raíz** puede hacer clic en el icono **carpeta** para que se muestre un modal y se seleccione la ruta. Esto restringirá las carpetas que los autores pueden utilizar para rellenar la ruta.

   ![Ruta raíz configurada](assets/content-fragment-models/root-path-configure.png)

1. Añada una validación en la **Referencia de imagen** para que solo se puedan usar tipos de contenido de **Imágenes** para rellenar el campo.

   ![Restringir a imágenes](assets/content-fragment-models/picture-reference-content-types.png)

1. Haga clic en la ficha **Tipos de datos** y arrastre y suelte un tipo de datos **Lista desglosada** debajo del campo **Referencia de imagen**. Introduzca las siguientes propiedades:

   * **Etiqueta** de campo:  **Ocupación**
   * **Nombre de propiedad**: `occupation`

1. Añada varias **Opciones** con el botón **Añadir una opción**. Utilice el mismo valor para **Etiqueta de opción** y **Valor de opción**:

   **Artista**,  **Influenciador**,  **fotógrafo**,  **viajero**,  **escritor**,  **YouTuber**

   ![Valores de opciones de ocupación](assets/content-fragment-models/occupation-options-values.png)

1. El modelo final **Colaborador** debe tener el siguiente aspecto:

   ![Modelo de colaborador final](assets/content-fragment-models/final-contributor-model.png)

1. Haga clic en **Guardar** para guardar los cambios.

## Habilitar el modelo de colaborador

Los modelos de fragmento de contenido tienen el estado predeterminado **Borrador** cuando se crean por primera vez. Esto permite a los usuarios refinar el modelo de fragmento de contenido **antes de**, permitiendo que los autores lo utilicen. Recuerde que la modificación del **Nombre de propiedad** de un campo del modelo cambia el esquema de datos subyacente y puede tener efectos descendentes significativos en los fragmentos existentes y las aplicaciones externas. Se recomienda planificar cuidadosamente la convención de nombres utilizada para los campos **Nombre de propiedad**.

1. Observe que el modelo **Colaborador** está actualmente en estado **Borrador**.

1. Habilite el **Modelo de colaborador** pasando el ratón por encima de la tarjeta y haciendo clic en el icono **Habilitar**:

   ![Habilitar el modelo de colaborador](assets/content-fragment-models/enable-contributor-model.png)

## Felicitaciones! {#congratulations}

Enhorabuena, acaba de crear su primer modelo de fragmento de contenido.

## Próximos pasos {#next-steps}

En el siguiente capítulo, [Creación de modelos de fragmento de contenido](author-content-fragments.md), creará y editará un nuevo fragmento de contenido basado en un modelo de fragmento de contenido. También aprenderá a crear variaciones de fragmentos de contenido.