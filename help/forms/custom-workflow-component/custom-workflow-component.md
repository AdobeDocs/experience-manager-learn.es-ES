---
title: Crear un componente de flujo de trabajo para guardar los archivos adjuntos en el sistema de archivos
description: Escribir archivos adjuntos de formularios adaptables en el sistema de archivos mediante el componente de flujo de trabajo personalizado
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
exl-id: acc701ec-b57d-4c20-8f97-a5a69bb180cd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# Componente de flujo de trabajo personalizado

Este tutorial está diseñado para los clientes de AEM Forms que necesiten crear un componente de flujo de trabajo personalizado. El componente de flujo de trabajo se configura para ejecutar el código escrito en el paso anterior. El componente de flujo de trabajo tiene la capacidad de especificar argumentos de proceso para el código. En este artículo exploraremos el componente de flujo de trabajo asociado al código.


[Descargar el componente de flujo de trabajo personalizado](assets/saveFiles.zip)
Importación del componente de flujo de trabajo [uso del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)

El componente de flujo de trabajo personalizado se encuentra en /apps/AEMFormsDemoListings/workflow/component/SaveFiles

Seleccione el nodo SaveFiles y examine sus propiedades

**componentGroup** : el valor de esta propiedad determina la categoría del componente de flujo de trabajo.

**jcr:Título** : título del componente del flujo de trabajo.

**sling:resourceSuperType** El valor de esta propiedad determinará la herencia de este componente. En este caso, se hereda del componente de proceso


![component-properties](assets/component-properties1.png)

## cq:dialog

Los cuadros de diálogo se utilizan para permitir que el autor interactúe con el componente. El cuadro de diálogo cq:dialog se encuentra en el nodo SaveFiles
![cq-dialog](assets/cq-dialog.png)

Los nodos bajo el nodo de elementos representan las pestañas del componente a través de las cuales los autores interactuarán con el componente. Las pestañas comunes y de proceso están ocultas. Las pestañas Común y Argumentos están visibles.

Los argumentos de proceso del proceso se encuentran en el nodo processargs

![process-args](assets/process-arguments.png)

El autor especifica los argumentos como se muestra en la captura de pantalla siguiente
![workflow-component](assets/custom-workflow-component.png)

Los valores se almacenan como propiedades del nodo de metadatos. Por ejemplo, el valor **c:\formsattachments** se almacenará en la propiedad saveToLocation del nodo de metadatos
![save-location](assets/save-to-location.png)

## cq:editConfig

cq:EditConfig es simplemente un nodo con el tipo principal cq:EditConfig y el nombre cq:editConfig bajo la raíz del componente. El comportamiento de edición de un componente se configura añadiendo un nodo cq:editConfig de tipo cq:EditConfig debajo del nodo del componente (de tipo cq:Component)

![edit-config](assets/cq-edit-config.png)

cq:formParameters (tipo de nodo nt:unstructured): define parámetros adicionales que se añaden al formulario de diálogo.


Observe las propiedades del nodo cq:formParameters
![from-parameters-properties](assets/form-parameters-properties.png)

El valor de la propiedad PROCESS indica el código Java que se asociará con el componente de flujo de trabajo.
