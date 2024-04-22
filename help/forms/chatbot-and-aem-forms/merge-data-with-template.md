---
title: Combinar datos con la plantilla XDP
description: Crear un documento de PDF combinando datos con la plantilla
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Combinar datos con la plantilla XDP

El siguiente paso es combinar los datos XML con la plantilla para generar el PDF. A continuación, se envía a este PDF para que firme mediante Adobe Sign.

## Usar OutputService para generar el PDF

El [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) del OutputService se utilizó para generar el PDF.
El PDF generado se envió para firmar utilizando la API de REST de Adobe Sign.

