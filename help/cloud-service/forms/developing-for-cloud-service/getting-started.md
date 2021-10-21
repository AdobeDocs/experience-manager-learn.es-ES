---
title: Instalación de los requisitos previos
description: Instale el software necesario para configurar su entorno de desarrollo
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8842
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 2%

---


# Instalación del software requerido

Este tutorial le guiará a través de los pasos necesarios para crear un proyecto de AEM Forms, sincronizar el proyecto de AEM Forms con la instancia de AEM local mediante IntelliJ y la herramienta repo. También aprenderá a añadir su proyecto al repositorio de Git local e insertar el repositorio de Git local en el repositorio de Cloud Manager.




Este tutorial hace referencia a esta estructura de carpetas a partir de ahora.

* [Instalación de JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). He descargado jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Por ejemplo, si ha instalado Maven en c:\maven folder, deberá crear una variable de entorno denominada M2_HOME con el valor C:\maven\apache-maven-3.6.0. A continuación, agregue M2_HOME\bin a la ruta y guarde la configuración.

## Crear un proyecto Maven con AEM tipo de archivo del proyecto

* Cree una carpeta llamada **cloudmanager**(puede darle cualquier nombre) en su unidad c
* Abra la ventana del símbolo del sistema y vaya a **c:\cloudmanager**
* Copie y pegue el contenido del [archivo de texto](assets/creating-maven-project.txt) en la ventana del símbolo del sistema. Puede que tenga que cambiar el tipo de archivoVersión=30 según la variable [última versión](https://github.com/adobe/aem-project-archetype/releases). La última versión era 30 en el momento de escribir este artículo.
* Ejecute el comando pulsando la tecla Intro. Si todo va correctamente, debería ver el mensaje de éxito de la compilación.





