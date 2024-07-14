---
title: Combinar datos con la plantilla XDP
description: Crear un documento de PDF combinando datos con la plantilla
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Combinar datos con la plantilla XDP

El siguiente paso es combinar los datos XML con la plantilla para generar el PDF. A continuación, se envía a este PDF para que firme mediante Adobe Sign.

## Usar OutputService para generar el PDF

Se usó el método [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) de OutputService para generar el PDF.
El PDF generado se envió para firmar utilizando la API de REST de Adobe Sign.
