---
title: Asociar el componente de página con la nueva plantilla de formulario adaptable
description: Crear un nuevo componente de página
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 8%

---

# Asocie el componente de página con la plantilla

El siguiente paso es asociar el componente de página con la nueva plantilla de formulario adaptable. Esto garantiza que el código del componente de página se ejecute cada vez que se procese un formulario adaptable basado en la nueva plantilla. Para los fines de este tutorial, se creó una nueva plantilla de formulario adaptable llamada **StoreAndRestoreFromAzure** en la carpeta **AzurePortalStorage**.
Vaya al nodo /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content, agregue la siguiente propiedad y guarde los cambios.

| **Nombre de propiedad** | **Tipo de propiedad** | **Valor de propiedad** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Cadena | azureportalpagecomponent/component/page/storeandfetch |

Vaya al nodo /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr:content, agregue la siguiente propiedad y guarde los cambios.

| **Nombre de propiedad** | **Tipo de propiedad** | **Valor de propiedad** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Cadena | azureportalpagecomponent/component/page/storeandfetch |


## Siguientes pasos

[Crear integración con Azure Storage](./create-fdm.md)
