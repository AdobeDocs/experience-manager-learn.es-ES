---
title: Impulso de la configuración de los servicios de nube y del modelo de datos de formulario a la instancia de nube
description: Cree y inserte un formulario adaptable basado en el modelo de datos de formulario de almacenamiento de Azure en la instancia de nube.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Incluya la configuración de servicios de nube en su proyecto

Cree un contenedor de configuración llamado &quot;FormTutorial&quot; para guardar la configuración de los servicios de nube Cree una configuración de servicios de nube para Azure Storage denominada &quot;FormsCSAndAzureBlob&quot; en el contenedor &quot;FormTutorial&quot;. Para ello, proporcione los detalles de la cuenta de almacenamiento de Azure y la clave de acceso de Azure.

Abra el proyecto AEM en IntelliJ. Asegúrese de añadir la carpeta FormTutorial como se muestra a continuación en el proyecto ui.content
![cloud-services-configuration](assets/cloud-services-configuration.png)

Asegúrese de añadir la siguiente entrada en el filter.xml del proyecto ui.content

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Incluir el modelo de datos de formulario en el proyecto

Cree un modelo de datos de formulario basado en la configuración de los servicios de nube que creó en el paso anterior. Para incluir el modelo de datos de formulario en el proyecto, cree la estructura de carpetas adecuada en el proyecto de AEM en intelliJ. Por ejemplo, mi modelo de datos de formulario está en una carpeta llamada registros
![fdm-content](assets/ui-content-fdm.png)

Incluya la entrada adecuada en el filter.xml del proyecto ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Ahora, al crear e implementar el proyecto mediante el administrador de nube, tendrá que volver a introducir la clave de acceso de Azure en la configuración de los servicios de nube. Para evitar volver a introducir la clave de acceso, se recomienda crear una configuración según el contexto utilizando las variables de entorno como se explica en la sección [artículo siguiente](./context-aware-fdm.md)
