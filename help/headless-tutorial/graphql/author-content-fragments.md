---
title: 'Creación de fragmentos de contenido: Introducción a AEM Headless - GraphQL'
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Cree y edite un nuevo fragmento de contenido basado en un modelo de fragmento de contenido. Aprenda a crear variaciones de fragmentos de contenido.
sub-product: activos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: Fragmentos de contenido, API de GraphQL
topic: Sin objetivos, Administración de contenido
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '794'
ht-degree: 0%

---


# Creación de fragmentos de contenido {#authoring-content-fragments}

En este capítulo, creará y editará un nuevo fragmento de contenido basado en el [recién definido Modelo de fragmento de contenido del colaborador](./content-fragment-models.md). También aprenderá a crear variaciones de fragmentos de contenido.

## Requisitos previos {#prerequisites}

Este es un tutorial en varias partes y se da por hecho que se han completado los pasos descritos en [Definición de modelos de fragmento de contenido](./content-fragment-models.md).

## Objetivos {#objectives}

* Creación de un fragmento de contenido basado en un modelo de fragmento de contenido
* Creación de una variación de fragmento de contenido

## Información general sobre la creación de fragmentos de contenido {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

El vídeo anterior ofrece información general de alto nivel sobre la creación de fragmentos de contenido.

## Crear un fragmento de contenido {#create-content-fragment}

En el capítulo anterior, [Definición de modelos de fragmento de contenido](./content-fragment-models.md), se creó un modelo de **colaborador**. Cree un nuevo fragmento de contenido con este modelo.

1. En el menú **Inicio de AEM** vaya a **Assets** > **Archivos**.
1. Haga clic en las carpetas para navegar a **Sitio WKND** > **Inglés** > **Colaboradores**. Esta carpeta contiene una lista de capturas de cabeza para los colaboradores de la marca WKND.

1. Haga clic en **Create** en la esquina superior derecha y seleccione **Content Fragment**:

   ![Haga clic en Crear un nuevo fragmento](assets/author-content-fragments/create-content-fragment-menu.png)

1. Seleccione el modelo **Contributor** y haga clic en **Siguiente**.

   ![Seleccionar modelo de colaborador](assets/author-content-fragments/select-contributor-model.png)

   Este es el mismo modelo de **colaborador** que se creó en el capítulo anterior.

1. Introduzca **Stacey Roswells** para el título y haga clic en **Create**.
1. Haga clic en **Open** en el cuadro de diálogo **Success** para abrir el fragmento recién creado.

   ![Nuevo fragmento de contenido creado](assets/author-content-fragments/new-content-fragment.png)

   Observe que los campos definidos por el modelo ahora están disponibles para crear esta instancia del fragmento de contenido.

1. Para **Nombre completo**, introduzca: **Stacey Roswells**.
1. Para **Biografía** introduzca una breve biografía. ¿Necesita algo de inspiración? No dude en reutilizar este [archivo de texto](assets/author-content-fragments/stacey-roswells-bio.txt).
1. Para **Picture Reference** haga clic en el icono **folder** y vaya a **WKND Site** > **English** > **Contributors** > **stacey-roswells.jpg**. Esto se evaluará en la ruta: `/content/dam/wknd/en/contributors/stacey-roswells.jpg`.
1. Para **Ocupation**, elija **Fotógrafo**.

   ![Fragmento creado](assets/author-content-fragments/stacye-roswell-fragment-authored.png)

1. Haga clic en **Guardar** para guardar los cambios.

## Crear una variación de fragmento de contenido

Todos los fragmentos de contenido comienzan con una variación **Principal**. La variación **Master** se puede considerar el contenido *predeterminado* del fragmento y se utiliza automáticamente cuando el contenido se expone mediante las API de GraphQL. También es posible crear variaciones de un fragmento de contenido. Esta función ofrece una flexibilidad adicional para diseñar una implementación.

Se pueden usar variables para segmentar canales específicos. Por ejemplo, se puede crear una variación **mobile** que contenga una cantidad menor de texto o haga referencia a una imagen específica del canal. La forma en que se utilizan las variaciones depende en realidad de la implementación. Al igual que cualquier función, se debe realizar una planificación cuidadosa antes de utilizar.

A continuación, cree una nueva variación para hacerse una idea de las capacidades disponibles.

1. Vuelva a abrir el fragmento de contenido **Stacey Roswells**.
1. En el carril lateral izquierdo, haga clic en **Crear variación**.
1. En el modal **New Variation** introduzca un Título de **Summary**.

   ![Nueva variación: Resumen](assets/author-content-fragments/new-variation-summary.png)

1. Haga clic en el campo multilínea **Biografía** y haga clic en el botón **Expandir** para introducir la vista de pantalla completa del campo multilínea.

   ![Entrar en la vista Pantalla completa](assets/author-content-fragments/enter-full-screen-view.png)

1. Haga clic en **Resumir texto** en el menú superior derecho.

1. Introduzca un **Target** de **50** palabras y haga clic en **Inicio**.

   ![Vista previa de resumen](assets/author-content-fragments/summarize-text-preview.png)

   Se abrirá una vista previa de resumen. El procesador de idioma del equipo de AEM intentará resumir el texto basado en el recuento de palabras objetivo. También puede seleccionar distintas frases para eliminarlas.

1. Haga clic en **Resumir** cuando esté satisfecho con el resumen. Haga clic en el campo de texto multilínea y active el botón **Expandir** para volver a la vista principal.

1. Haga clic en **Guardar** para guardar los cambios.

## Crear un fragmento de contenido adicional

Repita los pasos descritos en [Crear un fragmento de contenido](#create-content-fragment) para crear un **colaborador** adicional. Esto se utilizará en el capítulo siguiente como ejemplo de cómo consultar varios fragmentos.

1. En la carpeta **Contributors**, haga clic en **Create** en la esquina superior derecha y seleccione **Content Fragment**:
1. Seleccione el modelo **Contributor** y haga clic en **Siguiente**.
1. Introduzca **Jacob Wester** para el título y haga clic en **Crear**.
1. Haga clic en **Open** en el cuadro de diálogo **Success** para abrir el fragmento recién creado.
1. Para **Nombre completo**, introduzca: **Jacob Wester**.
1. Para **Biografía** introduzca una breve biografía. ¿Necesita algo de inspiración? No dude en reutilizar este [archivo de texto](assets/author-content-fragments/jacob-wester.txt).
1. Para **Picture Reference** haga clic en el icono **folder** y vaya a **WKND Site** > **English** > **Contributors** > **jacob_wester.jpg**. Esto se evaluará en la ruta: `/content/dam/wknd/en/contributors/jacob_wester.jpg`.
1. Para **Ocupation**, elija **Writer**.
1. Haga clic en **Guardar** para guardar los cambios. No es necesario crear una variación, a menos que desee.

   ![Fragmento de contenido adicional](assets/author-content-fragments/additional-content-fragment.png)

   Ahora debe tener dos fragmentos **Contributors**.

## Felicitaciones! {#congratulations}

Felicidades, acaba de crear varios fragmentos de contenido y crear una variación.

## Pasos siguientes {#next-steps}

En el capítulo siguiente, [Explorar las API de GraphQL](explore-graphql-api.md), explorará las API de GraphQL de AEM mediante la herramienta integrada GrapiQL. Descubra cómo AEM genera automáticamente un esquema de GraphQL basado en un modelo de fragmento de contenido. Experimentará la construcción de consultas básicas mediante la sintaxis de GraphQL.