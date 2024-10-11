---
title: Uso del sistema de estilos en AEM Forms
description: Crear variaciones de estilo para el componente Botón
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 21%

---

# Introducción

El sistema de estilos de Adobe Experience Manager AEM () permite a los usuarios crear varias variaciones visuales de un componente y, a continuación, seleccionar qué estilo utilizar al crear un formulario. Esto hace que los componentes sean más flexibles y reutilizables, sin necesidad de crear componentes personalizados para cada estilo.

Este artículo le ayudará a crear variaciones del componente botón y a probar las variaciones en su entorno local listo para la nube antes de insertar los cambios en la instancia de la nube mediante Cloud Manager.

La captura de pantalla muestra las 2 variaciones de estilo para el componente Botón disponibles para el autor del formulario.


![variaciones de botón](assets/button-variations.png)

## Requisitos previos

* Instancia de AEM Forms preparada para la nube con componentes principales.
* Clonación de una temática: debe estar familiarizado con la clonación de una temática. Para el propósito de este tutorial hemos clonado el [tema del caballete](https://github.com/adobe/aem-forms-theme-easel). Puede clonar cualquiera de las temáticas disponibles para adaptarlas a sus necesidades.

* Instale la última versión de Apache Maven. Apache Maven es una herramienta de automatización de compilaciones que se utiliza comúnmente en proyectos Java™. La instalación de la última versión garantiza que tenga las dependencias necesarias para la personalización de temáticas.
* Instale un editor de texto sin formato. Por ejemplo, Microsoft® Visual Studio Code. Utilizar un editor de texto sin formato como Microsoft® Visual Studio Code proporciona un entorno fácil de usar para editar y modificar archivos de temas.



## Siguientes pasos

[Crear política de estilo](./style-policy.md)
