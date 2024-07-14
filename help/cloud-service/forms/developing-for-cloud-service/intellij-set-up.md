---
title: Instalar IntelliJ Community Edition
description: AEM Instalar e importar el proyecto de la en IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
duration: 43
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---

# Instalación de IntelliJ

Instalar [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/#section=windows). Puede aceptar la configuración predeterminada mientras se sugiere durante la instalación.

## AEM Importación del proyecto de

* Iniciar IntelliJ
* AEM Importe el proyecto de la que creó en el paso anterior. Una vez importado el proyecto, la pantalla debería ser similar a esta ![aem-banking-app](assets/aem-banking-app.png). Normalmente trabajará con subproyectos core, ui.apps, ui.config y ui.content.
* Si no ve la ventana de Maven y la ventana de terminal, vaya a ver->Ventana de herramientas y seleccione Maven y Terminal

## Añadir el módulo de fuentes

Si desea utilizar fuentes personalizadas en el archivo de PDF, deberá insertar las fuentes personalizadas en la instancia de AEM Forms CS. Siga estos pasos

* Cree una carpeta llamada **fonts** en C:\CloudManager\aem-banking-application
* Extraiga el contenido de [font.zip](assets/fonts.zip) en la carpeta de fuentes recién creada
* El módulo de fuentes incluye algunas fuentes personalizadas. Puede agregar las fuentes personalizadas de su organización a la carpeta C:\CloudManager\aem-banking-application\fonts\src\main\resources del módulo de fuentes
* Abra el archivo C:\CloudManager\aem-banking-application\pom.xml
* Agregue la línea siguiente ```<module>fonts</module>``` en la sección de módulos del archivo pom.xml
* Guarde el archivo pom.xml.
* Actualice el proyecto aem-banking-application en IntelliJ

Estructura del proyecto con módulo de fuentes
![módulo de fuentes](assets/fonts-module.png)

Módulo de fuentes incluido en el POM de proyectos
![fuentes-pom](assets/fonts-module-pom.png)

## Siguientes pasos

[Configurar Git](./setup-git.md)
