---
title: Inserción de la configuración de servicios en la nube y el modelo de datos de formulario en la instancia de nube
description: Cree y envíe un formulario adaptable basado en el modelo de datos de formulario de Azure Storage a la instancia de la nube de.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
duration: 51
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Incluir la configuración de los servicios en la nube en el proyecto

Cree un contenedor de configuración denominado &quot;FormTutorial&quot; para la configuración de los servicios en la nube Cree una configuración de servicios en la nube para el almacenamiento de Azure denominada &quot;FormsCSAndAzureBlob&quot; en el contenedor &quot;FormTutorial&quot; proporcionando los detalles de la cuenta de almacenamiento de Azure y la clave de acceso de Azure.

AEM Abra el proyecto de la en IntelliJ. Asegúrese de añadir la carpeta FormTutorial como se muestra a continuación en el proyecto ui.content
![cloud-services-configuration](assets/cloud-services-configuration.png)

Asegúrese de añadir la siguiente entrada en el archivo filter.xml del proyecto ui.content

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Incluir el modelo de datos de formulario en el proyecto

Cree un modelo de datos de formulario basado en la configuración de servicios en la nube que creó en el paso anterior. AEM Para incluir el modelo de datos de formulario en el proyecto, cree la estructura de carpetas adecuada en el proyecto de en intelliJ. Por ejemplo, mi modelo de datos de formulario está en una carpeta llamada Registros
![fdm-content](assets/ui-content-fdm.png)

Incluya la entrada adecuada en el archivo filter.xml del proyecto ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Ahora, cuando genere e implemente su proyecto mediante Cloud Manager, tendrá que volver a introducir su clave de acceso de Azure en la configuración de los servicios en la nube. Para evitar volver a introducir la clave de acceso, se recomienda crear una configuración según el contexto utilizando las variables de entorno como se explica en la sección [artículo siguiente](./context-aware-fdm.md)

## Pasos siguientes

[Crear configuración según el contexto](./context-aware-fdm.md)
