---
title: Prueba de la solución de ensamblador de Forms
description: Ejecute ExecuteAssemblerService.java para probar la solución
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 55
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Importar proyecto de Eclipse

* Descargar y descomprimir el [archivo zip](./assets/pdf-manipulation.zip)
* Inicie Eclipse e importe el proyecto en Eclipse
* El proyecto incluye las siguientes carpetas en la carpeta de recursos:
   * ddxFiles: esta carpeta contiene el archivo ddx para describir la salida que desea generar
   * archivos PDF: esta carpeta contiene los archivos PDF que desea ensamblar y los archivos PDF para probar las utilidades PDFA
   * credenciales: esta carpeta contiene el archivo pdfa-options.json

![archivo de recursos](./assets/resources.png)

## Probar archivos de PDF de ensamblaje

* Copie y pegue las credenciales del servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo AssemblePDFiles.java y especifique la carpeta en la que desea guardar los archivos de PDF generados
* Abra ExecuteAssemblerService.java. AEM Establezca el valor de la variable __FORMS_CS_ para que apunte a su instancia.
* Elimine los comentarios de las líneas adecuadas para probar la combinación de dos o más archivos de PDF
* Ejecute ExecuteAssemblerService.java como aplicación java

### Probar utilidades de PDFA

* Copie y pegue las credenciales del servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo PDFAUtilities.java y especifique la carpeta en la que desea guardar los archivos de PDF generados.
* Abra ExecuteAssemblerService.java. AEM Establezca el valor de la variable __FORMS_CS_ para que apunte a su instancia.
* Elimine los comentarios de las líneas adecuadas para probar las operaciones de PDFA.
* Ejecute ExecuteAssemblerService.java como aplicación java.



>[!NOTE]
> La primera vez que ejecute el programa java obtendrá el error HTTP 403. AEM Para superar esto, asegúrese de dar los [permisos apropiados al usuario de la cuenta técnica en el usuario de la cuenta técnica en la que se encuentra el usuario de la cuenta técnica en la que se encuentra el usuario de la cuenta de](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=es#configuraci%C3%B3n-del-acceso-en-aem).

**Usuarios de AEM Forms** es la función que utilicé para este curso.
