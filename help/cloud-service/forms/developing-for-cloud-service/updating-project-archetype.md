---
title: Actualización del proyecto de Cloud Service con el último tipo de archivo
description: Actualización del proyecto de servicio en la nube de AEM Forms con el último tipo de archivo
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9534
source-git-commit: cea9a9dc003b76369db1b7fedb9549062885258d
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 1%

---

# Migración desde arquetipo antiguo de aem

Para actualizar el proyecto de AEM Forms existente con el último tipo de archivo maven, deberá copiar manualmente el código/configuraciones, etc., del proyecto antiguo al nuevo proyecto.

Se siguieron los siguientes pasos para migrar el proyecto creado con el tipo de archivo 30 al proyecto de arquetipo 33

## Crear un proyecto maven con el último tipo de archivo

* Abra el símbolo del sistema y vaya a c:\cloudmanager
* Cree un proyecto maven con el arquetipo más reciente.
* Copie y pegue el contenido del [archivo de texto](assets/creating-maven-project.txt) en la ventana del símbolo del sistema. Puede que tenga que cambiar el tipo de archivoVersión=33 según la variable [última versión](https://github.com/adobe/aem-project-archetype/releases). El tipo de archivo 33 incluye nuevos temas de AEM Forms.
Dado que estamos creando el nuevo proyecto maven en la carpeta cloudmanager que ya tiene el proyecto aem-banking-application, debe cambiar la **IdDartifact** desde aem-banking-application a algo diferente. He utilizado aem-banking-application1 para este artículo.

>[!NOTE]
>
>Si implementa este nuevo proyecto tal cual es, la instancia del servicio en la nube no tendrá HandleFormSubmission ni SubmitToAEMServlet. Esto se debe a que cada vez que implemente un proyecto mediante el administrador de la nube, se eliminará y sobrescribirá cualquier elemento que se encuentre en la carpeta de aplicaciones.

## Copiar el código java

Una vez que el proyecto se haya creado correctamente, puede empezar a copiar código/configuraciones, etc., del proyecto antiguo a este nuevo proyecto

* Copiar el servlet HandleFormSubmission desde ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
a

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copiar el elemento PersonalizarEnviar de
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` desde aem-banking-application al proyecto aem-banking-application1

* importar el nuevo proyecto en IntelliJ

* Actualice filter.xml en el módulo ui.apps del proyecto aem-banking-application1 para incluir la siguiente línea
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Una vez copiado todo el código en el nuevo proyecto, puede insertar este proyecto en cloud manager.

>[!NOTE]
>
>Para sincronizar el contenido (Forms adaptable, Modelo de datos de formulario, etc.) con el nuevo proyecto, deberá crear la estructura de carpetas adecuada en el proyecto IntelliJ y, a continuación, sincronizar el proyecto IntelliJ con la instancia de AEM mediante el comando Get de la herramienta repo.
