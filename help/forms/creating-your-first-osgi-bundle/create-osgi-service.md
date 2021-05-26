---
title: Creación del primer servicio OSGi con AEM Forms
description: 'Cree su primer servicio OSGi con AEM Forms '
feature: Formularios adaptables
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: Desarrollo
role: Developer
level: Beginner
source-git-commit: c74c6f5627e69e32bbf0098d6b6bab122cace798
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 2%

---


# Servicio OSGi

Un servicio OSGi es una interfaz de servicio o clase Java, junto con varias propiedades de servicio como pares de nombre-valor. Las propiedades del servicio diferencian entre diferentes proveedores de servicios que proporcionan servicios con la misma interfaz de servicio.

Un servicio OSGi se define semánticamente por su interfaz de servicio y se implementa como un objeto de servicio. Las interfaces que implementa definen la funcionalidad de un servicio. Por lo tanto, diferentes aplicaciones pueden implementar el mismo servicio. Las interfaces de servicio permiten que los paquetes interactúen mediante interfaces de enlace, no implementaciones. Se debe especificar una interfaz de servicio con el menor número posible de detalles de implementación.

## Definición de la interfaz

Una interfaz sencilla con un método para combinar datos con la plantilla <span class="x x-first x-last">XDP</span>.

```java
package com.learningaemforms.adobe.core;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface {
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
} 
```

## Implementación de la interfaz

Cree un nuevo paquete llamado `com.learningaemforms.adobe.core.impl` para mantener la implementación de la interfaz.

```java
package com.learningaemforms.adobe.core.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.learningaemforms.adobe.core.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

La anotación `@Component(...)` en la línea 10 marca esta clase Java como un componente OSGi, así como la registra como un servicio OSGi.

La anotación `@Reference` forma parte de los servicios declarativos de OSGi y se utiliza para insertar una referencia de [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) en la variable `outputService`.


## Generar e implementar el paquete

* Abra la ventana **símbolo del sistema**
* Ir a `c:\aemformsbundles\learningaemforms\core`
* Ejecute el comando `mvn clean install -PautoInstallBundle`
* El comando anterior creará e implementará automáticamente el paquete en su instancia de AEM que se ejecuta en localhost:4502

El paquete también estará disponible en la siguiente ubicación `C:\AEMFormsBundles\learningaemforms\core\target`. El paquete también se puede implementar en AEM usando la consola web [Felix.](http://localhost:4502/system/console/bundles)

## Uso del servicio

Ahora puede utilizar el servicio en su página JSP. El siguiente fragmento de código muestra cómo obtener acceso al servicio y utilizar los métodos implementados por el servicio

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.learningaemforms.adobe.core.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

El paquete de muestra que contiene la página JSP se puede ![descargar desde aquí](assets/learning-aem-forms.zip)

## Probar el paquete

Importe e instale el paquete en AEM mediante el [gestor de paquetes](http://localhost:4502/crx/packmgr/index.jsp)

Utilice postman para realizar una llamada al POST y proporcione los parámetros de entrada como se muestra en la captura de pantalla siguiente
![postman](assets/test-service-postman.JPG)
