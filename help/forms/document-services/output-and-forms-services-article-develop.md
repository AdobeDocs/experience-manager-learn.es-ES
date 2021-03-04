---
title: Desarrollo con Output y Forms Services en AEM Forms
seo-title: Desarrollo con Output y Forms Services en AEM Forms
description: Uso de la API de Output y Forms Service en AEM Forms
seo-description: Uso de la API de Output y Forms Service en AEM Forms
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---


# Desarrollo con Output y Forms Services en AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Uso de la API de Output y Forms Service en AEM Forms

En este artículo echaremos un vistazo a lo siguiente

* Servicio de salida: normalmente este servicio se utiliza para combinar datos xml con plantillas xdp o pdf para generar pdf plano
* FormsService : Se trata de un servicio muy versátil que le permite exportar e importar datos desde y hacia un archivo PDF

El javadoc oficial para la API de AEM Forms se enumera [aquí](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

El siguiente fragmento de código exporta datos desde un archivo PDF

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

La línea 1 extrae el archivo pdffile de la solicitud

Line2 extrae saveLocation de la solicitud

Línea 5 obtiene la retención de FormsService

La línea 6 exporta el xmlData desde el archivo PDF

**Para probar el paquete de muestra en el sistema**

[Descargue e instale el paquete mediante el gestor de paquetes AEM](assets/outputandformsservice.zip)




**Después de instalar el paquete, tendrá que incluir las siguientes direcciones URL en el filtro CSRF de Adobe Granite.**

1. Siga los pasos que se indican a continuación para incluir en la lista de permitidos las rutas mencionadas anteriormente.
1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el filtro CSRF de Adobe Granite
1. Añada las 3 rutas siguientes en las secciones excluidas y guarde
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Buscar &quot;Filtro de referente de Sling&quot;
1. Marque la casilla de verificación &quot;Permitir vacío&quot;. (Esta configuración solo debe utilizarse con fines de prueba)
Existen varias formas de probar el código de muestra. Lo más rápido y fácil es usar la aplicación Postman. Postman le permite realizar solicitudes POST a su servidor. Instale la aplicación Postman en su sistema.
Inicie la aplicación e introduzca la siguiente URL para probar la API de datos de exportación

Asegúrese de haber seleccionado &quot;POST&quot; en la lista desplegable
http://localhost:4502/content/AemFormsSamples/exportdata.html
Asegúrese de especificar &quot;Autorización&quot; como &quot;Autenticación básica&quot;. Especifique el nombre de usuario y la contraseña del servidor AEM
Vaya a la pestaña &quot;Cuerpo&quot; y especifique los parámetros de solicitud como se muestra en la imagen siguiente
![exportar](assets/postexport.png)
A continuación, haga clic en el botón Send

El paquete contiene 3 muestras. En los párrafos siguientes se explica cuándo utilizar el servicio de salida o el servicio de Forms, la url del servicio, los parámetros de entrada que cada servicio espera

**Combinar datos y acoplar salida:**

* Utilice Output Service para combinar datos con documentos xdp o pdf para generar pdf plano
* **URL** de POST: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parámetros de solicitud -**

   * xdp_or_pdf_file : El archivo xdp o pdf con el que desea combinar los datos
   * xmlfile: El archivo de datos xml que se combinará con xdp_or_pdf_file
   * saveLocation: Ubicación para guardar el documento procesado en el sistema de archivos

**Importar datos en archivo PDF:**
* Utilizar FormsService para importar datos en un archivo PDF
* **URL**  de POST: http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parámetros de solicitud:**

   * pdffile : El archivo pdf con el que desea combinar los datos
   * xmlfile: El archivo de datos xml que se combinará con el archivo pdf
   * saveLocation: Ubicación para guardar el documento procesado en el sistema de archivos. Por ejemplo, c:\\\outputsample.pdf.

**Exportar datos de un archivo PDF**
* Utilizar FormsService para exportar datos desde un archivo PDF
* **URL de** la publicación: http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parámetros de solicitud:**

   * pdffile : El archivo pdf desde el que desea exportar datos
   * saveLocation: Ubicación para guardar los datos exportados en el sistema de archivos

[Puede importar esta colección de postman para probar la API](assets/document-services-postman-collection.json)

