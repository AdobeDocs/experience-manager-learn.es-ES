---
title: Generación de varios archivos pdf a partir de un archivo de datos
description: OutputService proporciona varios métodos para crear documentos utilizando un diseño de formulario y datos para combinarlos con el diseño de formulario. Aprenda a generar varios pdf a partir de un xml grande que contenga varios registros individuales.
feature: Servicio de salida
version: 6.4,6.5
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 1%

---


# Generar un conjunto de documentos PDF a partir de un archivo de datos xml

OutputService proporciona varios métodos para crear documentos utilizando un diseño de formulario y datos para combinarlos con el diseño de formulario. El siguiente artículo explica el caso de uso para generar varios pdf a partir de un xml grande que contiene varios registros individuales.
A continuación se muestra la captura de pantalla del archivo xml que contiene varios registros.

![multi-record-xml](assets/multi-record-xml.PNG)

El xml de datos tiene 2 registros. Cada registro se representa mediante el elemento form1 . Este xml se pasa al método OutputService [generatePDFOutputBatch](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) obtenemos una lista de documentos pdf (uno por registro)
La firma del método generatePDFOutputBatch toma los siguientes parámetros

* plantillas : mapa que contiene la plantilla, identificada con una clave
* data : mapa que contiene documentos de datos xml, identificados por clave
* pdfOutputOptions: opciones para configurar la generación de pdf
* batchOptions: opciones para configurar batch

>[!NOTE]
>
>Este caso de uso está disponible como ejemplo activo en este [sitio web](https://forms.enablementadobe.com/content/samples/samples.html?query=0).

## Detalles de caso de uso{#use-case-details}

En este caso de uso vamos a proporcionar una sencilla interfaz web para cargar la plantilla y el archivo data(xml). Una vez finalizada la carga de los archivos y enviada la solicitud del POST a AEM servlet. Este servlet extrae los documentos y llama al método generatePDFOutputBatch del OutputService. Los archivos pdf generados se comprimen en un archivo zip y se ponen a disposición del usuario final para su descarga desde el explorador web.

## Código Servlet{#servlet-code}

El siguiente es el fragmento de código del servlet. El código extrae la plantilla (xdp) y el archivo de datos (xml) de la solicitud. El archivo de plantilla se guarda en el sistema de archivos. Se crean dos mapas: templateMap y dataFileMap que contienen la plantilla y los archivos xml(data) respectivamente. A continuación, se realiza una llamada al método generateMultipleRecords del servicio DocumentServices.

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### Código de implementación de interfaz{#Interface-Implementation-Code}

El siguiente código genera varios pdf utilizando generatePDFOutputBatch de OutputService y devuelve un archivo zip que contiene los archivos pdf al servlet que realiza la llamada

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### Implementar en el servidor{#Deploy-on-your-server}

Para probar esta capacidad en su servidor, siga las siguientes instrucciones:

* [Descargue y extraiga el contenido del archivo zip en su sistema](assets/mult-records-template-and-xml-file.zip) de archivos. Este archivo zip contiene la plantilla y el archivo de datos xml.
* [Apunte el navegador a la consola web Felix](http://localhost:4502/system/console/bundles)
* [Implementar el paquete DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Implementar el paquete personalizado AEMFormsDocumentServices Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Custom que genera el pdf mediante la API de OutputService
* [Apunte el navegador al gestor de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Importe e instale el paquete](assets/generate-multiple-pdf-from-xml.zip). Este paquete contiene una página html que permite soltar la plantilla y los archivos de datos.
* [Apunte el navegador a MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Arrastre y suelte la plantilla y el archivo de datos xml juntos
* Descargue el archivo zip creado. Este archivo zip contiene los archivos pdf generados por el servicio de salida.

>[!NOTE]
>Existen varias formas de almacenar en déclencheur esta capacidad. En este ejemplo hemos utilizado una interfaz web para soltar la plantilla y el archivo de datos para demostrar la capacidad.

