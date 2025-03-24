---
title: Combinar datos con la plantilla XDP
description: Crear un documento de PDF combinando datos con la plantilla
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Combinar datos con la plantilla XDP

El siguiente paso es combinar los datos XML con la plantilla para generar PDF. A continuación, se envía esta PDF para que la firme mediante Adobe Sign.

## Usar OutputService para generar PDF

El método [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) de OutputService se utilizó para generar PDF.
La PDF generada se envió para firmar utilizando la API de REST de Adobe Sign.
