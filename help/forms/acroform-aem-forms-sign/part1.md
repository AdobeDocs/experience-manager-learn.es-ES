---
title: Acroforms con AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 1 de la integración de Acroforms con AEM Forms. Crear un formulario adaptable mediante AcroForm y combinar los datos para obtener un PDF.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 2%

---


# Creación de AcroForm

Los acroforms son formularios creados con Acrobat. Puede crear un nuevo formulario desde cero mediante Acrobat o tomar un formulario existente creado en Microsoft Word y convertirlo en AcroForm mediante Acrobat. Es necesario seguir los siguientes pasos para convertir un formulario creado en Microsoft Word a AcroForm.

* Abrir documento de Word con Acrobat
* Utilice la herramienta Preparar formulario de Acrobat para identificar los campos del formulario.
* Guarde el PDF. Asegúrese de que el nombre del archivo no contenga espacios.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>Si desea enviar el acroformulario rellenable para firmar mediante Acrobat Sign, asigne un nombre a los campos según corresponda. Por ejemplo, puede asignar un nombre a un campo **Sig_es_:signer1:firma**. Esta es la sintaxis que Acrobat Sign entiende.

>[!NOTE]
>
>Si está enviando un documento basado en XFA, debe acoplar el documento y las etiquetas de firma de Acrobat Sign deben estar presentes como texto estático en el documento.

[Documento de etiquetas de texto de Acrobat Sign](https://helpx.adobe.com/es/sign/using/text-tag.html)

>[!NOTE]
>
>Asegúrese de que el nombre del archivo de AcroForm no contenga espacios. El código de ejemplo actual no controla espacios.
>
>Los nombres de los campos de formulario solo pueden contener lo siguiente:
>
>* espacio único
>* guion bajo único
>* caracteres alfanuméricos

