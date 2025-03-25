---
title: Administrar un envío de formulario HTML5
description: Crear controlador de envío de formularios HTML5.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# Administrar el envío de formularios HTML5

Los formularios HTML5 se pueden enviar a un servlet alojado en AEM. Se puede acceder a los datos enviados en el servlet como una secuencia de entrada. Para enviar el formulario de HTML5, agregue un &quot;Botón de envío HTTP&quot; en la plantilla de formulario mediante AEM Forms Designer.

## Cree su controlador de envío

Un servlet simple puede administrar el envío de formularios HTML5. Extraiga los datos enviados mediante el siguiente fragmento de código. Descargue el [servlet](assets/html5-submit-handler.zip) proporcionado en este tutorial. Instale el [servlet](assets/html5-submit-handler.zip) mediante el [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp).

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

Asegúrese de haber configurado la [configuración de SDK del cliente de Adobe LiveCycle](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) si planea utilizar el código para invocar un proceso J2EE.

## Configuración de la dirección URL de envío del formulario HTML5

![Enviar URL](assets/submit-url.PNG)

- Abra el xdp y vaya a _Propiedades_->_Avanzadas_.
- Copie http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html y péguelo en el campo de texto Enviar URL.
- Haga clic en el botón _Guardar y cerrar_.

### Añadir entrada en las rutas de exclusión

- Ir a [configMgr](http://localhost:4502/system/console/configMgr).
- Busque _Filtro CSRF de Adobe Granite_.
- Agregue la siguiente entrada en la sección Rutas excluidas: _/content/AemFormsSamples/handlehml5formsubmission_.
- Guarde los cambios.

### Prueba del formulario

- Abra la plantilla xdp.
- Haz clic en _Vista previa_->Vista previa como HTML.
- Introduzca datos en el formulario y haga clic en enviar.
- Compruebe los datos enviados en el archivo stdout.log del servidor.

### Lectura adicional

Para obtener más información sobre la generación de PDF a partir de los envíos de formularios HTML5, consulte este [artículo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html).

