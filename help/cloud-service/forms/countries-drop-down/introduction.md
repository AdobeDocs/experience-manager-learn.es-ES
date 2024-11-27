---
title: Creación de un componente de lista desplegable de países
description: Cree un componente de lista desplegable de países basado en un componente desplegable del núcleo de aem forms.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Cree un componente desplegable de países basado en un componente desplegable

La creación de un nuevo componente principal en Adobe Experience Manager AEM () es un proceso interesante que implica varios pasos, incluida la definición de la estructura del componente, la personalización del cuadro de diálogo y la implementación de un modelo Sling para la funcionalidad dinámica.

Al final de este tutorial, habrá dominado cómo hacer lo siguiente:

* Cree y utilice un modelo Sling para recuperar datos de forma dinámica.
* Personalice el cuadro de diálogo cq-dialog añadiendo nuevos campos y ocultando otros.
* Defina una estructura de componentes sólida y adaptada para su reutilización.

El componente, denominado &quot;Países&quot;, permite a los usuarios seleccionar un continente y rellenar dinámicamente un menú desplegable con países correspondientes al continente elegido. Esto se creará sobre la base del componente de lista desplegable predeterminado, mejorado para este caso de uso específico.

¡Vamos a sumergirnos y crear este componente dinámico y potente!

## Requisitos previos

La creación de un nuevo componente principal en Adobe Experience Manager AEM () requiere cumplir varios requisitos previos para garantizar un proceso de desarrollo sin problemas. Esto es lo que necesita antes de empezar:

* AEM Entorno de desarrollo: una instalación funcional preparada para la nube que se ejecuta localmente
* AEM Acceso a las herramientas de desarrollo de la aplicación, como Visual Studio Code o IntelliJ
* AEM Configuración y ejecución del proyecto de MAven con el último tipo de archivo
* AEM Conocimiento básico de conceptos de la
* Herramientas básicas y configuración, como repositorio Git, versión correcta de JDK


## Siguientes pasos

[Crear estructura de componentes](./component.md)
