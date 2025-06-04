---
title: Edición de la aplicación React con el Editor universal
description: Obtenga información sobre cómo editar el contenido de una aplicación React de ejemplo mediante el Editor universal.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '415'
ht-degree: 100%

---

# Edición de la aplicación React con el Editor universal

Obtenga información sobre cómo editar el contenido de una aplicación React de ejemplo mediante el Editor universal. El contenido se almacena en fragmentos de contenido en AEM y se recupera mediante las API de GraphQL.

Este tutorial le guía a través del proceso de configuración del entorno de desarrollo local, instrumentación de la aplicación React para editar su contenido y edición del contenido mediante el Editor universal.

## Qué aprenderá

Este tutorial abarca los siguientes temas:

- Breve descripción general del Editor universal
- Configuración del entorno de desarrollo local
   - **AEM SDK**: proporciona el contenido almacenado dentro de los fragmentos de contenido para la aplicación React mediante las API de GraphQL.
   - **Aplicación React**: una interfaz de usuario sencilla que muestra el contenido de AEM.
   - **Servicio de Editor universal**: una _copia local del servicio de Editor universal_ que enlaza el Editor universal con el SDK de AEM.
- Cómo instrumentar la aplicación React para editar el contenido mediante el Editor universal
- Cómo editar el contenido de la aplicación React mediante el Editor universal


## Descripción general del Editor universal

El Editor universal ofrece a los autores y desarrolladores de contenido las herramientas necesarias (front-end y back-end). Revisemos algunas de las ventajas clave del Editor universal:

- Creado para editar contenido sin encabezado y con encabezado en el modo lo que se ve es lo que se obtiene (WYSIWYG).
- Proporciona una experiencia de edición de contenido coherente en diferentes tecnologías front-end como React, Angular, Vue, etc. Por lo tanto, los autores de contenido pueden editar el contenido sin preocuparse por la tecnología front-end subyacente.
- Se requiere una instrumentación mínima para habilitar el Editor universal en la aplicación front-end. De este modo, se maximiza la productividad del desarrollador y se libera a este para que se centre en la creación de la experiencia.
- Separación de preocupaciones en tres funciones, autores de contenido, desarrolladores de front-end y desarrolladores de back-end, lo que permite que cada función se centre en sus responsabilidades principales.


## Aplicación React de muestra

Este tutorial utiliza [**Equipos WKND**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) como aplicación React de ejemplo. La aplicación **WKND Teams** React muestra la lista de los integrantes del equipo y sus datos.

Los datos del equipo, como el título, la descripción y los integrantes del equipo, se almacenan como Fragmentos de contenido del _equipo_ en AEM. Del mismo modo, los detalles de la persona, como el nombre, la biografía y la imagen del perfil, se almacenan como Fragmentos de contenido de _persona_ en AEM.

AEM proporciona el contenido para la aplicación React mediante las API de GraphQL y la interfaz de usuario se ha creado mediante dos componentes React, `Teams` y `Person`.

Hay disponible un tutorial correspondiente para aprender a crear la aplicación **WKND Teams** React. Puede encontrar el tutorial [aquí](https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Siguiente paso

Aprenda a [configurar el entorno de desarrollo local](./local-development-setup.md).
