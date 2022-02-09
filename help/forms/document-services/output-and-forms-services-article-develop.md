---
title: Desarrollo con Output y Forms Services en AEM Forms
description: Uso de la API de Output y Forms Service en AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '601'
ht-degree: 0%

---

# Desarrollo con Output y Forms Services en AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Uso de la API de Output y Forms Service en AEM Forms

En este artículo echaremos un vistazo a lo siguiente

* Servicio de salida : normalmente este servicio se utiliza para combinar datos xml con plantillas xdp o pdf para generar pdf plano. Para obtener más información, consulte la[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para el servicio Output .
* FormsService : se trata de un servicio muy versátil que le permite exportar e importar datos desde y hacia un archivo PDF. Para obtener más información, consulte la [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/forms/api/class-use/FormsService.html) para el servicio Forms.


El siguiente fragmento de código exporta datos desde el archivo PDF

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

[Descargue e instale el paquete mediante el administrador de paquetes de AEM](assets/outputandformsservice.zip)




**Después de instalar el paquete tendrá que lista de permitidos las siguientes URL en el Adobe Granite CSRF Filter.**

1. Siga los pasos que se indican a continuación para realizar la lista de permitidos de las rutas mencionadas anteriormente.
1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el filtro de Adobe Granite CSRF
1. Añada las 3 rutas siguientes en las secciones excluidas y guarde
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Buscar &quot;Filtro de referente de Sling&quot;
1. Marque la casilla de verificación &quot;Permitir vacío&quot;. (Esta configuración solo debe utilizarse con fines de prueba) Existen varias formas de probar el código de muestra. Lo más rápido y fácil es usar la aplicación Postman. Postman le permite realizar solicitudes de POST a su servidor. Instale la aplicación Postman en su sistema.
Inicie la aplicación e introduzca la siguiente URL para probar la API de datos de exportación

Asegúrese de haber seleccionado &quot;POST&quot; en la lista desplegable http://localhost:4502/content/AemFormsSamples/exportdata.html Asegúrese de especificar &quot;Autorización&quot; como &quot;Auth básica&quot;. Especifique el nombre de usuario y la contraseña del servidor de AEM Vaya a la pestaña &quot;Cuerpo&quot; y especifique los parámetros de solicitud, tal y como se muestra en la imagen siguiente
![exportar](assets/postexport.png)
A continuación, haga clic en el botón Send

El paquete contiene 3 muestras. En los párrafos siguientes se explica cuándo utilizar el servicio de salida o el servicio de Forms, la url del servicio , los parámetros de entrada que cada servicio espera

## Combinar datos y acoplar salida

* Utilice Output Service para combinar datos con documentos xdp o pdf para generar pdf plano
* **URL del POST**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parámetros de solicitud -**

   * **xdp_or_pdf_file** : El archivo xdp o pdf con el que desea combinar los datos
   * **xmlfile**: El archivo de datos xml que se combinará con xdp_or_pdf_file
   * **saveLocation**: Ubicación para guardar el documento procesado en el sistema de archivos. Por ejemplo c:\\documents\\sample.pdf

### Importar datos en archivo PDF

* Usar FormsService para importar datos en un archivo de PDF
* **URL del POST** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parámetros de solicitud:**

   * **pdffile** : El archivo pdf con el que desea combinar los datos
   * **xmlfile**: El archivo de datos xml que se combinará con el archivo pdf
   * **saveLocation**: Ubicación para guardar el documento procesado en el sistema de archivos. Por ejemplo, c:\\outputsample.pdf.

**Exportar datos de un archivo de PDF**
* Usar FormsService para exportar datos desde el archivo PDF
* **URL del POST** L: http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parámetros de solicitud:**

   * **pdffile** : El archivo pdf desde el que desea exportar datos
   * **saveLocation**: Ubicación para guardar los datos exportados en el sistema de archivos. Por ejemplo c:\\documents\\exported_data.xml

[Puede importar esta colección de postman para probar la API](assets/document-services-postman-collection.json)
