---
title: Creación de fragmentos de contenido - Introducción a AEM sin encabezado - GraphQL
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
translation-type: tm+mt
source-git-commit: 8c5b425e6dcf23cbef042097f17db9e51bdf63c9
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---


# Creación de fragmentos de contenido {#authoring-content-fragments}

En este capítulo, creará y editará un nuevo fragmento de contenido basado en el [modelo de fragmento de contenido colaborador recién definido](./content-fragment-models.md). También aprenderá a crear variaciones de fragmentos de contenido.

## Requisitos previos {#prerequisites}

Se trata de un tutorial en varias partes y se da por hecho que se han completado los pasos descritos en [Definición de modelos de fragmento de contenido](./content-fragment-models.md).

## Objetivos {#objectives}

* Creación de un fragmento de contenido basado en un modelo de fragmento de contenido
* Creación de una variación de fragmento de contenido

## Información general sobre la creación de fragmentos de contenido {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

El vídeo anterior proporciona información general de alto nivel sobre la creación de fragmentos de contenido.

## Crear un fragmento de contenido {#create-content-fragment}

En el capítulo anterior, [Definición de modelos de fragmento de contenido](./content-fragment-models.md), se creó un modelo **Colaborador**. Cree un nuevo fragmento de contenido con este modelo.

1. En el menú **AEM Inicio** vaya a **Recursos** > **Archivos**.
1. Haga clic en las carpetas para navegar a **Sitio WKND** > **Inglés** > **Colaboradores**. Esta carpeta contiene una lista de capturas de pantalla para colaboradores de la marca WKND.

1. Haga clic en **Crear** en la esquina superior derecha y seleccione **Fragmento de contenido**:

   ![Haga clic en Crear un nuevo fragmento](assets/author-content-fragments/create-content-fragment-menu.png)

1. Seleccione el modelo **Colaborador** y haga clic en **Siguiente**.

   ![Seleccionar modelo de colaborador](assets/author-content-fragments/select-contributor-model.png)

   Este es el mismo modelo **Colaborador** que se creó en el capítulo anterior.

1. Escriba **Stacey Roswells** para el título y haga clic en **Crear**.
1. Haga clic en **Abrir** en el cuadro de diálogo **Éxito** para abrir el fragmento recién creado.

   ![Nuevo fragmento de contenido creado](assets/author-content-fragments/new-content-fragment.png)

   Observe que los campos definidos por el modelo ahora están disponibles para crear esta instancia del fragmento de contenido.

1. Para **Nombre completo** escriba: **Stacey Roswells**.
1. Para **Biografía** ingrese una breve biografía. ¿Necesita algo de inspiración? No dude en reutilizar este [archivo de texto](assets/author-content-fragments/stacey-roswells-bio.txt).
1. Para **Referencia de imagen** haga clic en el icono **carpeta** y busque **Sitio WKND** > **Inglés** > **Colaboradores** > **stacey-roswells.jpg**. Esto evaluará la ruta: `/content/dam/wknd/en/contributors/stacey-roswells.jpg`.
1. Para **Ocupación** elija **Fotógrafo**.

   ![Fragmento de creación](assets/author-content-fragments/stacye-roswell-fragment-authored.png)

1. Haga clic en **Guardar** para guardar los cambios.

## Crear una variación de fragmento de contenido

Todos los fragmentos de contenido inicio con una variación **principal**. La variación **Master** puede considerarse el contenido *predeterminado* del fragmento y se utiliza automáticamente cuando el contenido se expone mediante las API de GraphQL. También es posible crear variaciones de un fragmento de contenido. Esta función oferta flexibilidad adicional para diseñar una implementación.

Las variaciones pueden utilizarse para destinatario de canales específicos. Por ejemplo, se puede crear una variación **mobile** que contenga una cantidad menor de texto o haga referencia a una imagen específica de un canal. El modo en que se utilizan las variaciones depende en realidad de la implementación. Al igual que cualquier función, se debe realizar una planificación cuidadosa antes de utilizar.

A continuación, cree una nueva variación para hacerse una idea de las capacidades disponibles.

1. Vuelva a abrir el fragmento de contenido **Stacey Roswells**.
1. En el carril lateral izquierdo, haga clic en **Crear variación**.
1. En el modal **Nueva variación** introduzca un Título de **Resumen**.

   ![Nueva variación - Resumen](assets/author-content-fragments/new-variation-summary.png)

1. Haga clic en el campo multilínea **Biografía** y haga clic en el botón **Expandir** para introducir la vista de pantalla completa para el campo multilínea.

   ![Introducir vista de pantalla completa](assets/author-content-fragments/enter-full-screen-view.png)

1. Haga clic en **Resumir texto** en el menú superior derecho.

1. Introduzca un **Destinatario** de **50** palabras y haga clic en **Inicio**.

   ![Previsualización de resumen](assets/author-content-fragments/summarize-text-preview.png)

   Esto abrirá una previsualización de resumen. AEM procesador de idioma del equipo intentará resumir el texto basado en el recuento de palabras del destinatario. También puede seleccionar distintas frases para eliminarlas.

1. Haga clic en **Resumir** cuando esté conforme con el resumen. Haga clic en el campo de texto multilínea y active el botón **Expandir** para volver a la vista principal.

1. Haga clic en **Guardar** para guardar los cambios.

## Crear un fragmento de contenido adicional

Repita los pasos descritos en [Crear un fragmento de contenido](#create-content-fragment) para crear un **colaborador** adicional. Esto se utilizará en el capítulo siguiente como ejemplo de cómo realizar consultas de varios fragmentos.

1. En la carpeta **Colaboradores** haga clic en **Crear** en la esquina superior derecha y seleccione **Fragmento de contenido**:
1. Seleccione el modelo **Colaborador** y haga clic en **Siguiente**.
1. Escriba **Jacob Wester** para el título y haga clic en **Crear**.
1. Haga clic en **Abrir** en el cuadro de diálogo **Éxito** para abrir el fragmento recién creado.
1. Para **Nombre completo** escriba: **Jacob Wester**.
1. Para **Biografía** ingrese una breve biografía. ¿Necesita algo de inspiración? No dude en reutilizar este [archivo de texto](assets/author-content-fragments/jacob-wester.txt).
1. Para **Referencia de imagen** haga clic en el icono **carpeta** y busque **Sitio WKND** > **Inglés** > **Colaboradores** > **jacob_wester.jpg**. Esto evaluará la ruta: `/content/dam/wknd/en/contributors/jacob_wester.jpg`.
1. Para **Ocupación** elija **Escritor**.
1. Haga clic en **Guardar** para guardar los cambios. No es necesario crear una variación, a menos que desee.

   ![Fragmento de contenido adicional](assets/author-content-fragments/additional-content-fragment.png)

   Ahora debe tener dos fragmentos **Colaboradores**.

## Felicitaciones! {#congratulations}

Enhorabuena, acaba de crear varios fragmentos de contenido y una variación.

## Próximos pasos {#next-steps}

En el siguiente capítulo, [Explorar las API de GraphQL](explore-graphql-api.md), explorará las API de GraphQL AEM mediante la herramienta integrada GrapiQL. Descubra cómo AEM genera automáticamente un esquema GraphQL basado en un modelo de fragmento de contenido. Experimentará la construcción de consultas básicas utilizando la sintaxis GraphQL.