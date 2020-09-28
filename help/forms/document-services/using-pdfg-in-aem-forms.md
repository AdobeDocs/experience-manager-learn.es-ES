---
title: Uso de PDFG en AEM Forms
seo-title: Uso de PDFG en AEM Forms
description: Demostración de la capacidad de arrastrar y soltar para crear archivos PDF con AEM Forms
seo-description: Demostración de la capacidad de arrastrar y soltar para crear archivos PDF con AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# Uso de PDFG en AEM Forms{#using-pdfg-in-aem-forms}

Demostración de la capacidad de arrastrar y soltar para crear archivos PDF con AEM Forms

PDFG significa Generación de PDF. Esto significa que puede convertir una gran variedad de formatos de archivo a PDF. Los más comunes son los documentos de Microsoft Office. PDFG forma parte de AEM Forms desde la versión 6.1.
[El archivo javadoc para la API de PDFG se muestra aquí](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Los recursos asociados a este artículo le permitirán arrastrar y soltar documentos de MS office o archivo JPG en la zona de colocación de la página HTML. Una vez que el documento se haya descartado, invocará el servicio PDFG, convertirá el documento en PDF y lo guardará en el sistema de archivos de AEM Server.

Para instalar los recursos de demostración, realice los siguientes pasos

1. Configure el PDFG como se indica en este documento [aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Siga la documentación adecuada relacionada con su versión de AEM Forms.
1. [Importe e instale recursos relacionados con este artículo mediante el administrador de paquetes.](assets/createpdfgdemov2.zip)
1. [Navegue hasta post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) en su CRX
1. Cambie la ubicación de guardado según sus preferencias (línea 9)
1. Guarde los cambios.
1. Abra la página [](http://localhost:4502/content/DocumentServices/CreatePDFG.html) html para arrastrar y soltar archivos para la conversión.
1. Coloque un archivo de palabra o jpg en la zona de colocación.
1. El documento de entrada se convertirá a PDF y se guardará en la misma ubicación especificada en el punto 4.

El siguiente fragmento de código muestra el uso del servicio PDFG para convertir archivos a PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

