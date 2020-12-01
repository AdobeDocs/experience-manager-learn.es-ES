---
title: Registro de tipos de recursos personalizados
seo-title: Registro de tipos de recursos personalizados
description: Activación de tipos de recursos personalizados para incluirlos en el portal de AEMForms
seo-description: Activación de tipos de recursos personalizados para incluirlos en el portal de AEMForms
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '666'
ht-degree: 2%

---


# Registrando tipos de recursos personalizados {#registering-custom-asset-types}

Activación de tipos de recursos personalizados para incluirlos en el portal de AEMForms

>[!NOTE]
>
>Asegúrese de que tiene AEM 6.3 con SP1 y el correspondiente AEM Forms Añadir al instalar. Esta función solo funciona con AEM Forms 6.3 SP1 y posterior

## Especificar ruta base {#specify-base-path}

La ruta de acceso base es la ruta del repositorio de nivel superior que comprende todos los recursos que un usuario puede querer lista en el componente de búsqueda y lista. Si lo desea, el usuario también puede configurar ubicaciones específicas dentro de la ruta base desde el cuadro de diálogo de edición de componentes, de modo que la búsqueda se active en ubicaciones específicas en lugar de buscar todos los nodos dentro de la ruta base. De forma predeterminada, la ruta base se utiliza como criterio de ruta de búsqueda para recuperar los recursos, a menos que el usuario configure un conjunto de rutas específicas desde esta ubicación. Es importante tener un valor óptimo de esta ruta para realizar una búsqueda de rendimiento. El valor predeterminado de la ruta de acceso base permanecerá como **_/content/dam/formsanddocuments_** porque todos los recursos de AEM Forms residen en **_/content/dam/formsanddocuments._**

Pasos para configurar la ruta base

1. Iniciar sesión en crx
1. Vaya a **/libs/fd/fp/extension/querybuilder/basepath**

1. Haga clic en &quot;Nodo superpuesto&quot; en la barra de herramientas
1. Asegúrese de que la ubicación de la superposición es &quot;/apps/&quot;
1. Haga clic en Aceptar
1. Haga clic en Guardar
1. Vaya a la nueva estructura creada en **/apps/fd/fp/Extensions/querybuilder/basepath**

1. Cambie el valor de la propiedad path a **&quot;/content/dam&quot;**
1. Haga clic en Guardar

Al especificar la propiedad path en **&quot;/content/dam&quot;**, básicamente está configurando la ruta base en /content/dam. Esto se puede verificar abriendo el componente Búsqueda y listado.

![basepath](assets/basepath.png)

## Registrar tipos de recursos personalizados {#register-custom-asset-types}

Se ha añadido una nueva ficha (Lista de recursos) en el componente de búsqueda y lista. Esta ficha lista los tipos de recursos predeterminados y los tipos de recursos adicionales que se configuran. De forma predeterminada, se enumeran los siguientes tipos de recursos

1. Formularios adaptables
1. Plantillas de formulario
1. PDF forms
1. Documento (PDF estáticos)

**Pasos para registrar el tipo de recurso personalizado**

1. Cree un nodo de superposición de **/libs/fd/fp/extension/querybuilder/assettypes**

1. Definir la ubicación de la superposición como &quot;/apps&quot;
1. Navegue a la nueva estructura creada en **/apps/fd/fp/extension/querybuilder/assettypes **

1. En esta ubicación, cree un nodo &#39;nt:desestructurado&#39; para el tipo que se va a registrar, asigne un nombre al nodo **mp4files. Añada las dos propiedades siguientes a este nodo mp4files**

   1. Añada la propiedad jcr:title para especificar el nombre para mostrar del tipo de recurso. Establezca el valor de jcr:title en &quot;Archivos Mp4&quot;.
   1. Añada la propiedad &quot;type&quot; y defina su valor en &quot;videos&quot;. Este es el valor que usamos en nuestra plantilla para lista de recursos del tipo de vídeos. Guarde los cambios.

1. Cree un nodo de tipo &quot;nt:desestructurado&quot; en archivos mp4X. Asigne a este nodo el nombre &quot;searchCriteria&quot;
1. Añada uno o más filtros bajo criterios de búsqueda. Supongamos que el usuario desea tener un filtro de búsqueda para lista mp4Files cuyo tipo de MIME es &quot;video/mp4&quot;, puede hacerlo aquí
1. Cree un nodo de tipo &quot;nt:desestructurado&quot; bajo los criterios de búsqueda de nodos. Asigne a este nodo el nombre &quot;filetypes&quot;
1. Añada las siguientes 2 propiedades a este nodo &quot;filetypes&quot;

   1. name: ./jcr:content/metadata/dc:format
   1. value: vídeo/mp4

1. Esto significa que los recursos con la propiedad dc:format igual a video/mp4 se considerarán un tipo de recurso &quot;Vídeos Mp4&quot;. Puede utilizar cualquier propiedad enumerada en el nodo &quot;jcr:content/metadata&quot; para los criterios de búsqueda

1. **Asegúrese de guardar su trabajo**

Después de realizar los pasos anteriores, el nuevo tipo de recurso (Archivos Mp4) aparecerá en inicio en la lista desplegable de tipos de recursos del componente Buscar y listado, como se muestra a continuación

![mp4files](assets/mp4files.png)

[Si tiene problemas para que esto funcione, puede importar el siguiente paquete.](assets/assettypeskt1.zip) El paquete tiene dos tipos de recursos personalizados definidos. Archivos y documentos de Word Mp4. Sugerir que eche un vistazo a **/apps/fd/fp/extension/querybuilder/assettypes**

[Instale el paquete](assets/customportalpage.zip) customPortal. Este paquete contiene una página de portal de muestra. Esta página se utilizará en la parte 2 de este tutorial

