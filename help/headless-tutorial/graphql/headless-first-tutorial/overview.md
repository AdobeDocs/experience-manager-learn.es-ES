---
title: Primer tutorial de AEM sin encabezado
description: Aprenda a ser una primera aplicación AEM sin encabezado.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---


# Primer tutorial de AEM sin encabezado

![Tutorial primero AEM sin encabezado](./assets/overview/overview.png)

Le damos la bienvenida al tutorial sobre la creación de una experiencia web con React, totalmente equipado con AEM API sin encabezado y GraphQL. En este tutorial, le guiaremos a través del proceso de creación de una aplicación web dinámica e interactiva combinando la potencia de las API React, Adobe Experience Manager (AEM) sin encabezado y GraphQL.

React es una popular biblioteca JavaScript para crear interfaces de usuario, conocida por su simplicidad, reutilización y arquitectura basada en componentes. AEM proporciona sólidas capacidades de administración de contenido y expone las API sin encabezado que permiten a los desarrolladores acceder al contenido y los datos almacenados en AEM a través de una variedad de canales y aplicaciones.

Al aprovechar AEM API sin encabezado, puede recuperar contenido, recursos y datos de su instancia de AEM y utilizarlos para impulsar su aplicación React. GraphQL, un lenguaje de consulta flexible para las API, proporciona una forma eficaz y precisa de solicitar datos específicos desde la instancia de AEM, lo que permite una integración perfecta entre React y AEM.

A lo largo de este tutorial, le guiaremos por el proceso paso a paso de crear una experiencia web con las API React y AEM sin encabezado con GraphQL. Aprenderá a configurar su entorno de desarrollo, establecer una conexión entre React y AEM, recuperar contenido mediante consultas de GraphQL y procesarlo dinámicamente en la aplicación web.

Abarcaremos temas como la configuración de su proyecto React, el establecimiento de autenticación con AEM, la consulta de contenido desde AEM usando GraphQL, el manejo de datos en sus componentes React y la optimización del rendimiento mediante el uso del almacenamiento en caché y la paginación.

Al final de este tutorial, tendrá una comprensión sólida de cómo aprovechar React, AEM API sin encabezado y GraphQL para crear una experiencia web potente y atractiva. Así que, vamos a sumergirnos y empezar a construir su próxima aplicación web.

## Requisitos previos

### Habilidades

+ Competencia en React
+ Competencia en GraphQL
+ Conocimientos básicos de AEM as a Cloud Service

### AEM as a Cloud Service

Este tutorial requiere acceso de administrador a un entorno as a Cloud Service AEM.

### Software

+ [Node.js v16+](https://nodejs.org/en/)
   + Compruebe la versión del nodo ejecutando `node -v` desde la línea de comandos
+ [npm 6+](https://www.npmjs.com/)
   + Compruebe su versión de npm ejecutando `npm -v` desde la línea de comandos
+ [Git](https://git-scm.com/)
   + Compruebe la versión de Git ejecutando `git -v` desde la línea de comandos

Uso [administrador de versiones de nodos (nvm)](https://github.com/nvm-sh/nvm) para tratar de tener varias versiones de node.js en el mismo equipo.

Asegúrese de que tiene privilegios para instalar software globalmente en su equipo.

## Siguiente paso

Ahora que su entorno está configurado, vamos al siguiente paso: [Configuración y creación de contenido en AEM as a Cloud Service](./1-content-modeling.md)
