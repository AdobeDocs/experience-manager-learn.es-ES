---
title: Probar la solución del ensamblador de Forms
description: Ejecute ExecuteAssemblerService.java para probar la solución
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Importar Proyecto Eclipse

* Descargue y descomprima el [archivo zip](./assets/pdf-manipulation.zip)
* Iniciar Eclipse e importar el proyecto en Eclipse
* El proyecto incluye las siguientes carpetas en la carpeta de recursos:
   * ddxFiles: esta carpeta contiene el archivo ddx para describir la salida que desea generar
   * pdffiles : Esta carpeta contiene los archivos pdf que desea ensamblar y los archivos pdf para probar las utilidades PDFA
   * credenciales: esta carpeta contiene el archivo pdfa-options.json

![resources-file](./assets/resources.png)

## Probar archivos de PDF de ensamblaje

* Copie y pegue sus credenciales de servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo AssemblePDFFiles.java y especifique la carpeta en la que desea guardar los archivos PDF generados
* Abra ExecuteAssemblerService.java. Establezca el valor de la variable _AEM_FORMS_CS_ para que apunte a la instancia.
* Descomente las líneas adecuadas para probar el ensamblaje de dos o más archivos PDF
* Ejecute ExecuteAssemblerService.java como aplicación java

### Probar utilidades PDFA

* Copie y pegue sus credenciales de servicio en el archivo de recursos service_token.json del proyecto.
* Abra el archivo PDFAUtilities.java y especifique la carpeta en la que desea guardar los archivos PDF generados.
* Abra ExecuteAssemblerService.java. Establezca el valor de la variable _AEM_FORMS_CS_ para que apunte a la instancia.
* Descomente las líneas adecuadas para probar operaciones PDFA.
* Ejecute ExecuteAssemblerService.java como aplicación java.



>[!NOTE]
> La primera vez que ejecute el programa java obtendrá el error HTTP 403. Para superar esto, asegúrese de proporcionar la variable [permisos adecuados para el usuario de cuenta técnica en AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Usuarios de AEM Forms** es el papel que he usado para este curso.
