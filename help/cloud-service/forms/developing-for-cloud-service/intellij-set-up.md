---
title: Instalación de la edición de la comunidad IntelliJ
description: Instalar e importar el proyecto de AEM en IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# Instalación de IntelliJ

Instalar [Edición comunitaria de IntelliJ](https://www.jetbrains.com/idea/download/#section=windows). Puede aceptar la configuración predeterminada mientras se sugiere durante la instalación.

## Importar el proyecto AEM

* Iniciar IntelliJ
* Importe el proyecto de AEM que ha creado en el paso anterior. Después de importar el proyecto, la pantalla debería tener este aspecto ![aem-banking-app](assets/aem-banking-app.png). Normalmente, trabajará con subproyectos core,ui.apps,ui.config y ui.content.
* Si no ve la ventana maven y terminal, vaya a ver->Ventana Herramientas y seleccione Maven y Terminal

## Añadir el módulo de fuentes

Si desea utilizar fuentes personalizadas en el archivo PDF, deberá insertar las fuentes personalizadas en la instancia de AEM Forms CS. Siga estos pasos

* Cree una carpeta llamada **fuentes** en C:\CloudManager\aem-banking-application
* Extraer el contenido de [font.zip](assets/fonts.zip) en la carpeta de fuentes recién creada
* En el módulo de fuentes se incluyen algunas fuentes personalizadas.Puede agregar las fuentes personalizadas de su organización a C:\CloudManager\aem-banking-application\fonts\src\main\resources folder of the fonts module
* Abra el archivo C:\CloudManager\aem-banking-application\pom.xml
* Añada la siguiente línea  ```<module>fonts</module>``` en la sección de módulos de pom.xml
* Guarde el pom.xml
* Actualizar el proyecto de la aplicación aem-banking-application en IntelliJ

Estructura del proyecto con módulo de fuentes
![fonts-module](assets/fonts-module.png)

Módulo Fuentes incluidas en el POM de proyectos
![fonts-pom](assets/fonts-module-pom.png)
