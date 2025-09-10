---
title: Crear fragmentos de contenido | AEM Headless OpenAPI, parte 2
description: Introducción a Adobe Experience Manager (AEM) y OpenAPI. Cree y edite un nuevo fragmento de contenido basado en un modelo de fragmento de contenido. Aprenda a crear variaciones de Fragmentos de contenido.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6713
thumbnail: 22451.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 700
source-git-commit: 01f51a3556cfbcc22bbe31c4c05c11caaab71708
workflow-type: tm+mt
source-wordcount: '1274'
ht-degree: 1%

---

# Crear fragmentos de contenido

En este capítulo, crea y edita nuevos fragmentos de contenido basados en los [modelos de fragmentos de contenido de persona y equipo](./1-content-fragment-models.md). Estos fragmentos de contenido son el contenido que consume la aplicación React mediante la Entrega de fragmentos de contenido de AEM con API de OpenAPI.

## Requisitos previos

Este es un tutorial de varias partes y se supone que se han completado los pasos descritos en [Definición de modelos de fragmentos de contenido](./1-content-fragment-models.md).

## Objetivos

* Cree un fragmento de contenido basado en un modelo de fragmento de contenido.
* Crear un fragmento de contenido.
* Publicar un fragmento de contenido.

## Creación de carpetas de recursos para fragmentos de contenido

Los fragmentos de contenido se almacenan en carpetas en los AEM Assets. Para crear fragmentos de contenido a partir de los modelos de fragmentos de contenido creados en el capítulo anterior, debe existir una carpeta para almacenarlos. Se requiere una configuración en la carpeta para habilitar la creación de fragmentos de contenido a partir de modelos de fragmentos de contenido específicos.

AEM admite la organización de carpetas &quot;plana&quot;, lo que significa que los fragmentos de contenido de diferentes modelos de fragmentos de contenido se mezclan en una sola carpeta. Sin embargo, en este tutorial, se usa, en parte, una estructura de carpetas que se alinea con los modelos de fragmentos de contenido para explorar la API **Enumerar todos los fragmentos de contenido por carpeta** en el [capítulo siguiente](./3-explore-openapis.md). Al determinar la organización de los fragmentos de contenido, tenga en cuenta cómo desea crear y administrar los fragmentos de contenido, así como cómo entregarlos y consumirlos mediante la Entrega de fragmentos de contenido de AEM con API de OpenAPI.

1. En la pantalla de inicio de AEM, vaya a **Assets** > **Archivos**.
1. Seleccione **Crear** en la esquina superior derecha y seleccione **Carpeta**. Escriba

   * Título: **Mi proyecto**
   * Nombre: **my-project**

   Seleccione **Crear** para crear la carpeta.

1. Abra la nueva carpeta **Mi proyecto** y cree una subcarpeta bajo la nueva carpeta **Mi proyecto** con los siguientes valores:

   * Título: **inglés**
   * Nombre: **en**

   Se crea una carpeta de idioma raíz para colocar el proyecto de modo que admita las capacidades de localización nativas de AEM. Una práctica recomendada es configurar proyectos para la asistencia multilingüe, incluso si no requiere localización hoy en día. Consulte [la siguiente página de documentos para obtener más información](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).

1. Cree dos subcarpetas en la nueva carpeta **Mi proyecto > Inglés** con los siguientes valores:

   Una carpeta `teams` que contiene los fragmentos de contenido **Equipo**

   * Título: **Equipos**
   * Nombre: **equipos**

   ... y una carpeta `people` que contiene los fragmentos de contenido de **Persona**.

   * Título: **Personas**
   * Nombre: **personas**

