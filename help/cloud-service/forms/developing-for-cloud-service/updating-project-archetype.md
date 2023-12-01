---
title: Actualización del proyecto de Cloud Service con el último tipo de archivo
description: Actualización del proyecto de AEM Forms Cloud Service con el último tipo de archivo
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---

# Migración desde el arquetipo de aem antiguo

Para actualizar el proyecto de AEM Forms existente con el último arquetipo de Maven, deberá copiar manualmente el código/las configuraciones, etc., del proyecto antiguo al nuevo.

Se siguieron los siguientes pasos para migrar el proyecto creado con el arquetipo 30 a un proyecto de arquetipo 33

## Creación de un proyecto Maven con el último tipo de archivo

* Abra el símbolo del sistema y vaya a c:\cloudmanager
* Cree un proyecto Maven con el último tipo de archivo.
* Copie y pegue el contenido del [archivo de texto](assets/creating-maven-project.txt) en la ventana del símbolo del sistema. Es posible que tenga que cambiar DarchetypeVersion=33 según el [última versión](https://github.com/adobe/aem-project-archetype/releases). El tipo de archivo 33 incluye nuevas temáticas de AEM Forms.
Dado que estamos creando el nuevo proyecto de maven en la carpeta cloudmanager que ya tiene el proyecto aem-banking-application, debe cambiar la **DartifactId** de aem-banking-application a algo diferente. He utilizado aem-banking-application1 para este artículo.

>[!NOTE]
>
>Si implementa este nuevo proyecto tal como está, la instancia del servicio en la nube no tendrá HandleFormSubmission ni SubmitToAEMervlet. Esto se debe a que, cada vez que implementa un proyecto con Cloud Manager, cualquier elemento en la variable `/apps` se elimina y se sobrescribe.

## Copie el código java

Una vez creado el proyecto correctamente, puede empezar a copiar código/configuraciones, etc., del proyecto antiguo a este nuevo proyecto

* Copie el servlet HandleFormSubmission de ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
hasta
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copiar el envío personalizado de
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` del proyecto aem-banking-application1 a aem-banking-application1

* importar el nuevo proyecto en IntelliJ

* Actualice el filter.xml en el módulo ui.apps del proyecto aem-banking-application1 para incluir la siguiente línea
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Una vez que haya copiado todo el código en el nuevo proyecto, puede insertar este proyecto en Cloud Manager.

>[!NOTE]
>
>Para sincronizar el contenido (Forms AEM adaptable, modelo de datos de formulario, etc.) en el nuevo proyecto, deberá crear la estructura de carpetas adecuada en el proyecto IntelliJ y, a continuación, sincronizar el proyecto IntelliJ con la instancia de la instancia de mediante el comando Obtener de la herramienta de repositorio.
