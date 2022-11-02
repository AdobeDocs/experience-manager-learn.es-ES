---
title: Uso de fragmentos en el servicio de salida con la carpeta vigilada
description: Generar documentos pdf con fragmentos que residen en el repositorio de crx
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Generación de un documento pdf con fragmentos usando una secuencia de comandos ECMA{#developing-with-output-and-forms-services-in-aem-forms}


En este artículo utilizaremos el servicio de salida para generar archivos pdf usando fragmentos xdp. El xdp principal y los fragmentos residen en el repositorio crx. Es importante imitar la estructura de carpetas del sistema de archivos en AEM. Por ejemplo, si está utilizando un fragmento en la carpeta de fragmentos de su xdp, debe crear una carpeta llamada **fragmentos** debajo de la carpeta base en AEM. La carpeta base contendrá la plantilla xdp base. Por ejemplo, si tiene la siguiente estructura en el sistema de archivos
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* La carpeta xdpdocuments contendrá la plantilla base y los fragmentos de **fragmentos** carpeta

Puede crear la estructura necesaria utilizando la variable [iu de formularios y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

La siguiente es la estructura de carpetas para el ejemplo xdp que utiliza 2 fragmentos
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* Servicio de salida : normalmente este servicio se utiliza para combinar datos xml con plantillas xdp o pdf para generar pdf plano. Para obtener más información, consulte la [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para el servicio Output . En este ejemplo utilizamos fragmentos que residen en el repositorio crx.


Se utilizó el siguiente script ECMA generate PDF. Observe el uso de ResourceResolver y ResourceResolverHelper en el código. Se necesita ResourceReolver ya que este código se ejecuta fuera de cualquier contexto de usuario.

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**Para probar el paquete de muestra en el sistema**
* [Implementar el paquete DevelopingWithServiceUSer](assets/DevelopingWithServiceUser.jar)
* Añadir la entrada **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** en la modificación del servicio de asignación de usuarios como se muestra en la captura de pantalla siguiente
   ![enmienda del asignador de usuarios](assets/user-mapper-service-amendment.png)
* [Descargue e importe los archivos xdp de muestra y las secuencias de comandos ECMA](assets/watched-folder-fragments-ecma.zip).
Esto creará una estructura de carpetas vigilada en la carpeta c:/fragmentsandoutputservice

* [Extraer el archivo de datos de ejemplo](assets/usingFragmentsSampleData.zip) y colóquelo en la carpeta de instalación de su carpeta vigilada (c:\fragmentsandoutputservice\install)

* Compruebe la carpeta de resultados de la configuración de la carpeta observada para el archivo pdf generado