1. Vuelva a la carpeta **Mi proyecto > Inglés** y asegúrese de que se hayan creado las dos carpetas nuevas.
1. Seleccione la carpeta **Teams** y seleccione **Properties** en la barra de acciones superior.
1. Seleccione la ficha **Políticas** y desmarque **Heredadas de`/content/dam/my-project`**.
1. En la ficha **Políticas**, seleccione el modelo de fragmento de contenido **Equipo** en el campo **Modelos de fragmento de contenido permitidos por ruta**.

   ![Modelos de fragmento de contenido permitidos](assets/2/folder-policies.png)

   Las subcarpetas heredan estas directivas automáticamente, pero se pueden sobrescribir. Las etiquetas pueden permitir los modelos de fragmento de contenido o habilitar modelos de fragmento de contenido desde otras configuraciones de proyecto. Este mecanismo proporciona una forma eficaz de administrar la jerarquía de contenido.

1. Seleccione **Guardar y cerrar** para guardar los cambios en las propiedades de la carpeta.
1. Actualice las **Directivas** para la carpeta **Personas** del mismo modo, pero seleccione el modelo de fragmento de contenido **Persona** en su lugar.

## Crear una persona Fragmento de contenido

Cree fragmentos de contenido basados en el modelo de fragmento de contenido **Persona** en la carpeta **Mi proyecto > Inglés > Personas**.

1. En la pantalla de inicio de AEM, seleccione **Fragmentos de contenido** para abrir la consola Fragmentos de contenido.
1. Seleccione el botón **Mostrar carpeta** para abrir el explorador de carpetas.
1. Seleccione la carpeta **Mi proyecto > Inglés > Personas**.
1. Seleccione **Crear > Fragmento de contenido** e introduzca los siguientes valores:

   * Ubicación: `/content/dam/my-project/en/people`
   * Modelo de fragmento de contenido: **Persona**
   * Título: **John Doe**
   * Nombre: `john-doe`

   Tenga en cuenta que estos campos **Título**, **Nombre** y **Descripción** del cuadro de diálogo **Nuevo fragmento de contenido** se almacenan como metadatos sobre el fragmento de contenido y no como parte de los datos del fragmento.

   ![Nuevo fragmento de contenido](assets/2/new-content-fragment.png)

1. Seleccione **Crear y abrir**.
1. Rellene los campos del fragmento **John Doe**:

   * Nombre completo: **John Doe**
   * Biografía: **John Doe ama los medios sociales y es un entusiasta de los viajes.**
   * Imagen del perfil: seleccione una imagen de `/content/dam` o cargue una nueva.
   * Ocupación: **Influencer**, **Viajero**

   Estos campos y valores definen el contenido del fragmento de contenido que se consumirá mediante la entrega de fragmentos de contenido de AEM con API de OpenAPI.

   ![Creación de un nuevo fragmento de contenido](assets/2/john-doe-content-fragment.png)

1. Los cambios en los fragmentos de contenido se guardan automáticamente, por lo que no hay ningún botón **Guardar**.
1. Vuelva a la consola Fragmento de contenido y seleccione **Mi proyecto > Inglés > Persona** para ver el nuevo fragmento de contenido.

### Crear fragmentos de contenido de persona adicional

Repita los pasos anteriores para crear fragmentos adicionales de **Persona**.

1. Crear un fragmento de contenido de persona para **Alison Smith** con las siguientes propiedades:

   * Ubicación: `/content/dam/my-project/en/people`
   * Modelo de fragmento de contenido: **Persona**
   * Título: **Alison Smith**
   * Nombre: `alison-smith`

   Seleccione **Crear y abrir** y cree los siguientes valores:

   * Nombre completo: **Alison Smith**
   * Biografía: **Alison es fotógrafa y le encanta escribir sobre sus viajes.**
   * Imagen del perfil: seleccione una imagen de `/content/dam` o cargue una nueva.
   * Ocupación: **Fotógrafo**, **Viajero**, **Escritor**.

Ahora debería tener dos fragmentos de contenido en la carpeta **Mi proyecto > Inglés > Personas**:

![Nuevos fragmentos de contenido](assets/2/people-content-fragments.png)

Si lo desea, puede crear más fragmentos de contenido de persona para representar a más personas.

## Crear un fragmento de contenido de equipo

