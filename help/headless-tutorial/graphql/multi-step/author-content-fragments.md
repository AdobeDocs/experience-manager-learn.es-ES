---
title: 'AEM Creación de fragmentos de contenido: Introducción a la creación sin encabezado de la - GraphQL'
description: Introducción a Adobe Experience Manager AEM () y GraphQL. Cree y edite un nuevo fragmento de contenido basado en un modelo de fragmento de contenido. Aprenda a crear variaciones de Fragmentos de contenido.
version: Cloud Service
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 4%

---

# Creación de fragmentos de contenido {#authoring-content-fragments}

En este capítulo, creará y editará un nuevo fragmento de contenido basado en [Modelo de fragmento de contenido recién definido](./content-fragment-models.md). También aprenderá a crear variaciones de Fragmentos de contenido.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que los pasos descritos en la sección [Definición de modelos de fragmentos de contenido](./content-fragment-models.md) se han completado.

## Objetivos {#objectives}

* Crear un fragmento de contenido basado en un modelo de fragmento de contenido
* Crear una variación de fragmento de contenido

## Crear una carpeta de recursos

Los fragmentos de contenido se almacenan en carpetas en AEM Assets. Para crear fragmentos de contenido a partir de los modelos creados en el capítulo anterior, se debe crear una carpeta para almacenarlos. Se requiere una configuración en la carpeta para habilitar la creación de fragmentos de modelos específicos.

1. AEM En la pantalla Inicio de la, vaya a **Assets** > **Archivos**.

   ![Navegar a archivos de recursos](assets/author-content-fragments/navigate-assets-files.png)

1. Tocar **Crear** en la esquina superior derecha y pulse **Carpeta**. En el cuadro de diálogo resultante, introduzca:

   * Título*: **Mi proyecto**
   * Nombre: **my-project**

   ![Cuadro de diálogo Crear carpeta](assets/author-content-fragments/create-folder-dialog.png)

1. Seleccione el **Mi carpeta** y pulse **Propiedades**.

   ![Abrir propiedades de carpeta](assets/author-content-fragments/open-folder-properties.png)

1. Pulse el botón **Cloud Services** pestaña. En la pestaña Configuración de nube, utilice el buscador de rutas para seleccionar **Mi proyecto** configuración. El valor debe ser `/conf/my-project`.

   ![Establecer configuración de nube](assets/author-content-fragments/set-cloud-config-my-project.png)

   Al establecer esta propiedad, se permiten crear fragmentos de contenido con los modelos creados en el capítulo anterior.

1. Pulse el botón **Políticas** , en la pestaña **Modelos permitidos de fragmento de contenido** utilice el buscador de rutas para seleccionar el campo **Persona** y **Equipo** modelo creado anteriormente.

   ![Modelos de fragmento de contenido permitidos](assets/author-content-fragments/allowed-content-fragment-models.png)

   Estas directivas las hereda cualquier subcarpeta automáticamente y se pueden anular. También puede permitir modelos por etiquetas o habilitar modelos de otras configuraciones de proyecto. Este mecanismo proporciona una forma eficaz de administrar la jerarquía de contenido.

1. Tocar **Guardar y cerrar** para guardar los cambios realizados en las propiedades de la carpeta.

1. Navegue dentro de **Mi proyecto** carpeta.

1. Cree otra carpeta con los siguientes valores:

   * Título*: **Inglés**
   * Nombre: **en**

   Una práctica recomendada es configurar proyectos para el soporte multilingüe. Consulte [consulte la siguiente página de documentos para obtener más información](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).


## Creación de un fragmento de contenido {#create-content-fragment}

A continuación, se crean varios fragmentos de contenido basados en la variable **Equipo** y **Persona** modelos.

1. AEM En la pantalla de inicio de la, pulse **Fragmentos de contenido** para abrir la IU Fragmentos de contenido.

   ![IU de fragmento de contenido](assets/author-content-fragments/cf-fragment-ui.png)

