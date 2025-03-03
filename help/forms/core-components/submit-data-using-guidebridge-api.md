---
title: Usar la API de GuideBridge para publicar datos de formulario
description: Obtenga información sobre cómo acceder y enviar datos de formulario mediante la API de GuideBridge para formularios adaptables. Guarde y recupere datos de formulario con facilidad.
duration: 68
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-15286
last-substantial-update: 2024-04-05T00:00:00Z
exl-id: 099aaeaf-2514-4459-81a7-2843baa1c981
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 1%

---


# Acceder y enviar datos de formulario con la API de GuideBridge

Aprenda a utilizar la API de GuideBridge para acceder a los datos de formulario y enviarlos a un extremo REST para su almacenamiento y recuperación. Esta funcionalidad permite a los usuarios guardar y reanudar la cumplimentación de formularios sin problemas.

Los datos del formulario se guardan activando una función de JavaScript al hacer clic en un botón del editor de reglas.

![Editor de reglas](assets/rule-editor.png)

La siguiente función de JavaScript muestra cómo enviar los datos del formulario al extremo especificado:

```javascript
/**
* Submits data and attachments 
* @name submitFormDataAndAttachments Submit form data and attachments to REST endpoint
* @param {string} endpoint in String format
* @return {string} 
 */
 
function submitFormDataAndAttachments(endpoint) {
    guideBridge.getFormDataObject({
        success: function(resultObj) {
            const afFormData = resultObj.data.data;
            const formData = new FormData();
            formData.append("dataXml", afFormData);
            resultObj.data.attachments.forEach(attachment => {
                console.log(attachment.name);
                formData.append(attachment.name, attachment.data);
            });
            fetch(endpoint, {
                method: 'POST',
                body: formData
            })
            .then(response => {
                if (response.ok) {
                    console.log("Successfully saved");
                    const fld = guideBridge.resolveNode("$form.confirmation");
                    return "Form data was saved successfully";
                } else {
                    throw new Error('Failed to save form data');
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    });
}
```

## Código del lado del servidor

El siguiente código Java del lado del servidor administra el procesamiento de datos del formulario. Este servlet Java en AEM se invoca mediante una llamada XHR en la función JavaScript anterior.

```java
package com.azuredemo.core.servlets;

import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.Serializable;
import java.util.List;

@Component(
   service = {
      Servlet.class
   }
)
@SlingServletResourceTypes(
   resourceTypes = "azure/handleFormSubmission",
   methods = "POST",
   extensions = "json"
)
public class StoreFormSubmission extends SlingAllMethodsServlet implements Serializable {
   private static final long serialVersionUID = 1L;
   private final transient Logger log = LoggerFactory.getLogger(this.getClass());

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      List<RequestParameter> listOfRequestParameters = request.getRequestParameterList();
      log.debug("The size of the list is " + listOfRequestParameters.size());
      
      for (int i = 0; i < listOfRequestParameters.size(); i++) {
         RequestParameter requestParameter = listOfRequestParameters.get(i);
         log.debug("Is this request parameter a form field?" + requestParameter.isFormField());
         
         if (!requestParameter.isFormField()) {
            Document attachmentDOC = new Document(requestParameter.getInputStream());
            attachmentDOC.copyToFile(new File(requestParameter.getName()));
         } else {
            log.debug("Not a form field " + requestParameter.getName());
            log.debug(requestParameter.getString());
         }
      }
      
      response.setStatus(HttpServletResponse.SC_OK);
   }
}
```