Con el mismo método, cree un fragmento de **Equipo** basado en el modelo de fragmento de contenido **Equipo** en la carpeta **Mi proyecto > Inglés > Equipos**.

1. Cree un fragmento **Equipo** que represente a **Equipo Alpha** con las siguientes propiedades:

   * Ubicación: `/content/dam/my-project/en`
   * Modelo de fragmento de contenido: **Equipo**
   * Título: **Team Alpha**
   * Nombre: `team-alpha`

   Seleccione **Crear y abrir** y cree los siguientes valores:

   * Título: **Team Alpha**
   * Descripción: **Team Alpha es un equipo especializado en fotografía y escritura de viajes.**
   * **Miembros del equipo**: seleccione los fragmentos de contenido de **John Doe** y **Alison Smith** para rellenar el campo **Miembros del equipo**.

   ![Fragmento de contenido de Team Alpha](assets/2/team-alpha-content-fragment.png)

1. Seleccione **Crear y abrir** para crear el fragmento de contenido del equipo
1. Debe haber un fragmento de contenido debajo de **Mi proyecto > Inglés > Equipo**:

Ahora debería tener un fragmento de contenido de **Team Alpha** en la carpeta **Mi proyecto > Inglés > Equipos**:

![Fragmentos de contenido de equipo](assets/2/team-content-fragments.png)

Opcionalmente, puede crear un **Equipo Omega** con un conjunto diferente de personas.

## Publicar fragmentos de contenido

Para que los fragmentos de contenido estén disponibles mediante las API abiertas, publíquelos. La publicación permite acceder a los fragmentos de contenido a través de:

* **Servicio de publicación**: proporciona contenido a aplicaciones de producción
* **Servicio de vista previa**: proporciona contenido para obtener una vista previa de las aplicaciones

Normalmente, el contenido se publica primero en el **servicio de vista previa** y se revisa en una aplicación de vista previa antes de publicarse en el **servicio de publicación**. La publicación en **Servicio de publicación** no se publica también en **Servicio de vista previa**. Debe publicar en el **servicio de vista previa** por separado.

En este tutorial publicaremos en el servicio AEM Publish, pero el uso del servicio AEM Preview es tan fácil como cambiar la URL del servicio [AEM en la aplicación React](./4-react-app.md)

1. En la consola Fragmento de contenido, busque la carpeta **Mi proyecto > Inglés**.
1. Seleccione todos los fragmentos de contenido de la carpeta **English** (que muestra todos los fragmentos de contenido de todas las subcarpetas) y seleccione **Publicar > Ahora** en la barra de acciones superior.

   ![Seleccionar fragmentos de contenido para publicar](assets/2/select-publish-content-fragments.png)

1. Seleccione el **servicio de publicación** en **Incluir todas las referencias**, seleccione **No publicado** y **Modificado** y seleccione **Publicar**.

   ![Publicar fragmento de contenido](assets/2/publish-content-fragments.png)

Ahora los fragmentos de contenido, todos los fragmentos de contenido de persona a los que hacen referencia los fragmentos de contenido de equipo y todos los recursos a los que se hace referencia se publican en el **servicio de publicación**.

Puede publicar en el **servicio de vista previa** del mismo modo.

## Enhorabuena.

Felicidades, ha creado correctamente fragmentos de contenido basados en modelos de fragmentos de contenido en AEM. Ha creado un modelo de fragmento de contenido **Persona**, ha creado varios fragmentos de contenido **Persona** y ha creado un fragmento de contenido **Equipo** que hace referencia a varios fragmentos de contenido de **Persona**.

Con los fragmentos de contenido publicados, ahora puede acceder a ellos a través de la Entrega de fragmentos de contenido de AEM con API de OpenAPI.

## Próximos pasos

En el capítulo siguiente, [Explorar las OpenAPI](3-explore-openapis.md), explorará la Entrega de fragmentos de contenido de AEM con API de OpenAPI mediante la funcionalidad **Probarlo** integrada en la documentación de la API.

