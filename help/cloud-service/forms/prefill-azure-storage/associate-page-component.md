---
title: Asociar el componente de página con la nueva plantilla de formulario adaptable
description: Crear un nuevo componente de página
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 5%

---

# Asocie el componente de página con la plantilla

El siguiente paso es asociar el componente de página con la nueva plantilla de formulario adaptable. Esto garantiza que el código del componente de página se ejecute cada vez que se procese un formulario adaptable basado en la nueva plantilla. Para los fines de este tutorial, una nueva plantilla de formulario adaptable llamada **Almacenar y restaurar desde Azure** se ha creado en **AzurePortalStorage** carpeta.
Vaya al nodo /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content, agregue la siguiente propiedad y guarde los cambios.

| **Nombre de propiedad** | **Tipo de propiedad** | **Valor de propiedad** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Cadena | azureportalpagecomponent/component/page/storeandfetch |

Vaya al nodo /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr:content, agregue la siguiente propiedad y guarde los cambios.
| **Nombre de propiedad**  | **Tipo de propiedad** | **Valor de propiedad**                                    | |--------------------|-------------------|-------------------------------------------------------| | sling:resourceType | Cadena | azureportalpagecomponent/component/page/storeandfetch |


## Pasos siguientes

[Crear integración con Azure Storage](./create-fdm.md)
