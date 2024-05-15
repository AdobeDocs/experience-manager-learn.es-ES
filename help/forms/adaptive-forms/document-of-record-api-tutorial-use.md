---
title: Usar la API para generar el documento de registro con AEM Forms
description: Generar documento de registro (DOR) mediante programación
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
duration: 67
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# Usar la API para generar el documento de registro en AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Generar documento de registro (DOR) mediante programación

Este artículo ilustra el uso de la variable `com.adobe.aemds.guide.addon.dor.DoRService API` para generar **Documento de registro** mediante programación. [Documento de registro](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/asynchronous-submissions-adaptive-forms.html?lang=es) es una versión del PDF de los datos capturados en el formulario adaptable.

1. A continuación se muestra el fragmento de código. La primera línea obtiene el servicio DOR.
1. Establezca DoROptions.
1. Invoque el método de procesamiento del DoRService y pase el objeto DoROptions al método de procesamiento

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
```

Para probar esto en su sistema local, siga los siguientes pasos

1. [Descargar e instalar los recursos del artículo mediante el administrador de paquetes](assets/dor-with-api.zip)
1. Asegúrese de haber instalado e iniciado el paquete DevelopersWithServiceUser proporcionado como parte de [Artículo Crear usuario de servicio](service-user-tutorial-develop.md)
1. [Inicie sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el servicio de asignador de usuarios del servicio Apache Sling
1. Asegúrese de introducir la siguiente entrada _DesarrollarWithServiceUser.core:getformsresourceresolver=fd-service_ en la sección Asignaciones de servicios
1. [Abra el formulario](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Rellene el formulario y haga clic en &quot;Ver PDF&quot;
1. Debe ver el documento de registro en la nueva pestaña del explorador


**Sugerencias de resolución de problemas**

El PDF no se muestra en la nueva pestaña del explorador:

1. Asegúrese de que no está bloqueando las ventanas emergentes del explorador
1. AEM Asegúrese de que está iniciando el servidor como administrador (al menos en Windows).
1. Asegúrese de que el paquete &quot;Desarrollando con usuario de servicio&quot; esté en *estado activo*
1. [Asegúrese de que el usuario del sistema](http://localhost:4502/useradmin) &quot;fd-service&quot; tiene permisos de lectura, modificación y creación en el siguiente nodo `/content/usergenerated/content/aemformsenablement`
