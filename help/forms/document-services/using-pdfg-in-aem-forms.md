---
title: Uso de PDFG en AEM Forms
seo-title: Uso de PDFG en AEM Forms
description: Demostración de la capacidad de arrastrar y soltar para crear PDF con AEM Forms
seo-description: Demostración de la capacidad de arrastrar y soltar para crear PDF con AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 2%

---


# Uso de PDFG en AEM Forms{#using-pdfg-in-aem-forms}

Demostración de la capacidad de arrastrar y soltar para crear PDF con AEM Forms

PDFG significa Generación de PDF. Esto significa que puede convertir una gran variedad de formatos de archivo a PDF. Los más comunes son documentos de Microsoft Office. PDFG forma parte de AEM Forms desde la versión 6.1.
[El javadoc para la API PDFG está listado aquí](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Los recursos asociados con este artículo le permitirán arrastrar y soltar documentos de MS office o archivos JPG en la zona de colocación de la página HTML. Una vez que el documento se haya colocado, invocará el servicio PDFG, convertirá el documento a PDF y lo guardará en el sistema de archivos de AEM Server.

Para instalar los recursos de demostración, realice los siguientes pasos

1. Configure el PDFG como se menciona en este documento [aquí](https://helpx.adobe.com/es/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Siga la documentación apropiada relacionada con su versión de AEM Forms.
1. [Importe e instale recursos relacionados con este artículo mediante el administrador de paquetes.](assets/createpdfgdemov2.zip)
1. [Vaya a post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin your CRX
1. Cambie la ubicación de guardado según sus preferencias (línea 9)
1. Guarde los cambios.
1. Abra la [ página html](http://localhost:4502/content/DocumentServices/CreatePDFG.html) para arrastrar y soltar archivos para la conversión.
1. Coloque un archivo de palabra o jpg en la zona de colocación.
1. El documento de entrada se convertirá a PDF y se guardará en la misma ubicación especificada en el punto 4.

El siguiente fragmento de código muestra el uso del servicio PDFG para convertir archivos a PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

