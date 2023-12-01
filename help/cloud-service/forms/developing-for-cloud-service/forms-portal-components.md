---
title: Habilitar componentes del portal de AEM Forms
description: Creación de un portal de AEM Forms con componentes principales
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 18%

---

# Componentes del portal de Forms

AEM Forms proporciona los siguientes componentes listos para usar del portal:

**Buscar y listar**: este componente le permite mostrar una lista de los formularios del repositorio de formularios en la página del portal y proporciona opciones de configuración para mostrar una lista de los formularios en función de criterios especificados.

**Borradores y envíos**: Mientras que el componente Buscar y listar muestra los formularios que publica el autor de Forms, el componente Borradores y envíos muestra los formularios guardados como borrador para completarlos posteriormente y enviarlos. Este componente proporciona una experiencia personalizada a cualquier usuario que haya iniciado sesión.

**Vínculo**: Este componente le permite crear un vínculo a un formulario en cualquier parte de la página.

## Habilitar componentes del portal de Forms

Inicie IntelliJ y abra el proyecto BankingApplication creado en el [paso anterior.](./getting-started.md) Amplíe ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

Para utilizar cualquier componente principal (incluidos los componentes de portal predeterminados) en un sitio de Adobe Experience Manager (AEM), debe crear un componente proxy y habilitarlo para su sitio.
El componente proxy recién creado debe apuntar al componente de formularios predeterminado para que herede todo de ellos. Para ello, cambie resourceSuperType en el content.xml del componente proxy. En el archivo content.xml también especificamos el título y el grupo de componentes.
>[!NOTE]
>
> Puede construir el supertipo de recurso para cada uno de los [estos componentes desde aquí](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Borradores y envíos

Realizar una copia de un componente existente (por ejemplo, `button`) y asígnele el nombre _draftsandsubmissions_.
![draftsandsubmissions](assets/forms-portal-components2.png)
Reemplace el contenido en la variable `.content.xml` con el siguiente XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Buscar y listar

Realice una copia del componente Botón y cambie su nombre a _lista de búsqueda_.
Reemplace el contenido en la variable `.content.xml` con el siguiente XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Componente Vínculo

Realice una copia del componente Botón y cambie su nombre a _vincular_.
Reemplace el contenido en la variable `.content.xml` con el siguiente XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

AEM Una vez implementado el proyecto, debe poder utilizar estos componentes en la página de para crear el portal de Forms.

## Pasos siguientes

[Incluir configuración de servicios en la nube](./azure-storage-fdm.md)
