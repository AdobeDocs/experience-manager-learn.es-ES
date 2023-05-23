---
title: Uso de PDFG en AEM Forms
description: Demostración de la capacidad de arrastrar y soltar para crear un PDF con AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 2%

---

# Uso de PDFG en AEM Forms{#using-pdfg-in-aem-forms}

Demostración de la capacidad de arrastrar y soltar para crear un PDF con AEM Forms

PDFG significa Generación de PDF. Esto significa que puede convertir una variedad de formatos de archivo en PDF. Los más comunes son los documentos de Microsoft Office. PDFG forma parte de AEM Forms desde la versión 6.1.
[El javadoc para la API de PDFG se enumera aquí](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

Los recursos asociados con este artículo le permiten arrastrar y soltar documentos de MS Office o archivos de JPG en el área de colocación de la página de HTML. Una vez colocado el documento, se invoca el servicio PDFG, se convierte el documento en PDF AEM y se guarda en el sistema de archivos de Servidor de.

Para instalar los recursos de demostración, realice los siguientes pasos

1. Configure PDFG como se menciona en este documento [aquí](https://helpx.adobe.com/es/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Siga la documentación adecuada relacionada con su versión de AEM Forms.
1. [Importe e instale los recursos relacionados con este artículo mediante el administrador de paquetes.](assets/createpdfgdemov2.zip)
1. [Vaya a post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) en su CRX
1. Cambie la ubicación de guardado según sus preferencias (línea 9)
1. Guarde los cambios.
1. Abra el [página html](http://localhost:4502/content/DocumentServices/CreatePDFG.html) para arrastrar y soltar archivos para la conversión.
1. Coloque un archivo de texto o jpg en la zona de colocación.
1. El documento de entrada se convierte en PDF y se guarda en la misma ubicación especificada en el punto 4.

El siguiente fragmento de código muestra el uso del servicio PDFG para convertir archivos a PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