1. En el carril izquierdo, expanda **Mi proyecto** y pulse **Inglés**.
1. Tocar **Crear** para que aparezca el **Fragmento de contenido nuevo** e introduzca los siguientes valores:

   * Ubicación: `/content/dam/my-project/en`
   * Modelo de fragmento de contenido: **Persona**
   * Título: **John Doe**
   * Nombre: `john-doe`

   ![Fragmento de contenido nuevo](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. Pulse **Crear**.
1. Repita los pasos anteriores para crear un fragmento que represente **Alison Smith**:

   * Ubicación: `/content/dam/my-project/en`
   * Modelo de fragmento de contenido: **Persona**
   * Título: **Alison Smith**
   * Nombre: `alison-smith`

   Tocar **Crear** para crear el fragmento Persona.

1. A continuación, repita los pasos para crear un **Equipo** fragmento que representa **Equipo alfa**:

   * Ubicación: `/content/dam/my-project/en`
   * Modelo de fragmento de contenido: **Equipo**
   * Título: **Equipo alfa**
   * Nombre: `team-alpha`

   Tocar **Crear** para crear el fragmento de equipo.

1. Debe haber tres fragmentos de contenido debajo **Mi proyecto** > **Inglés**:

   ![Nuevos fragmentos de contenido](assets/author-content-fragments/new-content-fragments.png)

## Editar fragmentos de contenido de persona {#edit-person-content-fragments}

A continuación, rellene los fragmentos recién creados con datos.

1. Pulse la casilla que hay junto a **John Doe** y pulse **Abrir**.

   ![Abrir fragmento de contenido](assets/author-content-fragments/open-fragment-for-editing.png)

1. El editor de fragmentos de contenido contiene un formulario basado en el modelo de fragmento de contenido. Rellene los distintos campos para añadir contenido al **John Doe** fragmento. Para la imagen de perfil, cargue su propia imagen en AEM Assets.

   ![Editor de fragmentos de contenido](assets/author-content-fragments/content-fragment-editor-jd.png)

1. Tocar **Guardar y cerrar** para guardar los cambios en el fragmento John Doe.
1. Vuelva a la IU de Fragmento de contenido y abra **Alison Smith** archivo para editar.
1. Repita los pasos anteriores para rellenar el **Alison Smith** fragmento con contenido.

## Editar fragmento de contenido de equipo {#edit-team-content-fragment}

1. Abra el **Equipo alfa** Fragmento de contenido mediante la IU de fragmento de contenido.
1. Rellene los campos de **Título**, **Nombre corto**, y **Descripción**.
1. Seleccione el **John Doe** y **Alison Smith** Fragmentos de contenido para rellenar el **Miembros del equipo** campo:

   ![Definir miembros del equipo](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >También puede crear fragmentos de contenido en línea mediante el **Fragmento de contenido nuevo** botón.

1. Tocar **Guardar y cerrar** para guardar los cambios en el fragmento Team Alpha.

## Publicar fragmentos de contenido

Tras la revisión y verificación, publique el `Content Fragments`

1. AEM En la pantalla de inicio de la, pulse **Fragmentos de contenido** para abrir la IU Fragmentos de contenido.

1. En el carril izquierdo, expanda **Mi proyecto** y pulse **Inglés**.

1. Pulse la casilla de verificación situada junto a los fragmentos de contenido y pulse **Publish**.
   ![Publicar fragmento de contenido](assets/author-content-fragments/publish-content-fragment.png)

## Enhorabuena. {#congratulations}

Felicidades, ha creado varios fragmentos de contenido y una variación.

## Pasos siguientes {#next-steps}

En el capítulo siguiente, [Explorar las API de GraphQL](explore-graphql-api.md)AEM Sin embargo, explorará las API de GraphQL mediante la herramienta integrada de GrapiQL. AEM Descubra cómo genera automáticamente un esquema de GraphQL basado en un modelo de fragmento de contenido. Experimentará construyendo consultas básicas usando la sintaxis de GraphQL.

## Documentación relacionada

* [Administrar fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [Variaciones: Crear contenido de fragmentos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=es)
