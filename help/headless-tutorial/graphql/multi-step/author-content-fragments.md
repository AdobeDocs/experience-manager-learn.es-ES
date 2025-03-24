---
title: 'Creación de fragmentos de contenido: Introducción a AEM sin encabezado, GraphQL'
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Cree y edite un nuevo fragmento de contenido basado en un modelo de fragmento de contenido. Aprenda a crear variaciones de Fragmentos de contenido.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
duration: 173
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '853'
ht-degree: 2%

---

# Creación de fragmentos de contenido {#authoring-content-fragments}

En este capítulo, crea y edita un nuevo fragmento de contenido basado en el [modelo de fragmento de contenido recién definido](./content-fragment-models.md). También aprenderá a crear variaciones de Fragmentos de contenido.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se supone que se han completado los pasos descritos en [Definición de modelos de fragmentos de contenido](./content-fragment-models.md).

## Objetivos {#objectives}

* Crear un fragmento de contenido basado en un modelo de fragmento de contenido
* Crear una variación de fragmento de contenido

## Crear una carpeta de recursos

Los fragmentos de contenido se almacenan en carpetas en los AEM Assets. Para crear fragmentos de contenido a partir de los modelos creados en el capítulo anterior, se debe crear una carpeta para almacenarlos. Se requiere una configuración en la carpeta para habilitar la creación de fragmentos de modelos específicos.

1. En la pantalla de inicio de AEM, vaya a **Assets** > **Archivos**.

   ![Vaya a los archivos de recursos](assets/author-content-fragments/navigate-assets-files.png)

1. Pulse **Crear** en la esquina superior derecha y pulse **Carpeta**. En el cuadro de diálogo resultante, introduzca:

   * Título*: **Mi proyecto**
   * Nombre: **my-project**

   ![Cuadro de diálogo Crear carpeta](assets/author-content-fragments/create-folder-dialog.png)

1. Seleccione la carpeta **Mi carpeta** y pulse **Propiedades**.

   ![Abrir propiedades de carpeta](assets/author-content-fragments/open-folder-properties.png)

1. Pulse la pestaña **Cloud Services**. En la pestaña Configuración de nube, use el buscador de rutas para seleccionar la configuración de **Mi proyecto**. El valor debe ser `/conf/my-project`.

   ![Establecer configuración de nube](assets/author-content-fragments/set-cloud-config-my-project.png)

   Al establecer esta propiedad, se permiten crear fragmentos de contenido con los modelos creados en el capítulo anterior.

1. Pulse la pestaña **Políticas**, en el campo **Modelos de fragmento de contenido permitidos**, use el buscador de rutas para seleccionar el modelo **Persona** y **Equipo** creado anteriormente.

   ![Modelos de fragmento de contenido permitidos](assets/author-content-fragments/allowed-content-fragment-models.png)

   Estas directivas las hereda cualquier subcarpeta automáticamente y se pueden anular. También puede permitir modelos por etiquetas o habilitar modelos de otras configuraciones de proyecto. Este mecanismo proporciona una forma eficaz de administrar la jerarquía de contenido.

1. Pulse **Guardar y cerrar** para guardar los cambios en las propiedades de la carpeta.

1. Vaya a la carpeta **Mi proyecto**.

1. Cree otra carpeta con los siguientes valores:

   * Título*: **inglés**
   * Nombre: **en**

   Una práctica recomendada es configurar proyectos para el soporte multilingüe. Consulte [la siguiente página de documentos para obtener más información](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).


## Creación de un fragmento de contenido {#create-content-fragment}

>[!TIP]
>
>Para usuarios locales de AEM SDK: utilice la interfaz de usuario de AEM Assets para crear fragmentos de contenido, en lugar de la interfaz de usuario de fragmentos de contenido que se describe a continuación. Para obtener instrucciones detalladas, consulte la [documentación de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html).

A continuación, se crean varios fragmentos de contenido basados en los modelos **Equipo** y **Persona**.

1. En la pantalla de inicio de AEM, pulse **Fragmentos de contenido** para abrir la interfaz de usuario de los fragmentos de contenido.

   ![IU de fragmento de contenido](assets/author-content-fragments/cf-fragment-ui.png)

