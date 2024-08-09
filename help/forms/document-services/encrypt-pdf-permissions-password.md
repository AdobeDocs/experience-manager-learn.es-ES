---
title: Cifrar PDF con una contraseña de permisos
description: Usar DocAssuranceService para cifrar un PDF
feature: Document Services
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-15849
last-substantial-update: 2024-07-19T00:00:00Z
exl-id: 5df8581c-a44c-449c-bf3b-8cdf57635c4d
source-git-commit: d01a56cd1fd3085b0230918b15b4635ba375e346
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Cifrar el PDF con una contraseña de permiso

Se requiere una contraseña de permisos, también conocida como contraseña de propietario o maestra, para copiar, editar o imprimir un documento de PDF. Aprenda a utilizar la API [DocAssuranceService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/docassurance/client/api/DocAssuranceService.html) para aplicar una contraseña de permiso a un PDF mediante programación.

El siguiente código JSP cifra un PDF con una contraseña de permisos:

```java
<%--
     Encrypt PDF with permissions password
--%>
    <%@include file="/libs/foundation/global.jsp"%>
<%@ page import="com.adobe.fd.docassurance.client.api.EncryptionOptions,java.util.*,java.io.*,com.adobe.fd.encryption.client.*" %>
    <%@page session="false" %>
<%
    String filePath = request.getParameter("saveLocation");
    InputStream pdfIS = null;
    com.adobe.aemfd.docmanager.Document generatedDocument = null;
    // get the pdf file
    javax.servlet.http.Part pdfPart = request.getPart("pdfFile");
    pdfIS = pdfPart.getInputStream();
    com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);


// encrypt the document with permssions password. You can only print this document
    PasswordEncryptionOptionSpec poSpec = new PasswordEncryptionOptionSpec();    
    poSpec.setCompatability(PasswordEncryptionCompatability.ACRO_X);
    poSpec.setEncryptOption(PasswordEncryptionOption.ALL);
    List<PasswordEncryptionPermission> permissionList = new ArrayList<PasswordEncryptionPermission>();
    permissionList.add(PasswordEncryptionPermission.PASSWORD_PRINT_LOW);
    //hardcoding passwords into code is for demonstration purposes only.In real life scenarios the password is sourced from a secure location
    poSpec.setPermissionPassword("adobe");
    poSpec.setPermissionsRequested(permissionList);
    EncryptionOptions encryptionOptions = EncryptionOptions.getInstance();
    encryptionOptions.setEncryptionType(com.adobe.fd.docassurance.client.api.DocAssuranceServiceOperationTypes.ENCRYPT_WITH_PASSWORD);
    encryptionOptions.setPasswordEncryptionOptionSpec(poSpec);
    com.adobe.fd.docassurance.client.api.DocAssuranceService docAssuranceService = sling.getService(com.adobe.fd.docassurance.client.api.DocAssuranceService.class);
    com.adobe.aemfd.docmanager.Document securedDocument = docAssuranceService.secureDocument(pdfDocument,encryptionOptions,null,null,null);
    securedDocument.copyToFile(new java.io.File(filePath));
    out.println("Document encrypted and saved to " +filePath);
%>
```


**Para probar el paquete de muestra en el sistema**

[AEM Descargue e instale el paquete mediante el administrador de paquetes de la](assets/encryptpdf.zip)

**Después de instalar el paquete, agregue las siguientes direcciones URL a la lista de permitidos de configuración OSGi del filtro CSRF de Granite de Adobe:**

1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Búsqueda de Adobe Granite CSRF Filter
1. Añada la siguiente ruta en las secciones excluidas y guarde
1. /content/AemFormsSamples/encrypt

## Prueba de la muestra

Existen varias formas de probar el código de ejemplo. La forma más rápida y sencilla de usar una aplicación de Postman es. Postman permite realizar solicitudes de POST al servidor. La siguiente captura de pantalla muestra los parámetros de solicitud necesarios para que funcione la solicitud de publicación. Asegúrese de especificar el tipo de autorización adecuado antes de enviar la solicitud.

![encrypt-pdf-postman](assets/encrypt-pdf-postman.png)
