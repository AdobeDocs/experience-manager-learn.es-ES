---
title: Reaccionar la edición de aplicaciones con el editor universal
description: Obtenga información sobre cómo editar el contenido de una aplicación React de ejemplo mediante el Editor universal.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Reaccionar la edición de aplicaciones con el editor universal

Obtenga información sobre cómo editar el contenido de una aplicación React de ejemplo mediante el Editor universal. AEM El contenido se almacena en Fragmentos de contenido en la y se recupera mediante las API de GraphQL.

Este tutorial le guía a través del proceso de configuración del entorno de desarrollo local, instrumentación de la aplicación React para editar su contenido y edición del contenido mediante el Editor universal.

## Lo que aprende

Este tutorial abarca los siguientes temas:

- Breve descripción general del editor universal
- Configuración del entorno de desarrollo local
   - AEM **SDK de**: proporciona el contenido almacenado dentro de los fragmentos de contenido para la aplicación React mediante las API de GraphQL.
   - AEM **Aplicación React**: una sencilla interfaz de usuario que muestra el contenido de los recursos de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de.
   - AEM **Servicio de editor universal**: una _copia local del servicio de editor universal_ que enlaza el editor universal y el SDK de la.
- Cómo instrumentar la aplicación React para editar el contenido mediante el editor universal
- Cómo editar el contenido de la aplicación React mediante el Editor universal


## Descripción general del editor universal

El editor universal ofrece a los autores y desarrolladores de contenido las herramientas necesarias (front-end y back-end). Revisemos algunas de las ventajas clave del editor universal:

- Creado para editar contenido sin encabezado y con encabezado en el modo lo que se ve es lo que se obtiene (WYSIWYG).
- Proporciona una experiencia de edición de contenido coherente en diferentes tecnologías front-end como React, Angular, Vue, etc. Por lo tanto, los autores de contenido pueden editar el contenido sin preocuparse por la tecnología front-end subyacente.
- Se requiere una instrumentación mínima para habilitar el editor universal en la aplicación front-end. De este modo, se maximiza la productividad del desarrollador y se libera a este para que se centre en la creación de la experiencia.
- Separación de preocupaciones en tres funciones, autores de contenido, desarrolladores de front-end y desarrolladores de back-end, lo que permite que cada función se centre en sus responsabilidades principales.


## Aplicación React de muestra

Este tutorial utiliza [**Equipos WKND**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) como aplicación React de ejemplo. La aplicación **WKND Teams** React muestra una lista de miembros del equipo y sus detalles.

AEM Los detalles del equipo, como el título, la descripción y los integrantes del equipo, se almacenan como _Fragmentos de contenido del equipo_ en la. AEM Del mismo modo, los detalles de la persona, como el nombre, la biografía y la imagen del perfil, se almacenarán como _Fragmentos de contenido de persona_ en el sitio de trabajo de.

AEM El contenido de la aplicación React lo proporciona el usuario mediante las API de GraphQL y la interfaz de usuario se ha creado con dos componentes de React, `Teams` y `Person`.

Hay disponible un tutorial correspondiente para aprender a crear la aplicación React de **WKND Teams**. Puede encontrar el tutorial [aquí](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Siguiente paso

Aprenda a [configurar el entorno de desarrollo local](./local-development-setup.md).
