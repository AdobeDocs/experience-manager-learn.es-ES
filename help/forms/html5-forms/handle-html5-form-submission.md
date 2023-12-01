---
title: Controlar el envío de formularios de HTML 5
description: Crear controlador de envío de formulario de HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 3%

---

# Controlar el envío de formularios de HTML 5

Los formularios de HTML AEM 5 se pueden enviar al servlet alojado en la base de datos de. Se puede acceder a los datos enviados en el servlet como una secuencia de entrada. Para enviar el formulario de HTML5, debe agregar el botón de envío HTTP en la plantilla de formulario mediante AEM Forms Designer

## Cree su controlador de envío

Se puede crear un servlet simple para administrar el envío del formulario de HTML5. Los datos enviados se pueden extraer utilizando el siguiente código. Esta [servlet](assets/html5-submit-handler.zip) está disponible como parte de este tutorial. Instale el [servlet](assets/html5-submit-handler.zip) usando [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)

El código de la línea 9 se puede utilizar para invocar el proceso J2EE. Asegúrese de que ha configurado [Configuración del SDK del cliente de LiveCycle de Adobe](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) si tiene intención de utilizar el código para invocar el proceso J2EE.

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


## Configuración de la URL de envío del formulario de HTML5

![submit-url](assets/submit-url.PNG)

* Pulse en el xdp y haga clic en _Propiedades_->_Avanzadas_
* copie http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html y pegue esto en el campo de texto Enviar URL
* Clic _Guardar y cerrar_ botón.

### Añadir entrada en las rutas de exclusión

* Vaya a [configMgr](http://localhost:4502/system/console/configMgr).
* Buscar por _Adobe Granite CSRF Filter_
* Añada la siguiente entrada en la sección Rutas excluidas
* _/content/AemFormsSamples/handlehml5formsubmission_
* Guarde los cambios

### Prueba del formulario

* Pulse en la plantilla xdp.
* Haga clic en _Previsualizar_->Vista previa como HTML
* Introduzca algunos datos en el formulario y haga clic en enviar
* Debería ver los datos enviados escritos en el archivo stdout.log de su servidor

### Lectura adicional

Esta [artículo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) sobre la generación de un PDF a partir del envío del formulario de HTML5 también se recomienda.
