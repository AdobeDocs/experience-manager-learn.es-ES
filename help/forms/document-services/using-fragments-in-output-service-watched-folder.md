---
title: Usar fragmentos en el servicio de salida con la carpeta vigilada
description: Generar documentos pdf con fragmentos que residan en el repositorio crx
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
duration: 84
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# Generación de documentos PDF con fragmentos mediante script ECMA{#developing-with-output-and-forms-services-in-aem-forms}


En este artículo utilizaremos el servicio de salida para generar archivos pdf utilizando fragmentos xdp. El xdp principal y los fragmentos residen en el repositorio crx. Es importante imitar la estructura de carpetas del sistema de archivos en AEM. Por ejemplo, si está usando un fragmento en la carpeta de fragmentos de su xdp, debe crear una carpeta llamada **fragmentos** bajo la carpeta base en AEM. La carpeta base contendrá la plantilla xdp base. Por ejemplo, si tiene la siguiente estructura en el sistema de archivos
* c:\xdptemplates: contendrá la plantilla xdp base
* c:\xdptemplates\fragments: esta carpeta contendrá fragmentos y la plantilla principal hará referencia al fragmento como se muestra a continuación
  ![fragmento-xdp](assets/survey-fragment.png).
* La carpeta xdpdocuments contendrá su plantilla base y los fragmentos de la carpeta **fragments**

Puede crear la estructura necesaria con [forms y la interfaz de usuario del documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

La siguiente es la estructura de carpetas para el xdp de muestra, que utiliza 2 fragmentos
![formularios&amp;documento](assets/fragment-folder-structure-ui.png)


* Servicio de salida: normalmente, este servicio se utiliza para combinar datos xml con una plantilla xdp o un pdf para generar un pdf aplanado. Para obtener más información, consulte [javadoc](https://helpx.adobe.com/es/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para el servicio Output. En este ejemplo utilizamos fragmentos que residen en el repositorio crx.


El siguiente script ECMA se utilizó para generar PDF. Observe el uso de ResourceResolver y ResourceResolverHelper en el código. ResourceResolver es necesario, ya que este código se ejecuta fuera de cualquier contexto de usuario.

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
* [Implementar el paquete DesarrollandoConServicioUsuario](assets/DevelopingWithServiceUser.jar)
* Agregue la entrada **DesarrollandoConServicioUsuario.core:getformsresourceresolver=fd-service** en la modificación del servicio de asignación de usuarios como se muestra en la captura de pantalla siguiente
  ![modificación del asignador de usuarios](assets/user-mapper-service-amendment.png)
* [Descargue e importe los archivos xdp de ejemplo y los scripts ECMA](assets/watched-folder-fragments-ecma.zip).
Esto creará una estructura de carpetas vigilada en la carpeta c:/fragmentsandoutputservice

* [Extraiga el archivo de datos de ejemplo](assets/usingFragmentsSampleData.zip) y colóquelo en la carpeta de instalación de la carpeta vigilada(c:\fragmentsandoutputservice\install)

* Consulte la carpeta de resultados de la configuración de la carpeta inspeccionada para ver el archivo PDF generado
