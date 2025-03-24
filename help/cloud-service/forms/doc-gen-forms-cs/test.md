---
title: Prueba de la solución
description: Ejecute Main.java para probar la solución
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 43
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Importar proyecto de Eclipse

Descargar y descomprimir el [archivo zip](./assets/aem-forms-cs-doc-gen.zip)

Inicie Eclipse e importe el proyecto en Eclipse
El proyecto incluye los siguientes archivos en la carpeta de recursos:

* DataFile1,DataFile2 y DataFile3: archivos de datos xml de muestra que se combinarán con la plantilla para generar el archivo PDF final
* custom_fonts.xdp: plantilla XDP.
* service_token.json: tendrá que reemplazar el contenido de este archivo con las credenciales específicas de su cuenta
* options.json: las opciones especificadas en este archivo se utilizan para establecer las propiedades del archivo PDF generado por la API

![archivo de recursos](./assets/resource-files.png)

## Prueba de la solución

* Copie y pegue las credenciales del servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo DocumentGeneration.java y especifique la carpeta en la que desea guardar los archivos PDF generados
* Abra Main.java. Establezca el valor de la variable postURL para que apunte a la instancia.
* Ejecute Main.java como aplicación java

>[!NOTE]
> La primera vez que ejecute el programa java obtendrá el error HTTP 403. Para superar esto, asegúrese de dar los [permisos apropiados al usuario de la cuenta técnica en AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=es#configuraci%C3%B3n-del-acceso-en-aem).

**Usuarios de AEM Forms** es la función que utilicé para este curso.
