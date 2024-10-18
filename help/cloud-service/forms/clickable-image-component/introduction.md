---
title: Crear un componente de imagen en el que se puede hacer clic
description: Cree componentes de imagen en los que se puede hacer clic en AEM Forms as a Cloud Service.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Introducción a las imágenes en las que se puede hacer clic

El uso de imágenes en las que se puede hacer clic en Forms puede crear una experiencia de usuario más atractiva, intuitiva y visual. Para el propósito de este artículo, utilizamos SVG para imágenes en las que se puede hacer clic, ya que ofrece varias ventajas, particularmente en términos de flexibilidad de diseño, rendimiento y experiencia del usuario.
El SVG se puede crear con Adobe Illustrator o cualquiera de las herramientas en línea gratuitas. He usado el [mapa de EE. UU. de](https://simplemaps.com/resources/svg-us)simplemaps para mostrar el caso de uso.

## Caso de uso para utilizar el mapa de EE. UU. activo

El mapa en el que se puede hacer clic de EE. UU. permite a los usuarios explorar los envíos de formularios específicos del estado. Cuando un usuario hace clic en un estado, se enumeran los envíos de ese estado, con la opción de abrir un envío específico.
