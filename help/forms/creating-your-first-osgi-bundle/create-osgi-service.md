---
title: Creación de su primer servicio OSGi con AEM Forms
description: Cree su primer servicio OSGi con AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
duration: 103
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 2%

---

# Servicio OSGi

Un servicio OSGi es una clase Java o interfaz de servicio, junto con una serie de propiedades de servicio como pares nombre/valor. Las propiedades del servicio diferencian entre diferentes proveedores de servicios que proporcionan servicios con la misma interfaz de servicio.

Un servicio OSGi se define semánticamente mediante su interfaz de servicio y se implementa como un objeto de servicio. La funcionalidad de un servicio se define mediante las interfaces que implementa. Por lo tanto, diferentes aplicaciones pueden implementar el mismo servicio. Las interfaces de servicio permiten que los paquetes interactúen enlazando interfaces, no implementaciones. Se debe especificar una interfaz de servicio con el menor número posible de detalles de implementación.

## Definición de la interfaz

Una interfaz sencilla con un método para combinar datos con <span class="x x-first x-last">XDP</span> plantilla.

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## Implementación de la interfaz

Cree un nuevo paquete llamado `com.mysite.samples.impl` para mantener la implementación de la interfaz.

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
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

La anotación `@Component(...)` en la línea 10 marca esta clase Java como un componente OSGi y la registra como un servicio OSGi.

El `@Reference` La anotación forma parte de los servicios declarativos de OSGi y se utiliza para insertar una referencia de la variable [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) en la variable `outputService`.


## Creación e implementación del paquete

* Abrir **ventana del símbolo del sistema**
* Navegue hasta `c:\aemformsbundles\mysite\core`
* Ejecutar el comando `mvn clean install -PautoInstallBundle`
* AEM El comando anterior generará e implementará automáticamente el paquete en la instancia de que se ejecuta en localhost:4502

El paquete también estará disponible en la siguiente ubicación `C:\AEMFormsBundles\mysite\core\target`. AEM El paquete también se puede implementar en la configuración de la aplicación de la manera de usar la aplicación de la manera de [Consola web Felix.](http://localhost:4502/system/console/bundles)

## Usar el servicio

Ahora puede utilizar el servicio en la página JSP. El siguiente fragmento de código muestra cómo obtener acceso al servicio y utilizar los métodos implementados por el servicio

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

El paquete de muestra que contiene la página JSP puede ser [descargado desde aquí](assets/learning_aem_forms.zip)

[El paquete completo está disponible para descargar](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## Prueba del paquete

AEM Importe e instale el paquete en mediante el uso de la opción de configuración de la interfaz de usuario de [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)

Utilice postman para realizar una llamada al POST y proporcionar los parámetros de entrada como se muestra en la captura de pantalla siguiente
![cartero](assets/test-service-postman.JPG)

## Pasos siguientes

[Crear servlet de Sling](./create-servlet.md)

