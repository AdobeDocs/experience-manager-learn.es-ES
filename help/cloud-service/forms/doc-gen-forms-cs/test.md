---
title: Prueba de la solución
description: Ejecute Main.java para probar la solución
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
source-git-commit: 47d36e472719049de1346c5f0bba010c9af4e039
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 4%

---

# Importar proyecto de Eclipse

Descargue y descomprima el [archivo zip](./assets/aem-forms-cs-doc-gen.zip)

Iniciar Eclipse e importar el proyecto en Eclipse El proyecto incluye los siguientes archivos en la carpeta de recursos:

* DataFile1,DataFile2 y DataFile3: archivos de datos xml de ejemplo que se combinarán con la plantilla para generar el archivo de PDF final
* custom_fonts.xdp: plantilla XDP.
* service_token.json: tendrá que reemplazar el contenido de este archivo con las credenciales específicas de su cuenta
* options.json: las opciones especificadas en este archivo se utilizan para establecer las propiedades del archivo de PDF generado por la API

![resources-file](./assets/resource-files.png)

## Prueba de la solución

* Copie y pegue las credenciales del servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo DocumentGeneration.java y especifique la carpeta en la que desea guardar los archivos de PDF generados
* Abra Main.java. Establezca el valor de la variable postURL para que apunte a la instancia.
* Ejecute Main.java como aplicación java

>[!NOTE]
> La primera vez que ejecute el programa java obtendrá el error HTTP 403. Para superar esto, asegúrese de dar la [AEM los permisos adecuados para el usuario de la cuenta técnica en la aplicación de la cuenta de](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=es#configuraci%C3%B3n-del-acceso-en-aem).

**Usuarios de AEM Forms** Esta es la función que he utilizado en este curso.
