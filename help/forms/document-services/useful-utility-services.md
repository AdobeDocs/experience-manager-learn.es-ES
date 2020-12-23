---
title: Servicios de utilidad
description: Algunos servicios útiles para desarrolladores de AEM Forms
feature: document-services
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 243e26e2403e3d7816a0dd024ffbe73743827c7c
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%

---


# Servicios de utilidad

Este paquete de muestra proporciona servicios de utilidad útiles que puede utilizar un desarrollador de AEM Forms. Los siguientes servicios están disponibles.


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

## Código de muestra para utilizar los servicios de utilidad

El siguiente es el código que se utilizó en la página JSP para crear org.w3c.dom.Documento a partir de una cadena y convertir el documento y almacenarlo en el repositorio de CRX, como se muestra en el siguiente fragmento de código.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Requisitos previos


Deberá implementar [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) y inicio del paquete.


Si va a guardar documentos en el repositorio de CRX mediante este servicio de utilidad, siga el [desarrollo con el artículo del usuario del servicio](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Asegúrese de proporcionar al usuario de fd-service los [permisos requeridos](http://localhost:4502/useradmin) en las carpetas CRX correspondientes.

