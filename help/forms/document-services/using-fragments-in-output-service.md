---
title: Usar fragmentos en el servicio de salida
description: Generar documentos pdf con fragmentos que residan en el repositorio crx
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
exl-id: d7887e2e-c2d4-4f0c-b117-ba7c41ea539a
duration: 106
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Generación de documentos pdf mediante fragmentos{#developing-with-output-and-forms-services-in-aem-forms}


En este artículo utilizaremos el servicio de salida para generar archivos pdf utilizando fragmentos xdp. El xdp principal y los fragmentos residen en el repositorio crx. Es importante imitar la estructura de carpetas del sistema de archivos en AEM. Por ejemplo, si está usando un fragmento en la carpeta de fragmentos de su xdp, debe crear una carpeta llamada **fragmentos** bajo la carpeta base en AEM. La carpeta base contendrá la plantilla xdp base. Por ejemplo, si tiene la siguiente estructura en el sistema de archivos
* c:\xdptemplates: contendrá la plantilla xdp base
* c:\xdptemplates\fragments: esta carpeta contendrá fragmentos y la plantilla principal hará referencia al fragmento como se muestra a continuación
  ![fragmento-xdp](assets/survey-fragment.png).
* La carpeta xdpdocuments contendrá su plantilla base y los fragmentos de la carpeta **fragments**

Puede crear la estructura necesaria con [forms y la interfaz de usuario del documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

La siguiente es la estructura de carpetas para el xdp de muestra, que utiliza 2 fragmentos
![formularios&amp;documento](assets/fragment-folder-structure-ui.png)


* Servicio de salida: normalmente, este servicio se utiliza para combinar datos xml con una plantilla xdp o un pdf para generar un pdf aplanado. Para obtener más información, consulte [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para el servicio Output. En este ejemplo utilizamos fragmentos que residen en el repositorio crx.


El siguiente código se utilizó para incluir fragmentos en el archivo PDF

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**Para probar el paquete de muestra en el sistema**

* [Descargue e importe los archivos xdp de ejemplo en AEM](assets/xdp-templates-fragments.zip)
* [Descargue e instale el paquete mediante el administrador de paquetes de AEM](assets/using-fragments-assets.zip)
* [El xdp de muestra y los fragmentos se pueden descargar desde aquí](assets/xdptemplates.zip)

**Después de instalar el paquete, tendrá que lista de permitidos las siguientes direcciones URL en el filtro CSRF de Adobe Granite.**

1. Siga los pasos que se mencionan a continuación para lista de permitidos las rutas mencionadas anteriormente.
1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Búsqueda del filtro CSRF de Adobe Granite
1. Añada la siguiente ruta en las secciones excluidas y guarde
1. /content/AemFormsSamples/usingfragments

Existen varias formas de probar el código de ejemplo. La forma más rápida y sencilla de usar una aplicación de Postman es. Postman le permite realizar peticiones POST al servidor. Instale la aplicación de Postman en su sistema.
Inicie la aplicación e introduzca la siguiente URL para probar la API de datos de exportación

Asegúrese de haber seleccionado &quot;POST&quot; en la lista desplegable
http://localhost:4502/content/AemFormsSamples/usingfragments.html
Asegúrese de especificar &quot;Autorización&quot; como &quot;Autenticación básica&quot;. Especifique el nombre de usuario y la contraseña del servidor de AEM
Vaya a la pestaña &quot;Cuerpo&quot; y especifique los parámetros de solicitud como se muestra en la siguiente imagen
![exportar](assets/using-fragment-postman.png)
A continuación, haga clic en el botón Send

[Puede importar esta colección de Postman para probar la API](assets/usingfragments.postman_collection.json)
