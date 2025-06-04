---
title: Primer tutorial de AEM sin encabezado
description: Aprenda a crear la primera aplicación de AEM sin encabezado.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 89
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '421'
ht-degree: 100%

---

# Primer tutorial de AEM sin encabezado

Le damos la bienvenida al tutorial sobre cómo crear una experiencia web mediante React, con tecnología de las API de AEM sin encabezado y GraphQL. En este tutorial, le guiaremos a través del proceso de creación de una aplicación web dinámica e interactiva combinando la potencia de React, las API sin encabezado de Adobe Experience Manager (AEM) y GraphQL.

React es una popular biblioteca de JavaScript para crear interfaces de usuario, conocida por su simplicidad, su posibilidad de reutilización y una arquitectura basada en componentes. AEM ofrece funciones sólidas de administración de contenido y expone las API sin encabezado que permiten a los desarrolladores acceder al contenido y a los datos almacenados en AEM a través de una variedad de canales y aplicaciones.

Al aprovechar las API de AEM sin encabezado, puede recuperar contenido, recursos y datos de su instancia de AEM y utilizarlos para potenciar la aplicación React. GraphQL, un lenguaje de consulta flexible para las API, proporciona una forma eficaz y precisa de solicitar datos específicos de su instancia de AEM, lo que permite una integración perfecta entre React y AEM.

![Primer tutorial sobre AEM sin encabezado](./assets/overview/overview.png)

A lo largo de este tutorial, le guiaremos por el proceso de creación, paso a paso, de una experiencia web mediante las API de React y AEM sin encabezado con GraphQL. Aprenderá a configurar su entorno de desarrollo, establecer una conexión entre React y AEM, recuperar contenido mediante consultas de GraphQL y procesarlo dinámicamente en su aplicación web.

Cubriremos temas como la configuración del proyecto React, el establecimiento de la autenticación con AEM, la consulta de contenido desde AEM mediante GraphQL, el tratamiento de datos en los componentes de React y la optimización del rendimiento usando el almacenamiento en caché y la paginación.

Al final de este tutorial, comprenderá perfectamente cómo aprovechar React, las API sin encabezado de AEM y GraphQL para crear una experiencia web potente y atractiva. Así que manos a la obra y empiece a crear su próxima aplicación web.

## Requisitos previos

### Habilidades

+ Conocimientos avanzados de React
+ Conocimientos avanzados de GraphQL
+ Conocimientos básicos de AEM as a Cloud Service

### AEM as a Cloud Service

Este tutorial requiere acceso de administrador a un entorno de AEM as a Cloud Service.

### Software

+ [Node.js versión 16+](https://nodejs.org/es/)
   + Compruebe su versión del nodo ejecutando `node -v` desde la línea de comandos
+ [npm 6+](https://www.npmjs.com/)
   + Compruebe su versión de npm ejecutando `npm -v` desde la línea de comandos
+ [Git](https://git-scm.com/)
   + Compruebe su versión de Git ejecutando `git -v` desde la línea de comandos

Use el [gestor de versiones de nodo (nvm)](https://github.com/nvm-sh/nvm) para solucionar el problema de tener varias versiones de node.js en el mismo equipo.

Asegúrese de que tiene privilegios para instalar software globalmente en su equipo.

## Siguiente paso

Ahora que su entorno está configurado, pasemos al siguiente paso: [Configuración y creación de contenido en AEM as a Cloud Service](./1-content-modeling.md)
