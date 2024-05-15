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

* Descargue y descomprima el [archivo zip](./assets/pdf-manipulation.zip)
* Inicie Eclipse e importe el proyecto en Eclipse
* El proyecto incluye las siguientes carpetas en la carpeta de recursos:
   * ddxFiles: esta carpeta contiene el archivo ddx para describir la salida que desea generar
   * archivos PDF: esta carpeta contiene los archivos PDF que desea ensamblar y los archivos PDF para probar las utilidades PDFA
   * credenciales: esta carpeta contiene el archivo pdfa-options.json

![resources-file](./assets/resources.png)

## Probar archivos de PDF de ensamblaje

* Copie y pegue las credenciales del servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo AssemblePDFiles.java y especifique la carpeta en la que desea guardar los archivos de PDF generados
* Abra ExecuteAssemblerService.java. Establecer el valor de la variable _AEM_FORMS_CS_ para apuntar a su instancia.
* Elimine los comentarios de las líneas adecuadas para probar la combinación de dos o más archivos de PDF
* Ejecute ExecuteAssemblerService.java como aplicación java

### Probar utilidades de PDFA

* Copie y pegue las credenciales del servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo PDFAUtilities.java y especifique la carpeta en la que desea guardar los archivos de PDF generados.
* Abra ExecuteAssemblerService.java. Establecer el valor de la variable _AEM_FORMS_CS_ para apuntar a su instancia.
* Elimine los comentarios de las líneas adecuadas para probar las operaciones de PDFA.
* Ejecute ExecuteAssemblerService.java como aplicación java.



>[!NOTE]
> La primera vez que ejecute el programa java obtendrá el error HTTP 403. Para superar esto, asegúrese de dar la [AEM los permisos adecuados para el usuario de la cuenta técnica en la aplicación de la cuenta de](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=es#configuraci%C3%B3n-del-acceso-en-aem).

**Usuarios de AEM Forms** Esta es la función que he utilizado en este curso.
