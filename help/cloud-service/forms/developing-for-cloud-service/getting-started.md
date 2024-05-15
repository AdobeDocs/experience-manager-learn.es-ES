---
title: Instalación de los requisitos previos
description: Instale el software necesario para configurar su entorno de desarrollo
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
duration: 44
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 1%

---

# Instalación del software necesario

Este tutorial le guiará a través de los pasos necesarios para crear un proyecto de AEM Forms y sincronizar el proyecto de AEM Forms AEM con la instancia local mediante la herramienta IntelliJ y la herramienta de repositorio. También aprenderá a añadir su proyecto al repositorio de Git local e insertar el repositorio de Git local en el repositorio de Cloud Manager.





Este tutorial hará referencia a esta estructura de carpetas a partir de ahora.

* [Instalación de JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). He descargado jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Por ejemplo, si ha instalado Maven en la carpeta c:\maven, deberá crear una variable de entorno llamada M2_HOME con el valor C:\maven\apache-maven-3.6.0. A continuación, agregue M2_HOME\bin a la ruta y guarde la configuración.

## AEM Creación de un proyecto de Maven mediante el tipo de archivo del proyecto de

* Cree una carpeta llamada **cloudmanager**(puede darle cualquier nombre) en su unidad c
* Abra la ventana del símbolo del sistema y vaya a **c:\cloudmanager**
* Copie y pegue el contenido del [archivo de texto](assets/creating-maven-project.txt) en la ventana del símbolo del sistema. Es posible que tenga que cambiar DarchetypeVersion=30 según la variable [última versión](https://github.com/adobe/aem-project-archetype/releases). La última versión tenía 30 años en el momento de escribir este artículo.
* Ejecute el comando pulsando la tecla Intro. Si todo funciona correctamente, debería ver el mensaje de éxito de la versión.

## Siguientes pasos

[Instalación de IntelliJ](./intellij-set-up.md)