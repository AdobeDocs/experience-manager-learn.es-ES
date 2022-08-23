---
title: Se adapta a AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 2 de la integración de Acroforms con AEM Forms. Cree un esquema a partir de una Acroform.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 2%

---


# Crear esquema a partir del formulario

El siguiente paso es crear un esquema a partir de la Acrobat creada en el paso anterior. Se proporciona una aplicación de ejemplo para crear el esquema como parte de este tutorial. Para crear el esquema, siga las siguientes instrucciones:

1. Inicie sesión en [CRXDE Lite](http://localhost:4502/crx/de)
2. Abrir en el archivo `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Cambie el `saveLocation` a una carpeta adecuada de su disco duro. Asegúrese de que la carpeta en la que está guardando ya esté creada.
4. Especifique el explorador para [Crear XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) página alojada en AEM.
5. Arrastre y suelte la Acroform.
6. Compruebe la carpeta especificada en el paso 3. El archivo de esquema se guarda en esta ubicación.

## Cargar el formulario

Para que esta demostración funcione en su sistema, debe crear una carpeta llamada `acroforms` en AEM Assets. Cargue el formulario en esta `acroforms` carpeta.

>[!NOTE]
>
>El código de ejemplo busca el formulario en esta carpeta. Se necesita el formulario para combinar los datos enviados del formulario adaptable.
