---
title: Registro de tipos de recursos personalizados
seo-title: Registering Custom Asset Types
description: Habilitar tipos de recursos personalizados para incluirlos en el portal de AEM Forms
seo-description: Enabling custom asset types for listing in AEMForms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 2%

---

# Registro de tipos de recursos personalizados {#registering-custom-asset-types}

Habilitar tipos de recursos personalizados para incluirlos en el portal de AEM Forms

>[!NOTE]
>
>AEM Asegúrese de que tiene instalado la versión 6.3 con el SP1 y el complemento de AEM Forms correspondiente. Esta función solo funciona con AEM Forms 6.3 SP1 y versiones posteriores

## Especificar ruta base {#specify-base-path}

La ruta base es la ruta del repositorio de nivel superior que comprende todos los recursos que un usuario puede desear enumerar en el componente Buscar y listar. Si lo desea, el usuario también puede configurar ubicaciones específicas dentro de la ruta base desde el cuadro de diálogo de edición de componentes, de modo que la búsqueda se active en ubicaciones específicas en lugar de buscar todos los nodos dentro de la ruta base. De forma predeterminada, la ruta base se utiliza como criterio de ruta de búsqueda para recuperar los recursos a menos que el usuario configure un conjunto de rutas específicas desde esta ubicación. Es importante tener un valor óptimo de esta ruta para realizar una búsqueda eficaz. El valor predeterminado de la ruta base se mantendrá como **_/content/dam/formsanddocuments_** porque todos los recursos de AEM Forms residen en **_/content/dam/formsanddocuments._**

Pasos para configurar la ruta base

1. Iniciar sesión en crx
1. Vaya a **/libs/fd/fp/extensions/querybuilder/basepath**

1. Haga clic en &quot;Nodo de superposición&quot; en la barra de herramientas
1. Asegúrese de que la ubicación de la superposición sea &quot;/apps/&quot;
1. Haga clic en Ok
1. Haga clic en Guardar
1. Navegue hasta la nueva estructura creada en **/apps/fd/fp/extensions/querybuilder/basepath**

1. Cambie el valor de la propiedad path a **&quot;/content/dam&quot;**
1. Haga clic en Guardar

Al especificar la propiedad de ruta a **&quot;/content/dam&quot;** básicamente está estableciendo Ruta básica en /content/dam. Esto se puede comprobar abriendo el componente Buscar y listar.

![basepath](assets/basepath.png)

## Registrar tipos de recursos personalizados {#register-custom-asset-types}

Hemos añadido una nueva pestaña (Lista de recursos) en el componente Buscar y listar. Esta pestaña enumera los tipos de recursos predeterminados y los tipos de recursos adicionales que puede configurar. De forma predeterminada, se enumeran los siguientes tipos de recursos

1. Formularios adaptables
1. Plantillas de formulario
1. Formularios PDF
1. Documento (PDF estáticos)

**Pasos para registrar el tipo de recurso personalizado**

1. Crear nodo de superposición de **/libs/fd/fp/extensions/querybuilder/assettypes**

1. Establezca la ubicación de las superposiciones en &quot;/apps&quot;
1. Navegue hasta la nueva estructura creada en `/apps/fd/fp/extensions/querybuilder/assettypes`

1. En esta ubicación, cree un nodo &quot;nt:unstructured&quot; para el tipo que se va a registrar, asigne un nombre al nodo **mp4files. Agregue las dos propiedades siguientes a este nodo mp4files**

   1. Agregue la propiedad jcr:title para especificar el nombre para mostrar del tipo de recurso. Establezca el valor de jcr:title en &quot;Archivos Mp4&quot;.
   1. Agregue la propiedad &quot;type&quot; y establezca su valor en &quot;videos&quot;. Este es el valor que utilizamos en nuestra plantilla para enumerar los recursos del tipo vídeos. Guarde los cambios.

1. Cree un nodo de tipo &quot;nt:unstructured&quot; en mp4files. Asigne a este nodo el nombre &quot;searchcriteria&quot;
1. Añada uno o más filtros en los criterios de búsqueda. Supongamos que si el usuario desea tener un filtro de búsqueda para enumerar mp4Files cuyo tipo mime es &quot;video/mp4&quot;, puede hacerlo aquí
1. Cree un nodo de tipo &quot;nt:unstructured&quot; bajo los criterios de búsqueda del nodo. Asigne a este nodo el nombre &quot;filetypes&quot;
1. Agregue las dos propiedades siguientes a este nodo &quot;filetypes&quot;

   1. name: ./jcr:content/metadata/dc:format
   1. value: video/mp4

1. Esto significa que los recursos que tienen la propiedad dc:format igual a video/mp4 se consideran un tipo de recurso &quot;Mp4 Videos&quot;. Puede utilizar cualquier propiedad enumerada en el nodo &quot;jcr:content/metadata&quot; para los criterios de búsqueda

1. **Asegúrese de guardar el trabajo**

Después de realizar los pasos anteriores, el nuevo tipo de recurso (archivos Mp4) empezará a mostrarse en la lista desplegable de tipos de recurso del componente Buscar y listar como se muestra a continuación

![mp4files](assets/mp4files.png)

[Si tiene problemas para que esto funcione, puede importar el siguiente paquete.](assets/assettypeskt1.zip) El paquete tiene definidos dos tipos de recursos personalizados. Archivos Mp4 y documentos de Word. Le sugerimos que eche un vistazo a la **/apps/fd/fp/extensions/querybuilder/assettypes**

[Instalación del paquete del portal personalizado](assets/customportalpage.zip). Este paquete contiene una página de portal de ejemplo. Esta página se utiliza en la parte 2 de este tutorial
