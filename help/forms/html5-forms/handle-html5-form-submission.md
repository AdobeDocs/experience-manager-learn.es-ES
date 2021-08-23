---
title: Gestión del envío de formularios HTML5
description: Creación del controlador de envío de formularios HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 2%

---


# Gestión del envío de formularios HTML5

Los formularios HTML5 se pueden enviar al servlet alojado en AEM. Se puede acceder a los datos enviados en el servlet como flujo de entrada. Para enviar el formulario HTML5, debe agregar &quot;Botón Enviar HTTP&quot; en la plantilla de formulario con AEM Forms Designer

## Creación del controlador de envío

Se puede crear un servlet simple para gestionar el envío de formularios HTML5. Los datos enviados se pueden extraer utilizando el siguiente código. Este [servlet](assets/html5-submit-handler.zip) está disponible como parte de este tutorial. Instale el [servlet](assets/html5-submit-handler.zip) utilizando el [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)

El código de la línea 9 puede utilizarse para invocar el proceso J2EE. Asegúrese de haber configurado la [configuración del SDK del cliente de LiveCycle de Adobe](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) si desea utilizar el código para invocar el proceso J2EE.

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## Configuración de la URL de envío del formulario HTML5

![submit-url](assets/submit-url.PNG)

* Pulse en el xdp y haga clic en _Propiedades_->_Avanzadas_
* copie http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html y péguelo en el campo de texto Enviar URL
* Haga clic en el botón _SaveAndClose_.

### Agregar entrada en Excluir rutas

* Vaya a [configMgr](http://localhost:4502/system/console/configMgr).
* Buscar _Adobe Granite CSRF Filter_
* Agregue la siguiente entrada en la sección Rutas excluidas
* _/content/AemFormsSamples/handlehml5formsubmit_
* Guarde los cambios

### Comprobación del formulario

* Puntee en la plantilla xdp .
* Haga clic en _Vista previa_->Vista previa como HTML
* Introduzca algunos datos en el formulario y haga clic en enviar .
* Debería ver los datos enviados escritos en el archivo stdout.log de su servidor

### Lectura adicional

También se recomienda este [artículo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) sobre la generación de PDF a partir del envío de formularios HTML5.




