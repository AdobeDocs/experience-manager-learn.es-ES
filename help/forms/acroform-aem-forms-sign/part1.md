---
title: Se adapta a AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 1 de la integración de Acroforms con AEM Forms. Creación de un formulario adaptable mediante Acrobat y combinación de los datos para obtener un PDF.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# Creación de una plataforma

Los formularios son formularios creados con Acrobat. Puede crear un nuevo formulario desde cero con Acrobat o tomar un formulario existente creado en Microsoft Word y convertirlo en Acrobat mediante Acrobat. Se deben seguir los siguientes pasos para convertir un formulario creado en Microsoft Word a Acrobat.

* Abrir un documento de palabras con Acrobat
* Utilice la herramienta Acrobat Prepare form para identificar los campos del formulario.
* Guarde el pdf. Asegúrese de que el nombre del archivo no contenga espacios.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Si desea enviar el formulario que se puede rellenar para firmar con Adobe Sign, asigne un nombre a los campos correspondientes. Por ejemplo, puede asignar un nombre a un campo **Sig_es_:signer1:signature**. Esta es la sintaxis que entiende Adobe Sign.

>[!NOTE]
>
>Si va a enviar un documento basado en XFA, debe acoplar el documento y las etiquetas de firma de Adobe Sign deben estar presentes como texto estático en el documento.

[Documento de etiquetas de texto de Adobe Sign](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Asegúrese de que el nombre de archivo de la forma no tenga espacios. El código de muestra actual no gestiona espacios.
>
>Los nombres de los campos del formulario solo pueden contener lo siguiente:
>
>* espacio único
>* guión bajo único
>* Caracteres alfanuméricos

