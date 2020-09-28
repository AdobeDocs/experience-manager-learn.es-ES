---
title: Desarrollar proyectos en AEM
description: Un tutorial de desarrollo que ilustra cómo desarrollar proyectos AEM.  En este tutorial, crearemos una plantilla de proyecto personalizada que se puede utilizar para crear nuevos proyectos en AEM para administrar flujos de trabajo y tareas de creación de contenido.
version: 6.3, 6.4, 6.5
feature: projects, workflow
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '4652'
ht-degree: 0%

---


# Desarrollar proyectos en AEM

Este es un tutorial de desarrollo que ilustra cómo desarrollar para [!DNL AEM Projects].  En este tutorial, crearemos una plantilla de proyecto personalizada que se puede utilizar para crear nuevos proyectos en AEM para administrar flujos de trabajo y tareas de creación de contenido.

>[!VIDEO](https://video.tv.adobe.com/v/16904/?quality=12&learn=on)

*Este vídeo ofrece una breve demostración del flujo de trabajo terminado que se crea en el siguiente tutorial.*

## Introducción {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) es una función de AEM diseñada para facilitar la gestión y agrupación de todos los flujos de trabajo y tareas asociados con la creación de contenido como parte de una implementación de AEM Sites o Assets.

AEM Projects incluye varias plantillas [de proyecto](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates)OOTB. Al crear un nuevo proyecto, los autores pueden elegir entre estas plantillas disponibles. Las implementaciones AEM grandes con requisitos comerciales únicos desearán crear plantillas de proyecto personalizadas, adaptadas a sus necesidades. Al crear una plantilla de proyecto personalizada, los desarrolladores pueden configurar el panel del proyecto, vincularlo a flujos de trabajo personalizados y crear funciones comerciales adicionales para un proyecto. Echaremos un vistazo a la estructura de una plantilla de proyecto y crearemos una de muestra.

![Tarjeta de proyecto personalizada](./assets/develop-aem-projects/custom-project-card.png)

## Configuración

Este tutorial explicará el código necesario para crear una plantilla de proyecto personalizada. Puede descargar e instalar el paquete [](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) adjunto en un entorno local para seguir el tutorial. También puede acceder al proyecto completo de Maven alojado en [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [Paquete de tutoriales finalizado](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repositorio de código completo en GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

Este tutorial asume algunos conocimientos básicos de [AEM prácticas](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/the-basics.html) de desarrollo y cierta familiaridad con [AEM configuración](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ht-projects-maven.html)del proyecto Maven. Todo el código mencionado está destinado a utilizarse como referencia y solo debe implementarse en una instancia [de desarrollo](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted)local AEM.

## Estructura de una plantilla de proyecto

Las plantillas de proyecto deben estar bajo control de código fuente y deben estar debajo de la carpeta de la aplicación en /apps. Lo ideal es que se coloquen en una subcarpeta con la convención de nombres de ***/projects/templates/**&lt;my-template>. Al colocar después de esta convención de nombres, las nuevas plantillas personalizadas estarán disponibles automáticamente para los autores al crear un proyecto. La configuración de las plantillas de proyecto disponibles se establece en: **/content/projects/jcr:nodo content** por la propiedad **cq:permissionTemplates** . De forma predeterminada, se trata de una expresión normal: **/(apps|libs)/.*/projects/templates/.***

El nodo raíz de una plantilla de proyecto tendrá un **jcr:PrimaryType** de **cq:Template**. Debajo del nodo raíz hay 3 nodos: **gadgets**, **funciones** y **flujos de trabajo**. Todos estos nodos son **nt:unestructure**. Debajo del nodo raíz también puede haber un archivo thumbnail.png que se muestra al seleccionar la plantilla en el asistente Crear proyecto.

Estructura de nodos completa:

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### Raíz de plantilla de proyecto

El nodo raíz de la plantilla de proyecto será del tipo **cq:Template**. En este nodo puede configurar las propiedades **jcr:title** y **jcr:description** que se mostrarán en el Asistente para crear proyectos. También hay una propiedad denominada **asistente** que apunta a un formulario que rellenará las propiedades del proyecto. El valor predeterminado de: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** debería funcionar bien en la mayoría de los casos, ya que permite al usuario rellenar las propiedades básicas del proyecto y agregar miembros del grupo.

**Tenga en cuenta que el Asistente para crear proyectos no utiliza el servlet POST Sling. En su lugar, los valores se anuncian en un servlet personalizado:**com.adobe.cq.project.impl.servlet.ProjectServlet**. Esto debe tenerse en cuenta al agregar campos personalizados.*

Encontrará un ejemplo de asistente personalizado para la plantilla de proyecto de traducción: **/libs/cq/core/content/projects/Wizard/traduationproject/defaultproject**.

### Gadgets {#gadgets}

No hay propiedades adicionales en este nodo, pero los elementos secundarios del nodo gadgets controlan qué mosaicos de proyecto rellenan el panel del proyecto cuando se crea un nuevo proyecto. [Los mosaicos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) del proyecto (también conocidos como gadgets o pods) son tarjetas simples que rellenan el lugar de trabajo de un proyecto. En: **/libs/cq/gui/components/projects/admin/pod. **Los propietarios de proyectos siempre pueden agregar o quitar mosaicos después de crear un proyecto.

### Funciones {#roles}

Hay 3 funciones [](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject) predeterminadas para cada proyecto: **Observadores**, **editores** y **propietarios**. Al agregar nodos secundarios debajo del nodo de roles, puede agregar funciones de proyecto específicas del negocio adicionales para la plantilla. A continuación, puede vincular estas funciones a flujos de trabajo específicos asociados con el proyecto.

### Flujos de trabajo {#workflows}

Una de las razones más atractivas para crear una plantilla de proyecto personalizada es que le permite configurar los flujos de trabajo disponibles para utilizarlos con el proyecto. Pueden usar flujos de trabajo de OOTB o flujos de trabajo personalizados. Debajo del nodo de **flujos de trabajo** debe haber un nodo de **modelos** (también `nt:unstructured`) y nodos secundarios debajo de especificar los modelos de flujo de trabajo disponibles. La propiedad **modelId **apunta al modelo de flujo de trabajo en /etc/workflow y el **asistente** de propiedades señala al cuadro de diálogo utilizado al iniciar el flujo de trabajo. Una gran ventaja de Projects es la capacidad de agregar un cuadro de diálogo personalizado (asistente) para capturar metadatos específicos de la empresa en el inicio del flujo de trabajo que puede impulsar acciones adicionales dentro del flujo de trabajo.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Creating a project template {#creating-project-template}

Como principalmente copiaremos/configuraremos nodos, usaremos CRXDE Lite. En la instancia de AEM local, abra el [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. Inicio creando una nueva carpeta debajo de `/apps/&lt;your-app-folder&gt;` denominada `projects`. Cree otra carpeta debajo de ese nombre `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Para facilitar las cosas, vamos a inicio nuestra plantilla personalizada a partir de la plantilla de proyecto simple existente.

   1. Copie y pegue el nodo **/libs/cq/core/content/projects/templates/default** debajo de la carpeta *templates* creada en el Paso 1.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. Ahora debería tener una ruta como **/apps/aem-guide/projects-tareas/projects/templates/authoring-project**.

   1. Edite las propiedades **jcr:title** y **jcr:description** del nodo author-project en valores personalizados de título y descripción.

      1. Deje la propiedad **del asistente** que apunta a las propiedades predeterminadas de Project.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Para esta plantilla de proyecto queremos utilizar Tareas.
   1. Añada un nuevo nodo **nt:no estructurado** debajo de authoring-project/gadgets denominado **tareas**.
   1. Añada las propiedades String en el nodo tareas para **cardWeight** = &quot;100&quot;, **jcr:title**=&quot;Tareas&quot; y **sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   Ahora, el mosaico [de](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#Tasks) Tareas se mostrará de forma predeterminada cuando se cree un nuevo proyecto.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. Agregaremos una función de aprobador personalizada a nuestra plantilla de proyecto.

   1. Debajo del nodo de plantilla de proyecto (authoring-project), agregue un nuevo nodo **nt:unestructure** etiquetado con **funciones**.
   1. Añada otro nodo **nt:no estructurado** etiquetado aprobadores como un elemento secundario del nodo de roles.
   1. Añadir propiedades de cadena **jcr:title** = &quot;**Aprobadores**&quot;, **roleclass** =&quot;**propietario**&quot;, **roleid******=&quot;aprobadores&quot;.
      1. El nombre del nodo aprobador, así como jcr:title y roleid pueden ser cualquier valor de cadena (siempre que roleid sea único).
      1. **roleclass** rige los permisos aplicados a esa función en función de los [3 roles]OOTB (https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User de funciones en un proyecto): **propietario**, **editor** y **observador**.
      1. En general, si la función personalizada es más una función de gestión, entonces la clase de clase puede ser **propietaria;** si se trata de una función de creación más específica, como fotógrafo o diseñador, entonces bastará con la función de **editor** . La gran diferencia entre **propietario** y **editor** es que los propietarios del proyecto pueden actualizar las propiedades del proyecto y agregar nuevos usuarios al proyecto.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. Al copiar la plantilla de proyecto simple, se configurarán 4 flujos de trabajo OOTB. Cada nodo que se encuentra debajo de flujos de trabajo/modelos apunta a un flujo de trabajo específico y a un asistente de diálogo de inicio para ese flujo de trabajo. Más adelante en este tutorial crearemos un flujo de trabajo personalizado para este proyecto. Por ahora, elimine los nodos debajo del flujo de trabajo o los modelos:

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Para facilitar a los autores de contenido la identificación de la plantilla de proyecto, puede agregar una miniatura personalizada. El tamaño recomendado sería de 319 x 319 píxeles.
   1. En CRXDE Lite, cree un nuevo archivo como elemento secundario de los nodos gadgets, roles y flujos de trabajo denominados **thumbnail.png**.
   1. Guarde y, a continuación, navegue hasta el `jcr:content` nodo y haga clic en la `jcr:data` propiedad en el doble (evite hacer clic en &#39;vista&#39;).
      1. Esto debería solicitarle un cuadro de diálogo de edición `jcr:data` de archivos y puede cargar una miniatura personalizada.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

Representación XML finalizada de la plantilla de proyecto:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## Prueba de la plantilla de proyecto personalizada

Ahora podemos probar nuestra plantilla de proyecto creando un nuevo proyecto.

1. Debe ver la plantilla personalizada como una de las opciones para la creación del proyecto.

   ![Elegir plantilla](./assets/develop-aem-projects/choose-template.png)

1. Después de seleccionar la plantilla personalizada, haga clic en &#39;Siguiente&#39; y observe que al rellenar miembros del proyecto puede agregarlos como una función de aprobador.

   ![Aprobar](./assets/develop-aem-projects/user-approver.png)

1. Haga clic en &#39;Crear&#39; para terminar de crear el proyecto basado en la plantilla personalizada. Verá en el Panel Proyecto que el icono de Tareas y los demás mosaicos configurados en gadgets aparecen automáticamente.

   ![Mosaico Tareas](./assets/develop-aem-projects/tasks-tile.png)


## ¿Por qué el flujo de trabajo?

Tradicionalmente, los flujos de trabajo de AEM que se centran en un proceso de aprobación han utilizado los pasos del flujo de trabajo de los participantes. AEM bandeja de entrada incluye detalles sobre Tareas y flujo de trabajo y una integración mejorada con AEM proyectos. Estas funciones hacen que el uso de los pasos del proceso de creación de Tareas de proyectos sea una opción más atractiva.

### ¿Por qué Tareas?

El uso de un paso de creación de Tareas en comparación con los pasos tradicionales del participante oferta un par de ventajas:

* **Inicio y fecha** de vencimiento: facilita a los autores la gestión de su hora. La nueva función Calendario hace uso de estas fechas.
* **Prioridad** : las prioridades integradas de Bajo, Normal y Alto permiten a los autores priorizar el trabajo
* **Comentarios** enlazados: mientras los autores trabajan en una tarea, tienen la capacidad de dejar comentarios aumentando la colaboración
* **Visibilidad** : los mosaicos de Tarea y las vistas con los proyectos permiten a los administradores realizar vistas sobre cómo se invierte el tiempo
* **Integración** de proyectos: las Tareas ya están integradas con las funciones y paneles de proyectos

Al igual que los pasos de los participantes, las Tareas se pueden asignar y enrutar de forma dinámica. Los metadatos de tarea como Título, Prioridad también se pueden establecer dinámicamente en función de acciones anteriores, como se verá en el siguiente tutorial.

Aunque las Tareas tienen algunas ventajas con respecto a los pasos de los participantes, conllevan costes generales adicionales y no son tan útiles fuera de un proyecto. Además, todo el comportamiento dinámico de las Tareas debe codificarse mediante secuencias de comandos de ecma que tengan sus propias limitaciones.

## Requisitos de caso de uso de muestra {#goals-tutorial}

![Diagrama del proceso de flujo de trabajo](./assets/develop-aem-projects/workflow-process-diagram.png)

El diagrama anterior describe los requisitos de alto nivel para nuestro flujo de trabajo de aprobación de muestras.

El primer paso será crear una Tarea para terminar de editar un fragmento de contenido. Permitiremos que el iniciador del flujo de trabajo elija al usuario asignado de esta primera tarea.

Una vez completada la primera tarea, el usuario asignado tendrá tres opciones para el enrutamiento del flujo de trabajo:

**Normal **- enrutamiento normal crea una tarea asignada al grupo Aprobador del proyecto para revisarla y aprobarla. La prioridad de la tarea es Normal y la fecha de vencimiento es de 5 días a partir de su creación.

**Rush** - rush enrutamiento también crea una tarea asignada al grupo Aprobador del proyecto. La prioridad de la tarea es alta y la fecha de vencimiento es sólo 1 día.

**Omitir** : en este flujo de trabajo de muestra, el participante inicial tiene la opción de omitir el grupo de aprobación. (sí, esto podría frustrar el propósito de un flujo de trabajo de &#39;aprobación&#39;, pero nos permite ilustrar funciones de enrutamiento adicionales)

El grupo de aprobadores puede aprobar el contenido o enviarlo de vuelta al usuario asignado inicial para que lo vuelva a trabajar. En el caso de que se le devuelva para volver a trabajar, se crea una nueva tarea con la etiqueta &#39;Enviada para volver a trabajar&#39;.

El último paso del flujo de trabajo hace uso del paso de proceso Activar página/recurso y replica la carga útil.

## Creación del modelo de flujo de trabajo

1. En el menú Inicio de AEM, vaya a Herramientas -> Flujo de trabajo -> Modelos. Haga clic en &#39;Crear&#39; en la esquina superior derecha para crear un nuevo modelo de flujo de trabajo.

   Asigne un título al nuevo modelo: &quot;Flujo de trabajo de aprobación de contenido&quot; y un nombre de URL: &quot;content-approved-workflow&quot;.

   ![Cuadro de diálogo Crear flujo de trabajo](./assets/develop-aem-projects/workflow-create-dialog.png)

   Para obtener más información relacionada con la [creación de flujos de trabajo, lea aquí](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html).

1. Como práctica recomendada, los flujos de trabajo personalizados deben agruparse en su propia carpeta debajo de /etc/workflow/models. En CRXDE Lite, cree un nuevo **&#39;nt:folder&#39;** debajo de /etc/workflow/models denominado **&quot;aem-guide&quot;**. Añadir una subcarpeta garantiza que los flujos de trabajo personalizados no se sobrescriban accidentalmente durante las actualizaciones o instalaciones de Service Pack.

   *Tenga en cuenta que es importante no colocar nunca la carpeta o los flujos de trabajo personalizados bajo subcarpetas de ubicación como /etc/workflow/models/dam o /etc/workflow/models/projects, ya que la subcarpeta completa también se puede sobrescribir con actualizaciones o Service Packs.

   ![Ubicación del modelo de flujo de trabajo en 6.3](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Ubicación del modelo de flujo de trabajo en 6.3

   >[!NOTE]
   >
   >Si se usa AEM 6.4 o posterior, la ubicación del flujo de trabajo ha cambiado. Consulte [aquí para obtener más detalles.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   Si se utiliza AEM 6.4 o posterior, se creará el modelo de flujo de trabajo en `/conf/global/settings/workflow/models`. Repita los pasos anteriores con el directorio /conf y agregue una subcarpeta llamada `aem-guides` y mueva la `content-approval-workflow` debajo.

   ![Ubicación](./assets/develop-aem-projects/modern-workflow-definition-location.png)de definición de flujo de trabajo moderna Ubicación del modelo de flujo de trabajo en 6.4 o más

1. La AEM 6.3 incorpora la capacidad de agregar fases de flujo de trabajo a un flujo de trabajo determinado. El usuario verá las etapas desde la Bandeja de entrada en la ficha Información del flujo de trabajo. Mostrará al usuario la etapa actual del flujo de trabajo, así como las etapas anteriores y posteriores.

   Para configurar las etapas, abra el cuadro de diálogo Propiedades de la página desde SideKick. La cuarta ficha está etiquetada como &quot;Fases&quot;. Añada los siguientes valores para configurar las tres etapas de este flujo de trabajo:

   1. Editar contenido
   1. Aprobación
   1. Publicación

   ![configuración de etapas de flujo de trabajo](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Configure las etapas del flujo de trabajo desde el cuadro de diálogo Propiedades de la página.

   ![barra de progreso del flujo de trabajo](./assets/develop-aem-projects/workflow-info-progress.png)

   Barra de progreso del flujo de trabajo tal como se ve en la Bandeja de entrada de AEM.

   Opcionalmente, puede cargar una **imagen** en las Propiedades de página que se utilizarán como miniatura del flujo de trabajo cuando los usuarios la seleccionen. Las dimensiones de la imagen deben ser de 319 x 319 píxeles. Añadir una **descripción** en las propiedades de página también se mostrará cuando un usuario vaya a seleccionar el flujo de trabajo.

1. El proceso de flujo de trabajo Crear Tarea de proyecto está diseñado para crear una Tarea como un paso en el flujo de trabajo. Sólo después de completar la tarea se avanzará en el flujo de trabajo. Un aspecto importante del paso Crear Tarea de proyecto es que puede leer los valores de metadatos del flujo de trabajo y usarlos para crear dinámicamente la tarea.

   Primero elimine el paso de participante que se crea de forma predeterminada. En la barra de tareas del menú de componentes, expanda el subtítulo **&quot;Proyectos&quot;** y arrastre y suelte el **&quot;Crear Tarea de proyecto&quot;** en el modelo.

   Doble+Haga clic en el paso &quot;Crear Tarea de proyecto&quot; para abrir el cuadro de diálogo de flujo de trabajo. Configure las siguientes propiedades:

   Esta ficha es común para todos los pasos del proceso de flujo de trabajo y estableceremos el Título y la Descripción (no serán visibles para el usuario final). La propiedad importante que estableceremos es la etapa de flujo de trabajo para **&quot;Editar contenido&quot;** en el menú desplegable.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   El proceso de flujo de trabajo Crear Tarea de proyecto está diseñado para crear una Tarea como un paso en el flujo de trabajo. La ficha Tarea nos permite establecer todos los valores de la tarea. En nuestro caso, queremos que el usuario asignado sea dinámico, así que lo dejaremos en blanco. El resto de los valores de propiedad:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   La ficha enrutamiento es un cuadro de diálogo opcional que puede especificar las acciones disponibles para el usuario que completa la tarea. Estas acciones son solo valores de cadena y se guardarán en los metadatos del flujo de trabajo. Estos valores se pueden leer mediante secuencias de comandos o pasos de proceso más adelante en el flujo de trabajo para &quot;enrutar&quot; dinámicamente el flujo de trabajo. En función de los objetivos [del](#goals-tutorial) flujo de trabajo, agregaremos tres acciones a esta ficha:

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   Esta ficha nos permite configurar una secuencia de comandos de Tarea previa a la creación, donde podemos decidir mediante programación varios valores de la Tarea antes de crearla. Tenemos la opción de apuntar el script a un archivo externo o de incrustar un breve script directamente en el cuadro de diálogo. En nuestro caso, señalaremos la secuencia de comandos de Tarea previa a la creación a un archivo externo. En el paso 5 crearemos ese script.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. En el paso anterior hemos hecho referencia a un script de Tarea de precreación. Ahora crearemos esa secuencia de comandos en la que estableceremos el usuario asignado de la Tarea en función del valor de un valor de metadatos de flujo de trabajo &quot;**usuario** asignado&quot;. El valor **&quot;asignado&quot;** se establecerá cuando se inicie el flujo de trabajo. También leeremos los metadatos del flujo de trabajo para elegir dinámicamente la prioridad de la tarea leyendo el valor &quot;**taskPriority&quot;** de los metadatos del flujo de trabajo, así como el **&quot;taskDueDate&quot; **para establecer dinámicamente cuando venza la primera tarea.

   Para fines organizativos, hemos creado una carpeta debajo de la carpeta de la aplicación para albergar todos los scripts relacionados con el proyecto: **/apps/aem-guide/projects-tareas/projects/scripts**. Cree un nuevo archivo bajo esta carpeta llamado **&quot;inicio-tarea-config.ecma&quot;**. *Tenga en cuenta que la ruta del archivo inicio-tarea-config.ecma coincide con la ruta establecida en la ficha Configuración avanzada del paso 4.

   Añada lo siguiente como el contenido del archivo:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. Vuelva al flujo de trabajo de aprobación de contenido. Arrastre y suelte el componente **O dividido** (que se encuentra en la barra de tareas debajo de la categoría &#39;Flujo de trabajo&#39;) debajo del paso de Tarea **de** Inicio. En el Cuadro de diálogo común, seleccione el botón de opción para 3 ramas. La división OR leerá el valor de metadatos del flujo de trabajo **&quot;lastTaskAction&quot;** para determinar la ruta del flujo de trabajo. La propiedad **&quot;lastTaskAction&quot;** se establecerá en uno de los valores de la ficha Enrutamiento configurados en el paso 4. Para cada una de las fichas Ramificación, rellene el área de texto de la **secuencia de comandos** con los siguientes valores:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   *Tenga en cuenta que estamos haciendo una coincidencia de cadena directa para determinar la ruta, por lo que es importante que los valores establecidos en las secuencias de comandos de rama coincidan con los valores de ruta establecidos en el paso 4.

1. Arrastre y suelte otro paso &quot;**Crear Tarea** de proyecto&quot; en el modelo a la izquierda (Rama 1) debajo de la división O. Complete el cuadro de diálogo con las siguientes propiedades:

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Dado que esta es la ruta de aprobación normal, la prioridad de la tarea se establece en Media. Además, damos al grupo Aprobadores 5 días para completar la Tarea. El usuario asignado se deja en blanco en la ficha Tarea, ya que lo asignaremos de forma dinámica en la ficha Configuración avanzada. Proporcionamos al grupo Aprobadores dos rutas posibles al completar esta tarea: **&quot;Aprobar y publicar&quot;** si aprueban el contenido y se puede publicar y **&quot;Enviar de vuelta para revisión&quot;** si hay problemas que el editor original necesita corregir. El aprobador puede dejar comentarios que el editor original verá si se le devuelve el flujo de trabajo.

Anteriormente, en este tutorial, creamos una plantilla de proyecto que incluía una función de aprobador. Cada vez que se crea un nuevo proyecto a partir de esta plantilla, se crea un grupo específico para el proyecto para la función Aprobadores. Al igual que un paso de participante, una Tarea solo se puede asignar a un usuario o grupo. Queremos asignar esta tarea al grupo de proyectos que corresponda al Grupo de aprobadores. Todos los flujos de trabajo que se inician desde dentro de un proyecto tendrán metadatos que asignan las funciones del proyecto al grupo específico del proyecto.

Copie y pegue el siguiente código en el área de texto **Script** de la ficha **Configuración avanzada **ficha. Este código leerá los metadatos del flujo de trabajo y asignará la tarea al grupo Aprobadores del proyecto. Si no encuentra el valor del grupo aprobadores, volverá a asignar la tarea al grupo Administradores.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Arrastre y suelte otro paso &quot;**Crear Tarea** de proyecto&quot; en el modelo a la rama media (Ramificación 2) debajo de la división O. Complete el cuadro de diálogo con las siguientes propiedades:

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Dado que esta es la ruta Rush Approval, la prioridad de la tarea se establece en High. Además, damos al grupo Aprobadores un solo día para completar la tarea. El usuario asignado se deja en blanco en la ficha Tarea, ya que lo asignaremos de forma dinámica en la ficha Configuración avanzada.

   Podemos reutilizar el mismo fragmento de secuencia de comandos que en el paso 7 para rellenar el área de texto de la **secuencia de comandos** en la ficha** Configuración avanzada **. Copie y pegue el código siguiente:

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. Arrastre y suelte un componente** sin operación** en la rama de la derecha (Rama 3). El componente Sin operación no realiza ninguna acción y avanzará inmediatamente, representando el deseo del editor original de omitir el paso de aprobación. Técnicamente podemos dejar esta rama sin ningún paso de flujo de trabajo, pero como práctica recomendada agregaremos un paso Sin Operación. Esto deja claro a otros desarrolladores cuál es el propósito de la Rama 3.

   Doble haga clic en el paso del flujo de trabajo y configure el Título y la Descripción:

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![modelo de flujo de trabajo O dividido](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   El modelo de flujo de trabajo debe tener este aspecto después de configurar las tres ramas de la división O.

1. Dado que el grupo Aprobadores tiene la opción de enviar el flujo de trabajo de vuelta al editor original para futuras revisiones, dependeremos del paso **Goto** para leer la última acción realizada y dirigir el flujo de trabajo al principio o dejar que continúe.

   Arrastre y suelte el componente Paso Goto (que se encuentra en la barra de tareas en Flujo de trabajo) debajo de la división O donde se reune. Haga clic en el doble y configure las siguientes propiedades en el cuadro de diálogo:

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   La última pieza que configuraremos es el Script como parte del paso de proceso Goto. El valor Script se puede incrustar mediante el cuadro de diálogo o configurar para que apunte a un archivo externo. La secuencia de comandos Goto debe contener una **función check()** y devolver true si el flujo de trabajo debe ir al paso especificado. Un retorno de false produce un avance en el flujo de trabajo.

   Si el grupo aprobador elige la acción **&quot;Enviar para revisión&quot;** (configurada en los pasos 7 y 8), entonces queremos devolver el flujo de trabajo al paso **&quot;Creación de Tarea de Inicio&quot;** .

   En la ficha Proceso, agregue el siguiente fragmento de código al área de texto Secuencia de comandos:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. Para publicar la carga útil, utilizaremos el paso **Activar página/Proceso de recursos** . Este paso del proceso requiere poca configuración y agregará la carga útil del flujo de trabajo a la cola de replicación para la activación. Se agregará el paso debajo del paso Goto y de esta forma solo se podrá acceder a él si el grupo Aprobador ha aprobado el contenido para publicación o si el editor original ha elegido la ruta Omitir aprobación.

   Arrastre y suelte el paso **Activar página/Proceso de recursos** (que se encuentra en la barra de tareas en Flujo de trabajo de WCM) debajo de Paso Goto en el modelo.

   ![modelo de flujo de trabajo completado](assets/develop-aem-projects/workflow-model-final.png)

   Aspecto del modelo de flujo de trabajo después de agregar el paso Ir y el paso Activar página/recurso.

1. Si el grupo Aprobador devuelve el contenido para su revisión, queremos comunicárselo al editor original. Podemos hacerlo cambiando dinámicamente las propiedades de creación de Tareas. Se desactivará el valor de la propiedad lastActionTaken de **&quot;Send Back for Revision&quot;**. Si ese valor está presente, modificaremos el título y la descripción para indicar que esta tarea es el resultado de que el contenido se devuelve para su revisión. También actualizaremos la prioridad a **&quot;Alta&quot;** para que sea el primer elemento en el que trabaja el editor. Por último, estableceremos la fecha de vencimiento de la tarea en un día a partir del momento en que se devolvió el flujo de trabajo para su revisión.

   Reemplace la secuencia de comandos de inicio `start-task-config.ecma` (creada en el paso 5) con lo siguiente:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## Creación del asistente para el &quot;flujo de trabajo de inicio&quot; {#start-workflow-wizard}

Al iniciar un flujo de trabajo desde un proyecto, debe especificar un asistente para el inicio del flujo de trabajo. El asistente predeterminado: `/libs/cq/core/content/projects/workflowwizards/default_workflow` permite al usuario introducir un título de flujo de trabajo, un comentario de inicio y una ruta de carga útil para que se ejecute el flujo de trabajo. También hay otros ejemplos encontrados en: `/libs/cq/core/content/projects/workflowwizards`.

La creación de un asistente personalizado puede ser muy eficaz, ya que puede recopilar información crítica antes de los inicios del flujo de trabajo. Los datos se almacenan como parte de los metadatos del flujo de trabajo y los procesos de flujo de trabajo pueden leerlos y cambiar dinámicamente el comportamiento en función de los valores introducidos. Crearemos un asistente personalizado para asignar dinámicamente la primera tarea del flujo de trabajo en función de un valor del asistente para inicios.

1. En CRXDE-Lite crearemos una subcarpeta debajo de la `/apps/aem-guides/projects-tasks/projects` carpeta llamada &quot;asistentes&quot;. Copie el asistente predeterminado de: `/libs/cq/core/content/projects/workflowwizards/default_workflow` debajo de la carpeta de asistentes recién creada y cámbiele el nombre a **contenido-aprobación-inicio**. La ruta completa debería ser: `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   El asistente predeterminado es un asistente de dos columnas con la primera columna con Título, Descripción y Miniatura del modelo de flujo de trabajo seleccionado. La segunda columna incluye campos para Título del flujo de trabajo, Comentario del Inicio y Ruta de carga útil. El asistente es un formulario de IU táctil estándar y utiliza componentes [de formulario de](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/index.html) Granite estándar para rellenar los campos.

   ![asistente para flujo de trabajo de aprobación de contenido](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. Se agregará un campo adicional al asistente que se utilizará para establecer el usuario asignado de la primera tarea del flujo de trabajo (consulte [Creación del modelo](#create-workflow-model)de flujo de trabajo: Paso 5).

   Debajo de `../content-approval-start/jcr:content/items/column2/items` crear un nuevo nodo de tipo `nt:unstructured` llamado **&quot;asignar&quot;**. Utilizaremos el componente Selector de usuario de proyectos (basado en el componente [Selector de usuario de](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/userpicker/index.html)granito). Este campo de formulario permite restringir fácilmente la selección de usuarios y grupos solo a los que pertenecen al proyecto actual.

   A continuación se muestra la representación XML del nodo de **asignación** :

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. También agregaremos un campo de selección de prioridad que determinará la prioridad de la primera tarea en el flujo de trabajo (consulte [Creación del modelo](#create-workflow-model)de flujo de trabajo: Paso 5).

   Debajo de `/content-approval-start/jcr:content/items/column2/items` crear un nuevo nodo de tipo `nt:unstructured` llamado **priority**. Utilizaremos el componente [de selección de](https://docs.adobe.com/docs/en/aem/6-2/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/select/index.html) Granite UI para rellenar el campo de formulario.

   Debajo del nodo de **prioridad** agregaremos un nodo **items** de **nt:unestructure**. Debajo del nodo **items** , agregue 3 nodos más para rellenar las opciones de selección de Alto, Medio y Bajo. Cada nodo es de tipo **nt:unestructure** y debe tener una propiedad **text** y **value** . Tanto el texto como el valor deben ser el mismo:

   1. Alta
   1. Media
   1. Baja

   Para el nodo Medio, agregue una propiedad booleana adicional denominada &quot;**selected&quot;** con un valor definido como **true**. Esto garantizará que Media sea el valor predeterminado en el campo de selección.

   A continuación se muestra una representación XML de la estructura de nodos y las propiedades:

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. Permitiremos que el iniciador del flujo de trabajo establezca la fecha de vencimiento de la tarea inicial. Utilizaremos el campo de formulario [Granite UI DatePicker](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/datepicker/index.html) para capturar esta entrada. También agregaremos un campo oculto con una [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) para garantizar que la entrada se almacene como una propiedad Date en el JCR.

   Añada dos nodos **nt:no estructurados** con las siguientes propiedades representadas a continuación en XML:

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. Puede vista del código completo para el cuadro de diálogo del asistente de inicio [aquí](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## Conexión del flujo de trabajo y la plantilla del proyecto {#connecting-workflow-project}

Lo último que tenemos que hacer es garantizar que el modelo de flujo de trabajo esté disponible para su lanzamiento desde uno de los proyectos. Para ello, necesitamos volver a visitar la plantilla de proyecto que hemos creado en la parte 1 de esta serie.

La configuración de flujo de trabajo es un área de una plantilla de proyecto que especifica los flujos de trabajo disponibles que se utilizarán con ese proyecto. La configuración también se encarga de especificar el Asistente para flujo de trabajo de Inicio al iniciar el flujo de trabajo (que hemos creado en los pasos [anteriores)](#start-workflow-wizard). La configuración de flujo de trabajo de una plantilla de proyecto es &quot;activa&quot;, lo que significa que la actualización de la configuración de flujo de trabajo afectará tanto a los proyectos nuevos creados como a los existentes que utilicen la plantilla.

1. En CRXDE-Lite, vaya a la plantilla de proyecto de creación creada anteriormente en `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   Debajo del nodo de modelos, agregue un nuevo nodo denominado **contentapproved** con un tipo de nodo **nt:unestructure**. Añada las siguientes propiedades en el nodo:

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Si se utiliza AEM 6.4, la ubicación del flujo de trabajo ha cambiado. Apunte la `modelId` propiedad a la ubicación del modelo de flujo de trabajo de tiempo de ejecución en `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >Consulte [aquí para obtener más información sobre el cambio en la ubicación del flujo de trabajo.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Una vez agregado el flujo de trabajo de aprobación de contenido a la plantilla de proyecto, debe estar disponible para iniciarse desde el mosaico de flujo de trabajo del proyecto. Adelante, lanza y juega con los diversos enrutamientos que hemos creado.

## Materiales de apoyo

* [Descargar paquete de tutoriales finalizado](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repositorio de código completo en GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [Documentación de AEM proyectos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
