---
title: Servicios de utilidad útiles
description: Algunos servicios útiles para desarrolladores de AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Servicios de utilidad útiles

Este paquete de ejemplo proporciona servicios útiles que un desarrollador de AEM Forms puede utilizar. Los siguientes servicios están disponibles.


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

El siguiente es el código que se utilizó en la página JSP para crear org.w3c.dom.Document a partir de una cadena, convertir el documento y almacenarlo en el repositorio de CRX, como se muestra en el siguiente fragmento de código.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Requisitos previos


Deberá implementar [DesarrollandoConServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e iniciar el paquete.


Si va a guardar documentos en el repositorio de CRX usando este servicio de utilidad, siga el [artículo del usuario de desarrollo con servicio](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Asegúrese de proporcionar los [permisos necesarios](http://localhost:4502/useradmin) en las carpetas de CRX apropiadas al usuario del servicio FD.
