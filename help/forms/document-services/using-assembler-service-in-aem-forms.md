---
title: Usar el servicio Assembler en AEM Forms
description: Uso del servicio Assembler en AEM Forms para montar varios archivos PDF
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
duration: 121
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Usar el servicio Assembler en AEM Forms{#using-assembler-service-in-aem-forms}

Este artículo proporciona recursos para mostrar la capacidad de arrastrar y soltar varios archivos de PDF en el explorador y guardar el archivo PDF ensamblado en el sistema de archivos. El siguiente es el código del servlet que monta los archivos PDF cargados mediante el explorador.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

AEM Para que esta capacidad funcione en el servidor de la

* Descargue la [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) a su sistema local.
* Cargue e instale el paquete utilizando [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Descargar[Paquete de servicios de documentos personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Descargar [Desarrollo con paquete de usuario de servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Implementación e inicio de los paquetes mediante [consola web felix](http://localhost:4502/system/console/bundles)
* Dirija el explorador a [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* Arrastre y suelte un par de archivos de archivos de PDF

>[!NOTE]
>
>Asegúrese de que la instalación de AEM Forms haya finalizado. Todos los paquetes deben estar en estado activo.
>
>Asegúrese de haber agregado las bibliotecas RSA y BouncyCastle delegadas de arranque como se menciona en este [Instalación de AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**Advertencias para esta demostración**
>
> * El código no gestiona documentos de PDF basados en XFA
>
> * Asegúrese de arrastrar y soltar solo los archivos del PDF
>
>
