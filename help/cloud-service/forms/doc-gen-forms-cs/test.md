---
title: Probar la solución
description: Ejecute el archivo Main.java para probar la solución
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---


# Importar Proyecto Eclipse

Descargue y descomprima el [archivo zip](./assets/aem-forms-doc-gen.zip)

Iniciar Eclipse e importar el proyecto en Eclipse
El proyecto incluye los siguientes archivos en la carpeta de recursos:

* DataFile1 y DataFile2 : Archivos de datos xml de muestra que se combinarán con la plantilla para generar el archivo PDF final
* address.xdp - Plantilla XDP
* service_token.json : tendrá que reemplazar el contenido de este archivo con credenciales específicas de su cuenta
* options.json : las opciones especificadas en este archivo se utilizan para establecer las propiedades del archivo PDF generado por la API

![resources-file](./assets/resource-files.JPG)

## Probar la solución

* Copie y pegue sus credenciales de servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo DocumentGeneration.java y especifique la carpeta en la que desea guardar los archivos PDF generados
* Abra Main.java. Establezca el valor de la variable postURL para que apunte a la instancia.
* Ejecute Main.java como aplicación java

>[!NOTE]
> La primera vez que ejecute el programa java obtendrá el error HTTP 403. Para superarlo, asegúrese de dar los [permisos adecuados al usuario de cuenta técnica en AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms** Useris la función que he utilizado en este curso.

