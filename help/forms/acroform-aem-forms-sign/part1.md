---
title: Se ajusta a AEM Forms
seo-title: Combinar datos de formulario adaptable con Acroform
description: Parte 1 de la integración de Acroforms con AEM Forms. Creación de un formulario adaptable con Acrobat y combinación de los datos para obtener un PDF.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---


# Creación de Acrobat

Las formas son formularios creados con Acrobat. Puede crear un nuevo formulario desde cero con Acrobat o bien, tomar un formulario existente creado en Microsoft Word y convertirlo en Acrobat con Acrobat. Se deben seguir los siguientes pasos para convertir un formulario creado en Microsoft Word a Acroform.

* Abrir documento de palabras con Acrobat
* Utilice la herramienta de preparación de formularios de Acrobat para identificar los campos del formulario.
* Guarde el PDF. Asegúrese de que el nombre del archivo no tenga espacios.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Si desea enviar el formulario rellenable para firmar con Adobe Sign, asigne un nombre a los campos según corresponda. Por ejemplo, puede asignar un nombre a un campo **Sig_es_:signer1:signature**. Esta es la sintaxis que Adobe Sign entiende.

>[!NOTE]
>
>Si va a enviar un documento basado en XFA, debe acoplar el documento y las etiquetas de firma Adobe Sign deben estar presentes como texto estático en el documento.

[documento de etiquetas de texto de Adobe Sign](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Asegúrese de que el nombre de archivo de la forma de la pantalla no tenga espacios. El código de muestra actual no gestiona espacios.
>
>Los nombres de campo de formulario solo pueden contener lo siguiente:
>
>* espacio único
>* subrayado único
>* Caracteres alfanuméricos

