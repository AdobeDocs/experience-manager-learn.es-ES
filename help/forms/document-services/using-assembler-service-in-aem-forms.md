---
title: Uso del servicio de ensamblador en AEM Forms
seo-title: Uso del servicio de ensamblador en AEM Forms
description: Uso del servicio de ensamblador en AEM Forms para montar varios archivos PDF
seo-description: Uso del servicio de ensamblador en AEM Forms para montar varios archivos PDF
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---


# Uso del servicio de ensamblador en AEM Forms{#using-assembler-service-in-aem-forms}

Este artículo le proporciona los elementos necesarios para demostrar la capacidad de arrastrar y soltar varios archivos PDF en el navegador y guardar el archivo PDF ensamblado en el sistema de archivos. El siguiente es el código del servlet que ensambla los archivos pdf cargados mediante el navegador.

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

Para que esta capacidad funcione en el servidor AEM

* Descargue [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) en su sistema local.
* Cargue e instale el paquete mediante el [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Descargar[Paquete de servicios de Documento personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Descargar [Desarrollo con el paquete de usuario de servicios](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Implemente y inicio los paquetes utilizando la consola web [felix](http://localhost:4502/system/console/bundles)
* Seleccione el explorador para [EnsamblarPdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* Arrastrar y soltar un par de archivos PDF

>[!NOTE]
>
>Asegúrese de que la instalación de AEM Forms ha finalizado. Todos los paquetes deben estar en estado activo.
>
>Asegúrese de haber agregado las bibliotecas RSA y BouncyCastle delegadas de arranque como se indica en esta [Instalación de AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**Advertencias para esta demostración**
>
> * El código no gestiona documentos PDF basados en XFA
   >
   > 
* Asegúrese de arrastrar y soltar solo archivos PDF
>
>







