---
title: Recuperar formulario adaptable guardado
description: Servlet para procesar el formulario adaptable con datos guardados
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6553
thumbnail: 6553.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d722cb9c-6c8a-44de-aaea-fc07a555b864
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 1%

---

# Recuperar formulario guardado

El siguiente paso es crear un servlet que procese el formulario adaptable con los datos guardados y sus archivos adjuntos.
El siguiente código de servlet se ejecuta después de comprobar el código OTP. Los datos del formulario adaptable y su asignación de archivos adjuntos asociados al ID de aplicación único se recuperan de la base de datos. El objeto de solicitud se rellena con los datos de formulario adaptables guardados y la asignación de archivos adjuntos. A continuación, se reenvía la solicitud para procesar el formulario &quot;storeafwithattachments&quot; previamente rellenado con los datos originales y sus archivos adjuntos.

```java
package store.and.fetch.core.servlets;

import java.io.IOException;
import java.io.StringReader;

import javax.servlet.RequestDispatcher;
import javax.servlet.Servlet;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathExpressionException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;
import com.day.cq.wcm.api.WCMMode;
import store.and.fetch.core.*;
@Component(service = {
    Servlet.class
}, property = {
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/renderaf"
})

public class RenderForm extends SlingAllMethodsServlet {
    /**
     * 
     */
    private static final Logger log = LoggerFactory.getLogger(RenderForm.class);
    private static final long serialVersionUID = 1 L;
    @Reference
    AemFormsAndDB aemFormsAndDB;
    public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
        try {
            log.debug("Inside w3cDocumentFromString");
            DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
            InputSource is = new InputSource();
            is.setCharacterStream(new StringReader(xmlString));
            return db.parse(is);
        } catch (Exception e) {
            log.debug(e.getMessage());
        }
        return null;
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("*****In my Render Form Servlet*****");
        String submittedData = request.getParameter("jcr:data");
        String applicationNo = "/afData/afUnboundData/data/ApplicationNumber";
        org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
        XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
        try {
            org.w3c.dom.Node applicationNode = (org.w3c.dom.Node) xPath.compile(applicationNo).evaluate(submittedXml, javax.xml.xpath.XPathConstants.NODE);
            log.debug("The application number we got was " + applicationNode.getTextContent());
            org.json.JSONObject afDataObject = aemFormsAndDB.getAFFormDataWithAttachments(applicationNode.getTextContent());
            log.debug("The data I got was " + afDataObject.toString());
            request.setAttribute("data", afDataObject.get("afData"));
            org.json.JSONObject customMap = new org.json.JSONObject();
            customMap.put("fileAttachmentMap", afDataObject.get("afAttachments"));
            request.setAttribute("customContextProperty", customMap.toString());
            ServletContext sc = getServletContext();
            RequestDispatcher dispatcher = sc.getRequestDispatcher("/content/forms/af/storeafwithattachments.html");
            WCMMode.DISABLED.toRequest(request);
            dispatcher.forward(request, response);
        } catch (JSONException | ServletException | IOException | XPathExpressionException e) {
            log.debug("The error message is " + e.getMessage());
        }

 }

}
```

## Pasos siguientes

[Cree una biblioteca de cliente para invocar el servlet y almacenar datos de formulario](./create-client-lib.md)
