---
title: Extensibilidad de la IU de AEM
description: Obtenga información acerca de la extensibilidad de la IU de AEM mediante App Builder para crear extensiones.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 50
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 15%

---

# Extensibilidad de la IU de AEM {#aem-ui-extensibility}

Adobe Experience Manager (AEM) ofrece una potente interfaz de usuario (IU) para crear experiencias digitales. Para personalizar y ampliar la interfaz de usuario, Adobe ha introducido App Builder. Esta herramienta permite a los desarrolladores mejorar la experiencia del usuario sin codificaciones complejas mediante JavaScript y React.

App Builder proporciona una capa de implementación para crear extensiones enlazadas a puntos de extensión bien definidos en AEM. App Builder se integra perfectamente con AEM, lo que permite realizar pruebas y previsualizaciones en tiempo real. La implementación de cambios en AEM es rápida y sencilla. Al utilizar App Builder, los desarrolladores ahorran tiempo y esfuerzo, lo que permite crear prototipos rápidamente y colaborar con las partes interesadas.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_extensibility_app_builder"
>title="Introducción a Adobe Developer App Builder y AEM sin encabezado"
>abstract="Descubra cómo AEM App Builder permite a los desarrolladores personalizar y ampliar rápidamente las interfaces de usuario de AEM con JavaScript y React, lo que permite una integración perfecta y una implementación rápida."

## Desarrollo de una extensión de IU de AEM

Las distintas IU de AEM tienen diferentes puntos de extensión, aunque los conceptos básicos son los mismos.

Los vídeos y las visitas guiadas que se proporcionan vinculados a continuación muestran el uso de una extensión de la consola Fragmento de contenido para ilustrar varias actividades. Sin embargo, es importante tener en cuenta que los conceptos mencionados se pueden aplicar a todas las extensiones de la interfaz de usuario de AEM.

1. [Creación de un proyecto de Adobe Developer Console](./adobe-developer-console-project.md)
1. [Inicialización de una extensión](./app-initialization.md)
1. [Registro de una extensión](./extension-registration.md)
1. Implementación de un punto de extensión

   Los puntos de extensión y sus implementaciones varían en función de la interfaz de usuario que se amplía.

   + [Desarrollo de una extensión de IU de fragmentos de contenido](./content-fragments/overview.md)

1. [Desarrollar un modal](./modal.md)
1. [Desarrollar una acción de Adobe I/O Runtime](./runtime-action.md)
1. [Verificar una extensión](./verify.md)
1. [Implementación de una extensión](./deploy.md)

## Documentación de Adobe Developer

Adobe Developer contiene detalles del desarrollador sobre la extensibilidad de la IU de AEM. Revise el [contenido de Adobe Developer para obtener más información técnica](https://developer.adobe.com/uix/docs/).
