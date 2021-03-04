---
title: Uso de API para generar un documento de registro con AEM Forms
seo-title: Uso de API para generar un documento de registro con AEM Forms
description: Generar documento de registro (DOR) mediante programación
seo-description: Uso de API para generar un documento de registro con AEM Forms
feature: Formularios adaptables
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Uso de API para generar un documento de registro en AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Generar documento de registro (DOR) mediante programación

Este artículo ilustra el uso de `com.adobe.aemds.guide.addon.dor.DoRService API` para generar **Documento de registro** mediante programación. [El documento de ](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) grabación es una versión PDF de los datos capturados en el formulario adaptable.

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
1. Asegúrese de que ha instalado e iniciado el paquete DevelopingWithServiceUser proporcionado como parte del [artículo Crear usuario de servicio](service-user-tutorial-develop.md)
1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el servicio de asignador de usuarios del servicio Apache Sling
1. Asegúrese de la siguiente entrada _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ en la sección Asignaciones de servicios
1. [Abrir el formulario](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Rellene el formulario y haga clic en &quot;Ver PDF&quot;
1. Debería ver DOR en la nueva pestaña del navegador


**Consejos para la resolución de problemas**

El PDF no se muestra en la nueva ficha del explorador:

1. Asegúrese de que no está bloqueando las ventanas emergentes en su explorador
1. Asegúrese de haber seguido los pasos descritos en este [artículo](service-user-tutorial-develop.md)
1. Asegúrese de que el paquete &quot;DevelopingWithServiceUser&quot; esté en *estado activo*
1. Asegúrese de que los datos del usuario del sistema &#39; tienen permisos de lectura, modificación y creación en el siguiente nodo `/content/usergenerated/content/aemformsenablement`

