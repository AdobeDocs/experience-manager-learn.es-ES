---
title: Primer tutorial de AEM Headless
description: Aprenda a ser la primera aplicación de AEM sin encabezado.
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 3%

---

# Primer tutorial de AEM Headless

{{aem-headless-trials-promo}}

Bienvenido al tutorial sobre la creación de una experiencia web con React, totalmente equipado con las API de AEM sin encabezado y GraphQL. En este tutorial, le guiaremos a través del proceso de creación de una aplicación web dinámica e interactiva mediante la combinación de la potencia de React, las API sin encabezado de Adobe Experience Manager (AEM) y GraphQL.

React es una popular biblioteca de JavaScript para la creación de interfaces de usuario, conocida por su simplicidad, reutilización y arquitectura basada en componentes. AEM ofrece funciones sólidas de administración de contenido y expone las API sin encabezado que permiten a los desarrolladores acceder al contenido y a los datos almacenados en AEM a través de una variedad de canales y aplicaciones.

Al aprovechar las API de AEM sin encabezado, puede recuperar contenido, recursos y datos de su instancia de AEM y utilizarlos para impulsar la aplicación React. GraphQL, un lenguaje de consulta flexible para las API, proporciona una forma eficaz y precisa de solicitar datos específicos de su instancia de AEM, lo que permite una integración perfecta entre React y AEM.

![Primer tutorial de AEM sin encabezado](./assets/overview/overview.png)

A lo largo de este tutorial, le guiaremos por el proceso paso a paso de creación de una experiencia web mediante las API de React y AEM sin encabezado con GraphQL. Aprenderá a configurar su entorno de desarrollo, establecer una conexión entre React y AEM, recuperar contenido mediante consultas de GraphQL y procesarlo dinámicamente en su aplicación web.

Cubriremos temas como la configuración del proyecto React, el establecimiento de la autenticación con AEM, la consulta de contenido desde AEM mediante GraphQL, la administración de datos en los componentes React y la optimización del rendimiento mediante el uso del almacenamiento en caché y la paginación.

Al final de este tutorial, tendrá una comprensión sólida de cómo aprovechar React, las API sin encabezado de AEM y GraphQL para crear una experiencia web potente y atractiva. ¡Así que vamos a sumergirnos y comenzar a crear su próxima aplicación web!

## Requisitos previos

### Habilidades

+ Competencia en React
+ Competencia en GraphQL
+ Conocimientos básicos de AEM as a Cloud Service

### AEM as a Cloud Service

Este tutorial requiere acceso de administrador a un entorno de AEM as a Cloud Service.

### Software

+ [Node.js v16+](https://nodejs.org/en/)
   + Compruebe la versión del nodo ejecutando `node -v` desde la línea de comandos
+ [npm 6+](https://www.npmjs.com/)
   + Compruebe su versión de npm ejecutando `npm -v` desde la línea de comandos
+ [Git](https://git-scm.com/)
   + Compruebe su versión de Git ejecutando `git -v` desde la línea de comandos

Use [administrador de versiones de nodo (nvm)](https://github.com/nvm-sh/nvm) para abordar el problema de tener varias versiones de node.js en el mismo equipo.

Asegúrese de tener privilegios para instalar software globalmente en el equipo.

## Siguiente paso

Ahora que su entorno está configurado, pasemos al siguiente paso: [Configuración y creación de contenido en AEM as a Cloud Service](./1-content-modeling.md)