1. En el carril izquierdo, expanda **Mi proyecto** y pulse **Inglés**.
1. Pulse **Crear** para que aparezca el cuadro de diálogo **Nuevo fragmento de contenido** e introduzca los siguientes valores:

   * Ubicación: `/content/dam/my-project/en`
   * Modelo de fragmento de contenido: **Persona**
   * Título: **John Doe**
   * Nombre: `john-doe`

   ![Nuevo fragmento de contenido](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. Pulse **Crear**.
1. Repita los pasos anteriores para crear un fragmento que represente a **Alison Smith**:

   * Ubicación: `/content/dam/my-project/en`
   * Modelo de fragmento de contenido: **Persona**
   * Título: **Alison Smith**
   * Nombre: `alison-smith`

   Pulse **Crear** para crear el fragmento Persona.

1. A continuación, repita los pasos para crear un fragmento de **Equipo** que represente a **Equipo Alpha**:

   * Ubicación: `/content/dam/my-project/en`
   * Modelo de fragmento de contenido: **Equipo**
   * Título: **Team Alpha**
   * Nombre: `team-alpha`

   Pulse **Crear** para crear el fragmento de equipo.

1. Debe haber tres fragmentos de contenido debajo de **Mi proyecto** > **Inglés**:

   ![Nuevos fragmentos de contenido](assets/author-content-fragments/new-content-fragments.png)

## Editar fragmentos de contenido de persona {#edit-person-content-fragments}

A continuación, rellene los fragmentos recién creados con datos.

1. Puntee en la casilla que está junto a **John Doe** y luego en **Abrir**.

   ![Abrir fragmento de contenido](assets/author-content-fragments/open-fragment-for-editing.png)

1. El editor de fragmentos de contenido contiene un formulario basado en el modelo de fragmento de contenido. Rellene los distintos campos para agregar contenido al fragmento **John Doe**. Para la imagen de perfil, cargue su propia imagen a los AEM Assets.

   ![Editor de fragmentos de contenido](assets/author-content-fragments/content-fragment-editor-jd.png)

1. Pulse **Guardar y cerrar** para guardar los cambios en el fragmento John Doe.
1. Vuelva a la interfaz de usuario del fragmento de contenido y abra el archivo **Alison Smith** para editarlo.
1. Repita los pasos anteriores para rellenar el fragmento **Alison Smith** con contenido.

## Editar fragmento de contenido de equipo {#edit-team-content-fragment}

1. Abra el fragmento de contenido **Team Alpha** mediante la interfaz de usuario del fragmento de contenido.
1. Rellene los campos de **Título**, **Nombre corto** y **Descripción**.
1. Seleccione los fragmentos de contenido **John Doe** y **Alison Smith** para rellenar el campo **Miembros del equipo**:

   ![Establecer miembros del equipo](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >También puede crear fragmentos de contenido en línea con el botón **Nuevo fragmento de contenido**.

1. Pulse **Guardar y cerrar** para guardar los cambios en el fragmento de Team Alpha.

## Publicar fragmentos de contenido

>[!TIP]
>
>Para usuarios locales de AEM SDK: utilice la interfaz de usuario de AEM Assets para publicar fragmentos de contenido, en lugar de la interfaz de usuario de fragmentos de contenido que se describe a continuación. Para obtener instrucciones detalladas, consulte la [documentación de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html#publishing-and-referencing-a-fragment).

Tras la revisión y verificación, publique el `Content Fragments` creado

1. En la pantalla de inicio de AEM, pulse **Fragmentos de contenido** para abrir la interfaz de usuario de los fragmentos de contenido.

1. En el carril izquierdo, expanda **Mi proyecto** y pulse **Inglés**.

1. Pulse la casilla que está junto a los fragmentos de contenido y pulse **Publicar**.
   ![Publicar fragmento de contenido](assets/author-content-fragments/publish-content-fragment.png)

## Enhorabuena. {#congratulations}

Felicidades, ha creado varios fragmentos de contenido y una variación.

## Siguientes pasos {#next-steps}

En el capítulo siguiente, [Explorar las API de GraphQL](explore-graphql-api.md), explorará las API de GraphQL de AEM mediante la herramienta GrapiQL integrada. Descubra cómo AEM genera automáticamente un esquema de GraphQL basado en un modelo de fragmento de contenido. Experimentará construyendo consultas básicas usando la sintaxis de GraphQL.

## Documentación relacionada

* [Administrar fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [Variaciones: Crear contenido de fragmentos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=es)
