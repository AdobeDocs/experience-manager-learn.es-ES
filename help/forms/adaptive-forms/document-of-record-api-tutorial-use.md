---
title: Uso de API para generar Documento de registros con AEM Forms
seo-title: Uso de API para generar Documento de registros con AEM Forms
description: Generar Documento de registros (DOR) mediante programación
seo-description: Uso de API para generar Documento de registros con AEM Forms
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Uso de API para generar Documento de registros en AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Generar Documento de registros (DOR) mediante programación

Este artículo ilustra el uso de `com.adobe.aemds.guide.addon.dor.DoRService API` para generar **Documento de registro** mediante programación. [Documento de ](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) grabaciones es una versión PDF de los datos capturados en el formulario adaptable.

1. El siguiente es el fragmento de código. La primera línea recibe el servicio DOR.
1. Establezca DoROoptions.
1. Invocar el método de procesamiento de DoRService y pasar el objeto DoROoptions al método de procesamiento

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

Para probar esto en su sistema local, siga los pasos siguientes

1. [Descargar e instalar los recursos del artículo mediante el administrador de paquetes](assets/dor-with-api.zip)
1. Asegúrese de que ha instalado e iniciado el paquete DevelopingWithServiceUser proporcionado como parte del artículo [Crear usuario de servicio](service-user-tutorial-develop.md)
1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el servicio de asignación de usuarios del servicio Apache Sling
1. Asegúrese de la siguiente entrada _DevelopingWithServiceUser.core:getformsresources ceresolver=fd-service_ en la sección Asignaciones de servicios
1. [Abrir el formulario](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Rellene el formulario y haga clic en &#39; Vista PDF &#39;
1. Debería ver el DOR en la nueva ficha del explorador


**Sugerencias para la resolución de problemas**

El PDF no se muestra en la nueva ficha del explorador:

1. Asegúrese de que no está bloqueando las ventanas emergentes en el explorador
1. Haga que haya seguido los pasos descritos en este [artículo](service-user-tutorial-develop.md)
1. Asegúrese de que el paquete &#39;DevelopingWithServiceUser&#39; está en *estado activo*
1. Asegúrese de que los datos &#39; del usuario del sistema &#39; tienen permisos de lectura, modificación y creación en el nodo siguiente `/content/usergenerated/content/aemformsenablement`

