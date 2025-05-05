---
title: Desarrollar proyectos en AEM
description: Un tutorial de desarrollo que ilustra cómo desarrollar para proyectos de AEM. En este tutorial crearemos una plantilla de proyecto personalizada que se puede utilizar para crear nuevos proyectos en AEM para administrar los flujos de trabajo y las tareas de creación de contenido.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Projects, Workflow
doc-type: Tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
duration: 1417
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '4441'
ht-degree: 0%

---

# Desarrollar proyectos en AEM

Este es un tutorial de desarrollo que ilustra cómo desarrollar para [!DNL AEM Projects]. En este tutorial crearemos una plantilla de proyecto personalizada que se puede utilizar para crear proyectos en AEM para administrar los flujos de trabajo y las tareas de creación de contenido.

>[!VIDEO](https://video.tv.adobe.com/v/16904?quality=12&learn=on)

*Este vídeo ofrece una breve demostración del flujo de trabajo terminado que se crea en el tutorial siguiente.*

## Introducción {#introduction}

[[!DNL AEM Projects]](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) es una característica de AEM diseñada para facilitar la administración y el agrupamiento de todos los flujos de trabajo y tareas asociados con la creación de contenido como parte de una implementación de AEM Sites o Assets.

Proyectos AEM incluye varias [plantillas de proyecto OOTB](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects). Al crear un proyecto, los autores pueden elegir entre estas plantillas disponibles. Las implementaciones grandes de AEM con requisitos empresariales únicos querrán crear plantillas de proyecto personalizadas, adaptadas para satisfacer sus necesidades. Al crear una plantilla de proyecto personalizada, los desarrolladores pueden configurar el tablero del proyecto, vincularse a flujos de trabajo personalizados y crear funciones empresariales adicionales para un proyecto. Observaremos la estructura de una plantilla de proyecto y crearemos una de muestra.

![Tarjeta de proyecto personalizada](./assets/develop-aem-projects/custom-project-card.png)

## Configuración

Este tutorial recorrerá paso a paso el código necesario para crear una plantilla de proyecto personalizada. Puede descargar e instalar el [paquete adjunto](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) en un entorno local para seguirlo junto con el tutorial. También puede acceder al proyecto Maven completo alojado en [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [Paquete de tutorial finalizado](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repositorio de código completo en GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

Este tutorial asume algunos conocimientos básicos de [prácticas de desarrollo de AEM](https://experienceleague.adobe.com/es/docs/experience-manager-65/content/implementing/developing/introduction/the-basics) y cierta familiaridad con la [configuración del proyecto AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html). Todo el código mencionado está diseñado para utilizarse como referencia y solo debe implementarse en una [instancia de desarrollo local de AEM](https://experienceleague.adobe.com/es/docs/experience-manager-65/content/implementing/deploying/deploying/deploy).

## Estructura de una plantilla de proyecto

Las plantillas de proyecto deben colocarse bajo control de código fuente y deben residir debajo de la carpeta de la aplicación, en /apps. Lo ideal es que se coloquen en una subcarpeta con la convención de nombres de **&#42;/projects/templates/**&lt;my-template>. Al colocar siguiendo esta convención de nombres, las nuevas plantillas personalizadas estarán disponibles automáticamente para los autores al crear un proyecto. La configuración de las plantillas de proyecto disponibles se establece en: **/content/projects/jcr:content** mediante la propiedad **cq:allowedTemplates**. De manera predeterminada, se trata de una expresión regular: **/(apps|libs)/.&#42;/projects/templates/.&#42;**

El nodo raíz de una plantilla de proyecto tendrá un **jcr:primaryType** de **cq:Template**. Debajo del nodo raíz de hay tres nodos: **gadgets**, **roles** y **flujos de trabajo**. Todos estos nodos son **nt:unstructured**. Debajo del nodo raíz también puede haber un archivo thumbnail.png que se muestra al seleccionar la plantilla en el asistente Crear proyecto.

La estructura del nodo completo:

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

El nodo raíz de la plantilla de proyecto es del tipo **cq:Template**. En este nodo puede configurar las propiedades **jcr:title** y **jcr:description** que se muestran en el Asistente para crear proyectos. También hay una propiedad denominada **wizard** que señala a un formulario que rellenará las propiedades del proyecto. El valor predeterminado de: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** debería funcionar bien en la mayoría de los casos, ya que permite al usuario rellenar las propiedades básicas del proyecto y agregar miembros al grupo.

*&#42;Tenga en cuenta que el Asistente para crear proyectos no utiliza el servlet Sling POST. En su lugar, los valores se publican en un servlet personalizado:**com.adobe.cq.projects.impl.servlet.ProjectServlet**. Esto debe tenerse en cuenta al agregar campos personalizados.*

Se puede encontrar un ejemplo de asistente personalizado para la plantilla del proyecto de traducción: **/libs/cq/core/content/projects/wizard/translationproject/defaultproject**.

### Gadgets {#gadgets}

No hay propiedades adicionales en este nodo, pero los elementos secundarios del nodo de gadgets controlan qué mosaicos de proyecto rellenan el panel del proyecto cuando se crea un nuevo proyecto. [Los mosaicos del proyecto](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) (también conocidos como gadgets o pods) son tarjetas simples que rellenan el área de trabajo de un proyecto. Puede encontrar una lista completa de los mosaicos de ootb en: **/libs/cq/gui/components/projects/admin/pod. &#x200B;** Los propietarios de proyectos siempre pueden agregar o quitar mosaicos después de crear un proyecto.

### Funciones {#roles}

Hay tres [roles predeterminados](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) para cada proyecto: **Observadores**, **Editores** y **Propietarios**. Al agregar nodos secundarios debajo del nodo de funciones, puede agregar funciones de proyecto adicionales específicas de la empresa para la plantilla. A continuación, puede vincular estas funciones a flujos de trabajo específicos asociados al proyecto.

### Flujos de trabajo {#workflows}

Una de las razones más atractivas para crear una plantilla de proyecto personalizada es que le ofrece la capacidad de configurar los flujos de trabajo disponibles para utilizarlos con el proyecto. Pueden ser flujos de trabajo OOTB o flujos de trabajo personalizados. Debajo del nodo **workflows** debe haber un nodo **models** (también `nt:unstructured`) y nodos secundarios debajo de especifique los modelos de flujo de trabajo disponibles. La propiedad **modelId &#x200B;** apunta al modelo de flujo de trabajo en /etc/workflow y la propiedad **wizard** apunta al cuadro de diálogo utilizado al iniciar el flujo de trabajo. Una ventaja significativa de los proyectos es la capacidad de agregar un cuadro de diálogo personalizado (asistente) para capturar metadatos específicos de la empresa al principio del flujo de trabajo que puedan dirigir más acciones dentro del flujo de trabajo.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Creación de una plantilla de proyecto {#creating-project-template}

Como principalmente estamos copiando/configurando nodos, utilizaremos CRXDE Lite. En la instancia local de AEM, abre [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. Comience creando una carpeta debajo de `/apps/&lt;your-app-folder&gt;` llamada `projects`. Cree otra carpeta debajo de la denominada `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Para facilitar las cosas, empezaremos nuestra plantilla personalizada a partir de la plantilla de proyecto simple existente.

   1. Copie y pegue el nodo **/libs/cq/core/content/projects/templates/default** debajo de la carpeta *templates* creada en el paso 1.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. Ahora debería tener una ruta como **/apps/aem-guides/projects-tasks/projects/templates/authoring-project**.

   1. Edite las propiedades **jcr:title** y **jcr:description** del nodo author-project para asignar valores personalizados de título y descripción.

      1. Deje la propiedad **wizard** apuntando a las propiedades predeterminadas del proyecto.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Para esta plantilla de proyecto queremos hacer uso de Tareas.
   1. Agregue un nuevo nodo **nt:unstructured** debajo del proyecto/gadgets de creación llamados **tasks**.
   1. Agregue propiedades de cadena al nodo de tareas para **cardWeight** = &quot;100&quot;, **jcr:title**=&quot;Tasks&quot; y **sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   Ahora el [mosaico Tareas](https://experienceleague.adobe.com/en/docs) aparecerá de forma predeterminada cuando se cree un nuevo proyecto.

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

   1. Debajo del nodo de plantilla de proyecto (authoring-project) agregue un nuevo **nt:unstructured** **roles** etiquetado por nodos.
   1. Agregue otro nodo **nt:unstructured** etiquetado como aprobadores secundarios del nodo de funciones.
   1. Agregar propiedades de cadena **jcr:title** = &quot;**Aprobadores**&quot;, **roleclass** =&quot;**owner**&quot;, **roleid**=&quot;**approvers**&quot;.
      1. El nombre del nodo de aprobadores, así como jcr:title y roleid, pueden ser cualquier valor de cadena (siempre que roleid sea único).
      1. **roleclass** rige los permisos aplicados a ese rol en función de [tres roles OOTB](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects): **propietario**, **editor** y **observador**.
      1. En general, si la función personalizada es más bien una función directiva, la clase de rol puede ser **propietario;** si es una función de creación más específica como Fotógrafo o Designer, entonces **editor** de clase de rol debería ser suficiente. La gran diferencia entre **owner** y **editor** es que los propietarios del proyecto pueden actualizar las propiedades del proyecto y agregar nuevos usuarios al proyecto.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. Al copiar la plantilla Proyecto simple, obtendrá cuatro flujos de trabajo OOTB configurados. Cada nodo debajo de flujos de trabajo/modelos apunta a un flujo de trabajo específico y a un asistente de cuadro de diálogo de inicio para ese flujo de trabajo. Más adelante en este tutorial crearemos un flujo de trabajo personalizado para este proyecto. Por ahora, elimine los nodos debajo de flujo de trabajo/modelos:

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Para facilitar a los autores de contenido la identificación de la plantilla de proyecto, puede agregar una miniatura personalizada. El tamaño recomendado sería de 319 x 319 píxeles.
   1. En CRXDE Lite, cree un archivo como secundario de los nodos de gadgets, funciones y flujos de trabajo llamados **thumbnail.png**.
   1. Guarde, navegue hasta el nodo `jcr:content` y haga doble clic en la propiedad `jcr:data` (evite hacer clic en &quot;Ver&quot;).
      1. Esto le pedirá un cuadro de diálogo para editar el archivo `jcr:data` y podrá cargar una miniatura personalizada.

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

Ahora podemos probar nuestra plantilla de proyecto creando un proyecto.

1. Debería ver la plantilla personalizada como una de las opciones para crear proyectos.

   ![Elegir plantilla](./assets/develop-aem-projects/choose-template.png)

1. Después de seleccionar la plantilla personalizada, haga clic en &#39;Siguiente&#39; y observe que al rellenar Miembros del proyecto, puede agregarlos como una función de Aprobador.

   ![Aprobar](./assets/develop-aem-projects/user-approver.png)

1. Haga clic en &quot;Crear&quot; para terminar de crear el proyecto basado en la plantilla personalizada. Observará en el panel del proyecto que el mosaico Tareas y los demás mosaicos configurados en gadgets aparecen automáticamente.

   ![Mosaico de tareas](./assets/develop-aem-projects/tasks-tile.png)


## ¿Por qué Workflow?

Tradicionalmente, los flujos de trabajo de AEM centrados en un proceso de aprobación han utilizado pasos del flujo de trabajo de participantes. La bandeja de entrada de AEM incluye detalles sobre tareas y flujos de trabajo, así como una integración mejorada con los proyectos de AEM. Estas funciones hacen que el uso de los pasos del proceso de creación de tareas de proyectos sea una opción más atractiva.

### ¿Por qué tareas?

El uso de un paso de creación de tarea sobre los pasos tradicionales de participante ofrece un par de ventajas:

* **Fecha de inicio y de vencimiento**: facilita a los autores la administración de su tiempo, la nueva característica Calendario hace uso de estas fechas.
* **Prioridad**: las prioridades integradas de Baja, Normal y Alta permiten a los autores priorizar el trabajo
* **Comentarios enlazados**: a medida que los autores trabajan en una tarea, tienen la posibilidad de dejar comentarios para aumentar la colaboración
* **Visibilidad**: los mosaicos de tareas y las vistas con Proyectos permiten a los administradores ver cómo se invierte el tiempo
* **Integración del proyecto**: las tareas ya están integradas con los roles y paneles del proyecto

Al igual que los pasos de los participantes, las tareas se pueden asignar y distribuir dinámicamente. Los metadatos de la tarea, como Título y Prioridad, también se pueden establecer dinámicamente en función de acciones anteriores, como veremos en el siguiente tutorial.

Aunque las Tareas tienen algunas ventajas sobre los Pasos del Participante, sí conllevan gastos generales adicionales y no son tan útiles fuera de un Proyecto. Además, todo el comportamiento dinámico de las tareas debe codificarse mediante scripts ecma que tengan sus propias limitaciones.

## Requisitos de casos de uso de muestra {#goals-tutorial}

![Diagrama del proceso de flujo de trabajo](./assets/develop-aem-projects/workflow-process-diagram.png)

El diagrama anterior describe los requisitos de alto nivel para nuestro flujo de trabajo de aprobación de muestras.

El primer paso es crear una tarea para terminar de editar un fragmento de contenido. Permitiremos que el iniciador del flujo de trabajo elija el usuario asignado de esta primera tarea.

Una vez completada la primera tarea, el usuario asignado tendrá tres opciones para enrutar el flujo de trabajo:

**Normal &#x200B;**: el enrutamiento normal crea una tarea asignada al grupo de aprobadores del proyecto para su revisión y aprobación. La prioridad de la tarea es Normal y la fecha de vencimiento es cinco días a partir de la fecha de creación.

**Enviar** - el enrutamiento de envío también crea una tarea asignada al grupo de aprobadores del proyecto. La prioridad de la tarea es Alta y la fecha de vencimiento es solo un día.

**Omitir**: en este flujo de trabajo de muestra el participante inicial tiene la opción de omitir el grupo de aprobación. (sí, esto podría frustrar el propósito de un flujo de trabajo de &#39;Aprobación&#39;, pero nos permite ilustrar capacidades de enrutamiento adicionales)

El grupo de aprobadores puede aprobar el contenido o enviarlo de vuelta al usuario asignado inicial para que lo reprocese. En caso de que se devuelva para, se creará una nueva tarea con la etiqueta &quot;Enviado de nuevo para volver a trabajar&quot;.

El último paso del flujo de trabajo utiliza el paso ootb del proceso Activar página/recurso y replica la carga útil.

## Creación del modelo del flujo de trabajo

1. En el menú Inicio de AEM, vaya a Herramientas -> Flujo de trabajo -> Modelos. Haga clic en &quot;Crear&quot; en la esquina superior derecha para crear un modelo de flujo de trabajo.

   Asigne un título al nuevo modelo: &quot;Flujo de trabajo de aprobación de contenido&quot; y un nombre de URL: &quot;content-approval-workflow&quot;.

   ![Cuadro de diálogo de creación de flujo de trabajo](./assets/develop-aem-projects/workflow-create-dialog.png)

   [Para obtener más información relacionada con la creación de flujos de trabajo, lea este enlace](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-models).

1. Como práctica recomendada, los flujos de trabajo personalizados deben agruparse en su propia carpeta debajo de /etc/workflow/models. En CRXDE Lite, cree una **&#39;nt:folder&#39;** debajo de /etc/workflow/models denominado **&quot;aem-guides&quot;**. Añadir una subcarpeta garantiza que los flujos de trabajo personalizados no se sobrescriban accidentalmente durante las actualizaciones o instalaciones del Service Pack.

   &#42;Tenga en cuenta que es importante nunca colocar la carpeta o los flujos de trabajo personalizados debajo de subcarpetas ootb como /etc/workflow/models/dam o /etc/workflow/models/projects, ya que toda la subcarpeta también se puede sobrescribir mediante actualizaciones o paquetes de servicio.

   ![Ubicación del modelo de flujo de trabajo en 6.3](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Ubicación del modelo de flujo de trabajo en 6.3

   >[!NOTE]
   >
   >Si utiliza AEM 6.4+, la ubicación del flujo de trabajo ha cambiado. Vea [aquí para obtener más detalles.](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   Si utiliza AEM 6.4+, el modelo de flujo de trabajo se crea en `/conf/global/settings/workflow/models`. Repita los pasos anteriores con el directorio /conf, agregue una subcarpeta denominada `aem-guides` y mueva `content-approval-workflow` debajo de ella.

   ![Ubicación de definición de flujo de trabajo moderno](./assets/develop-aem-projects/modern-workflow-definition-location.png)
Ubicación del modelo de flujo de trabajo en 6.4+

1. En AEM 6.3 se introduce la capacidad de añadir fases de flujo de trabajo a un flujo de trabajo determinado. Las fases aparecerán al usuario en la bandeja de entrada de la pestaña Información del flujo de trabajo. Muestra al usuario la fase actual del flujo de trabajo, así como las fases anteriores y posteriores.

   Para configurar las fases, abra el cuadro de diálogo Propiedades de página desde Sidekick. La cuarta pestaña se denomina &quot;Fases&quot;. Añada los siguientes valores para configurar las tres fases de este flujo de trabajo:

   1. Editar contenido
   1. Aprobación
   1. Publicación

   ![configuración de fases del flujo de trabajo](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Configure las fases del flujo de trabajo desde el cuadro de diálogo Propiedades de página.

   ![barra de progreso del flujo de trabajo](./assets/develop-aem-projects/workflow-info-progress.png)

   La barra de progreso del flujo de trabajo, tal como se ve desde la bandeja de entrada AEM.

   Opcionalmente, puede cargar una **imagen** a las propiedades de página que se usan como miniatura de flujo de trabajo cuando los usuarios la seleccionan. Las dimensiones de la imagen deben ser de 319 x 319 píxeles. Agregar una **descripción** a las propiedades de página también se mostrará cuando un usuario seleccione el flujo de trabajo.

1. El proceso de flujo de trabajo Crear tarea del proyecto está diseñado para crear una tarea como paso en el flujo de trabajo. El flujo de trabajo solo avanzará una vez completada la tarea. Un aspecto importante del paso Crear tarea de proyecto es que puede leer valores de metadatos de flujo de trabajo y utilizarlos para crear la tarea de forma dinámica.

   En primer lugar, elimine el paso de participante que se crea de forma predeterminada. En Sidekick, en el menú de componentes, expanda el subencabezado **&quot;Proyectos&quot;** y arrastre y suelte **&quot;Crear tarea de proyecto&quot;** en el modelo.

   Haga doble clic en el paso &quot;Crear tarea del proyecto&quot; para abrir el cuadro de diálogo de flujo de trabajo. Configure las siguientes propiedades:

   Esta pestaña es común para todos los pasos del proceso de flujo de trabajo y estableceremos el Título y la Descripción (no serán visibles para el usuario final). La propiedad importante que estableceremos es la Fase del flujo de trabajo en **&quot;Editar contenido&quot;** del menú desplegable.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   El proceso de flujo de trabajo Crear tarea del proyecto está diseñado para crear una tarea como paso en el flujo de trabajo. La pestaña Tarea permite establecer todos los valores de la tarea. En nuestro caso, queremos que el usuario asignado sea dinámico, por lo que lo dejaremos en blanco. El resto de los valores de propiedad:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   La pestaña de enrutamiento es un cuadro de diálogo opcional que puede especificar las acciones disponibles para el usuario que completa la tarea. Estas acciones son solo valores de cadena que se guardan en los metadatos del flujo de trabajo. Estos valores se pueden leer mediante scripts o pasos de proceso más adelante en el flujo de trabajo para &quot;enrutar&quot; dinámicamente el flujo de trabajo. En función de los objetivos de flujo de trabajo que estableceremos, agregue tres acciones a esta pestaña:

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   Esta pestaña nos permite configurar un script de tarea previo a la creación en el que podemos decidir mediante programación varios valores de la tarea antes de crearla. Tenemos la opción de apuntar el script a un archivo externo o incrustar un script corto directamente en el cuadro de diálogo. En nuestro caso, señalaremos el script de tarea previo a la creación a un archivo externo. En el paso 5 crearemos esa secuencia de comandos.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. En el paso anterior hicimos referencia a un script anterior a la creación de la tarea. Crearemos ese script ahora en el cual estableceremos el usuario asignado de la tarea en función del valor de un valor de metadatos de flujo de trabajo &quot;**asignado**&quot;. El valor **&quot;usuario asignado&quot;** se establece cuando se inicia el flujo de trabajo. También leeremos los metadatos del flujo de trabajo para elegir dinámicamente la prioridad de la tarea al leer el valor &quot;**taskPriority&quot;** de los metadatos del flujo de trabajo, así como el **&#x200B; **&quot;taskDueDate&quot; que se establecerá dinámicamente cuando venza la primera tarea.

   Por motivos de organización, hemos creado una carpeta debajo de la carpeta de la aplicación para guardar todos los scripts relacionados con el proyecto: **/apps/aem-guides/projects-tasks/projects/scripts**. Cree un archivo debajo de esta carpeta denominado **&quot;start-task-config.ecma&quot;**. &#42;Tenga en cuenta que la ruta al archivo start-task-config.ecma coincide con la ruta establecida en la pestaña Configuración avanzada del paso 4.

   Agregue lo siguiente como contenido del archivo:

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

1. Vuelva al flujo de trabajo de aprobación de contenido. Arrastre y suelte el componente **OR Split** (que se encuentra en Sidekick debajo de la categoría &quot;Flujo de trabajo&quot;) debajo del paso **Iniciar tarea**. En el cuadro de diálogo Común, seleccione el botón de opción para 3 ramas. OR Split leerá el valor de metadatos de flujo de trabajo **&quot;lastTaskAction&quot;** para determinar la ruta del flujo de trabajo. La propiedad **&quot;lastTaskAction&quot;** se establece en uno de los valores de la ficha Enrutamiento configurada en el paso 4. Para cada una de las fichas de ramas, rellene el área de texto **Script** con los siguientes valores:

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

   &#42;Tenga en cuenta que estamos haciendo una coincidencia directa de cadena para determinar la ruta, por lo que es importante que los valores configurados en los scripts de rama coincidan con los valores de Ruta establecidos en el paso 4.

1. Arrastre y suelte otro paso &quot;**Crear tarea de proyecto**&quot; en el modelo en el extremo izquierdo (rama 1) debajo de la división O. Complete el cuadro de diálogo con las siguientes propiedades:

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

   Dado que es la ruta de aprobación normal, la prioridad de la tarea se establece en Medium. Además, damos al grupo de aprobadores 5 días para completar la tarea. El usuario asignado se deja en blanco en la pestaña Tarea, ya que se asigna dinámicamente en la pestaña Configuración avanzada. Al completar esta tarea, proporcionamos al grupo de aprobadores dos rutas posibles: **&quot;Aprobar y publicar&quot;** si aprueban el contenido y se puede publicar y **&quot;Enviar de nuevo para revisión&quot;** si hay problemas que el editor original debe corregir. El aprobador puede dejar comentarios que el editor original verá si se le devuelve el flujo de trabajo.

Anteriormente en este tutorial creamos una plantilla de proyecto que incluía una función de aprobador. Cada vez que se crea un nuevo proyecto a partir de esta plantilla, se crea un grupo específico de proyecto para la función Aprobadores. Al igual que una Etapa de participante, una Tarea solo puede asignarse a un Usuario o Grupo. Queremos asignar esta tarea al grupo de proyecto que corresponda al grupo de aprobadores. Todos los flujos de trabajo que se inicien desde un proyecto tendrán metadatos que asignarán las funciones del proyecto al grupo específico del proyecto.

Copie y pegue el código siguiente en el área de texto **Script** de la pestaña **Configuración avanzada &#x200B;**. Este código leerá los metadatos del flujo de trabajo y asignará la tarea al grupo de aprobadores del proyecto. Si no encuentra el valor del grupo de aprobadores, volverá a asignar la tarea al grupo de administradores.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Arrastre y suelte otro paso &quot;**Crear tarea de proyecto**&quot; en el modelo hasta la rama central (rama 2) debajo de la división O. Complete el cuadro de diálogo con las siguientes propiedades:

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

   Dado que esta es la ruta de aprobación de Rush, la prioridad de la tarea se establece en High. Además, le damos al grupo de aprobadores solo un día para completar la tarea. El usuario asignado se deja en blanco en la pestaña Tarea, ya que se asigna dinámicamente en la pestaña Configuración avanzada.

   Podemos reutilizar el mismo fragmento de script que en el paso 7 para rellenar el área de texto **Script** en la pestaña **&#x200B; Configuración avanzada &#x200B;**. Copie y pegue el siguiente código:

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. Arrastre y suelte un componente **&#x200B; Sin operación** en la rama derecha (rama 3). El componente Sin operación no realiza ninguna acción y avanzará inmediatamente, lo que representa el deseo del editor original de omitir el paso de aprobación. Técnicamente, podríamos dejar esta rama sin ningún paso de flujo de trabajo, pero como práctica recomendada, añadiremos un paso Sin operación. Esto deja claro a otros desarrolladores cuál es el propósito de la Rama 3.

   Haga doble clic en el paso del flujo de trabajo y configure el Título y la Descripción:

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![modelo de flujo de trabajo O división](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   El modelo de flujo de trabajo debe tener este aspecto una vez configuradas las tres ramas de la división O.

1. Dado que el grupo de aprobadores tiene la opción de enviar el flujo de trabajo de vuelta al editor original para realizar más revisiones, confiaremos en el paso **Ir a** para leer la última acción realizada y enrutar el flujo de trabajo al principio o permitir que continúe.

   Arrastre y suelte el componente Paso Ir a (que se encuentra en Sidekick, en Flujo de trabajo) debajo de la división O donde se vuelve a unir. Haga doble clic en y configure las siguientes propiedades en el cuadro de diálogo:

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   La última pieza que configuraremos es la secuencia de comandos como parte del paso Goto process. El valor Script se puede incrustar mediante el cuadro de diálogo o configurar para que apunte a un archivo externo. El script Goto debe contener **function check()** y devolver true si el flujo de trabajo debe ir al paso especificado. Devolución de resultados falsos para que el flujo de trabajo avance.

   Si el grupo de aprobadores elige la acción **&quot;Enviar de vuelta para revisión&quot;** (configurada en los pasos 7 y 8), entonces queremos devolver el flujo de trabajo al paso **&quot;Iniciar creación de tareas&quot;**.

   En la pestaña Proceso, agregue el siguiente fragmento al área de texto Script:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. Para publicar la carga útil usaremos el paso de proceso ootb **Activar página/recurso**. Este paso del proceso requiere poca configuración y agrega la carga útil del flujo de trabajo a la cola de replicación para su activación. Añadiremos el paso debajo del paso Ir a y de este modo solo se puede acceder a él si el grupo de aprobadores ha aprobado el contenido para su publicación o si el editor original eligió la ruta Omitir aprobación.

   Arrastre y suelte el paso del proceso **Activar página/recurso** (que se encuentra en Sidekick, en Flujo de trabajo de WCM) debajo del paso Ir a del modelo.

   ![modelo de flujo de trabajo completado](assets/develop-aem-projects/workflow-model-final.png)

   Aspecto que debería tener el modelo del flujo de trabajo después de agregar el paso Ir a y el paso Activar página/recurso.

1. Si el grupo de aprobadores devuelve el contenido para su revisión, queremos informar al editor original. Podemos lograrlo cambiando dinámicamente las propiedades de creación de la tarea. Se quitará el valor de la propiedad lastActionTaken de **&quot;Enviar de nuevo para revisión&quot;**. Si ese valor está presente, modificaremos el título y la descripción para indicar que esta tarea es el resultado de que el contenido se devuelva para su revisión. También actualizaremos la prioridad a **&quot;Alta&quot;** para que sea el primer elemento en el que trabaje el editor. Finalmente, estableceremos la fecha de vencimiento de la tarea en un día a partir de la fecha en que se envió el flujo de trabajo para su revisión.

   Reemplace el script de inicio `start-task-config.ecma` (creado en el paso 5) con lo siguiente:

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

## Creación del asistente para &quot;iniciar flujo de trabajo&quot; {#start-workflow-wizard}

Al iniciar un flujo de trabajo desde un proyecto, debe especificar un asistente para iniciarlo. El asistente predeterminado: `/libs/cq/core/content/projects/workflowwizards/default_workflow` permite al usuario introducir un Título de flujo de trabajo, un comentario de inicio y una ruta de acceso de carga útil para que se ejecute el flujo de trabajo. También se encontraron otros ejemplos en: `/libs/cq/core/content/projects/workflowwizards`.

La creación de un asistente personalizado puede ser muy eficaz, ya que puede recopilar información crítica antes de que se inicie el flujo de trabajo. Los datos se almacenan como parte de los metadatos del flujo de trabajo y los procesos de flujo de trabajo pueden leer esto y cambiar dinámicamente el comportamiento en función de los valores introducidos. Crearemos un asistente personalizado para asignar dinámicamente la primera tarea del flujo de trabajo en función de un valor de inicio del asistente.

1. En CRXDE-Lite, crearemos una subcarpeta debajo de la carpeta `/apps/aem-guides/projects-tasks/projects` llamada &quot;asistentes&quot;. Copie el asistente predeterminado de: `/libs/cq/core/content/projects/workflowwizards/default_workflow` debajo de la carpeta de asistentes recién creada y cambie su nombre a **content-approval-start**. La ruta de acceso completa debería ser: `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   El asistente predeterminado es un asistente de 2 columnas con la primera columna que muestra el título, la descripción y la miniatura del modelo de flujo de trabajo seleccionado. La segunda columna incluye campos para el Título del flujo de trabajo, el Comentario de inicio y la Ruta de carga útil. El asistente es un formulario de interfaz de usuario táctil estándar y utiliza [componentes de formulario de interfaz de usuario de Granite](https://experienceleague.adobe.com/en/docs) estándar para rellenar los campos.

   ![asistente para flujo de trabajo de aprobación de contenido](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. Agregaremos un campo adicional al asistente que se usa para establecer el usuario asignado de la primera tarea en el flujo de trabajo (consulte [Crear el modelo de flujo de trabajo](#create-workflow-model): Paso 5).

   Debajo de `../content-approval-start/jcr:content/items/column2/items` se crea un nuevo nodo de tipo `nt:unstructured` denominado **&quot;asignar&quot;**. Utilizaremos el componente Selector de usuarios de proyectos (que se basa en el [componente Selector de usuarios de Granite](https://experienceleague.adobe.com/en/docs)). Este campo de formulario facilita la restricción de la selección de usuarios y grupos únicamente a los que pertenecen al proyecto actual.

   A continuación se muestra la representación XML del nodo **assign**:

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

1. También agregaremos un campo de selección de prioridad que determinará la prioridad de la primera tarea del flujo de trabajo (consulte [Crear el modelo de flujo de trabajo](#create-workflow-model): Paso 5).

   Debajo de `/content-approval-start/jcr:content/items/column2/items` se crea un nuevo nodo de tipo `nt:unstructured` denominado **priority**. Utilizaremos [Granite UI Select component](https://experienceleague.adobe.com/es/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions) para rellenar el campo de formulario.

   Debajo del nodo **priority** agregaremos un nodo **items** de **nt:unstructured**. Debajo del nodo **items**, agregue tres nodos más para rellenar las opciones de selección Alta, Medium y Baja. Cada nodo es de tipo **nt:unstructured** y debe tener una propiedad **text** y **value**. El texto y el valor deben tener el mismo valor:

   1. Alta
   1. Media
   1. Baja

   Para el nodo Medium, agregue una propiedad booleana adicional denominada &quot;**selected&quot;** con un valor establecido en **true**. Esto garantizará que Medium sea el valor predeterminado en el campo de selección.

   A continuación se muestra una representación XML de la estructura y las propiedades del nodo:

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

1. Permitir al iniciador del flujo de trabajo establecer la fecha límite de la tarea inicial Utilizaremos el campo de formulario [Selector de fecha de interfaz de usuario de Granite](https://experienceleague.adobe.com/en/docs) para capturar esta entrada. También agregaremos un campo oculto con un [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) para garantizar que la entrada se almacene como una propiedad de tipo Fecha en el JCR.

   Agregue dos nodos **nt:unstructured** con las siguientes propiedades representadas a continuación en XML:

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

1. Puede ver el código completo del cuadro de diálogo de inicio del asistente [aquí](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## Conexión del flujo de trabajo y la plantilla de proyecto {#connecting-workflow-project}

Lo último que debemos hacer es garantizar que el modelo de flujo de trabajo esté disponible para iniciarse desde uno de los proyectos. Para ello, tenemos que volver a visitar la plantilla de proyecto que hemos creado en la parte 1 de esta serie.

La configuración del flujo de trabajo es un área de una plantilla de proyecto que especifica los flujos de trabajo disponibles que se utilizarán con ese proyecto. La configuración también es responsable de especificar el Asistente para iniciar flujo de trabajo al iniciar el flujo de trabajo (que creamos en los [pasos anteriores)](#start-workflow-wizard). La configuración de flujo de trabajo de una plantilla de proyecto está &quot;activa&quot;, lo que significa que la actualización de la configuración de flujo de trabajo afectará a los nuevos proyectos creados y a los proyectos existentes que utilicen la plantilla.

1. En CRXDE-Lite, vaya a la plantilla de proyecto de creación creada anteriormente en `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   Debajo del nodo de modelos, agregue un nuevo nodo denominado **contentapproval** con un tipo de nodo de **nt:unstructured**. Agregue las siguientes propiedades al nodo:

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Si se utiliza AEM 6.4, la ubicación del flujo de trabajo ha cambiado. Señale la propiedad `modelId` a la ubicación del modelo de flujo de trabajo de tiempo de ejecución en `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >Vea [aquí para obtener más detalles acerca del cambio en la ubicación del flujo de trabajo.](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Una vez agregado el flujo de trabajo de aprobación de contenido a la plantilla de proyecto, debe estar disponible para iniciarse desde el mosaico de flujo de trabajo del proyecto. Continúe, inicie y reproduzca con las distintas rutas que hemos creado.

## Materiales de apoyo

* [Descargar paquete de tutorial finalizado](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repositorio de código completo en GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [Documentación de proyectos de AEM](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects)
