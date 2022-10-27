---
title: Uso de API para generar un documento de registro con AEM Forms
description: Generar documento de registro (DOR) mediante programación
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 1%

---

# Uso de API para generar un documento de registro en AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Generar documento de registro (DOR) mediante programación

Este artículo ilustra el uso de la variable `com.adobe.aemds.guide.addon.dor.DoRService API` para generar **Documento de registro** mediante programación. [Documento de registro](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) es una versión PDF de los datos capturados en el formulario adaptable.

1. El siguiente es el fragmento de código. La primera línea recibe el servicio DOR.
1. Establezca DoROptions.
1. Invocar el método render del DoRService y pasar el objeto DoROptions al método render

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

Para probar esto en su sistema local, siga los siguientes pasos

1. [Descargue e instale los recursos del artículo mediante el administrador de paquetes](assets/dor-with-api.zip)
1. Asegúrese de que ha instalado e iniciado el paquete DevelopingWithServiceUser proporcionado como parte de [Artículo Crear usuario de servicio](service-user-tutorial-develop.md)
1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el servicio de asignador de usuarios del servicio Apache Sling
1. Asegúrese de introducir la siguiente entrada _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ en la sección Asignaciones de servicios
1. [Abrir el formulario](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Complete el formulario y haga clic en &quot;Ver PDF&quot;
1. Debería ver DOR en la nueva pestaña del navegador


**Consejos para la resolución de problemas**

El PDF no se muestra en la nueva pestaña del explorador:

1. Asegúrese de que no está bloqueando las ventanas emergentes en su explorador
1. Asegúrese de haber seguido los pasos descritos en esta [article](service-user-tutorial-develop.md)
1. Asegúrese de que el paquete &quot;DevelopingWithServiceUser&quot; esté en *estado activo*
1. Asegúrese de que los datos del usuario del sistema &#39; tienen permisos de lectura, modificación y creación en el siguiente nodo `/content/usergenerated/content/aemformsenablement`
