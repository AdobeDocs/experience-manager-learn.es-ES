---
title: Se ajusta a AEM Forms
seo-title: Combinar datos de formulario adaptable con Acroform
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---


# Crear Esquema a partir del formulario

El siguiente paso es crear un esquema a partir de la forma de la Acrobat creada en el paso anterior. Se proporciona una aplicación de ejemplo para crear el esquema como parte de este tutorial. Para crear el esquema, siga las instrucciones siguientes:

1. Iniciar sesión en el [CRXDE Lite](http://localhost:4502/crx/de)
2. Abrir en el archivo `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Cambie el `saveLocation` a una carpeta adecuada en el disco duro. Asegúrese de que la carpeta en la que está guardando ya está creada.
4. Seleccione el navegador para [crear una página XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) alojada en AEM.
5. Arrastre y suelte la Acroform.
6. Compruebe la carpeta especificada en el paso 3. El archivo esquema se guarda en esta ubicación.

## Cargar el formulario

Para que esta demostración funcione en su sistema, deberá crear una carpeta llamada `acroforms` en AEM Assets. Cargue Acrobat en esta `acroforms` carpeta.

>[!NOTE]
>
>El código de ejemplo busca el formulario en esta carpeta. El formulario es necesario para combinar los datos enviados del formulario adaptable.
