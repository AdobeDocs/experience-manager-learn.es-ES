---
title: Servicios de utilidad
description: Algunos servicios útiles para el desarrollador de AEM Forms
feature: Formularios adaptables
version: 6.4,6.5
topic: Desarrollo
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 2%

---


# Servicios de utilidad

Este paquete de ejemplo proporciona servicios útiles de utilidad que un desarrollador de AEM Forms puede usar. Los siguientes servicios están disponibles.


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

El paquete de muestra se puede [descargar desde aquí](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Código de ejemplo para utilizar los servicios de utilidad

El siguiente es el código que se utilizó en la página JSP para crear org.w3c.dom.Document a partir de una cadena y convertir el documento y almacenarlo en el repositorio CRX como se muestra en el siguiente fragmento de código.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Requisitos previos


Deberá implementar [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e iniciar el paquete.


Si va a guardar documentos en el repositorio CRX usando este servicio de utilidad, siga el artículo [desarrollo con el usuario del servicio](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Asegúrese de proporcionar los [permisos requeridos](http://localhost:4502/useradmin) en las carpetas CRX correspondientes al usuario del servicio fd.

