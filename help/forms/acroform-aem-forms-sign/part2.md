---
title: Acroforms con AEM Forms
description: Parte 2 de la integración de Acroforms con AEM Forms. Cree un esquema a partir de un AcroForm.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 34
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---


# Crear esquema a partir de acroform

El siguiente paso es crear un esquema a partir del AcroForm creado en el paso anterior. Se proporciona una aplicación de ejemplo para crear el esquema como parte de este tutorial. Para crear el esquema, siga las siguientes instrucciones:

1. Inicie sesión en [CRXDE Lite](http://localhost:4502/crx/de)
2. Abrir en el archivo `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Cambie el `saveLocation` a una carpeta adecuada del disco duro. Asegúrese de que la carpeta en la que está guardando ya se ha creado.
4. Dirija el explorador a [Crear XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) AEM página alojada en el servidor de correo electrónico de.
5. Arrastre y suelte el AcroForm.
6. Compruebe la carpeta especificada en el paso 3. El archivo de esquema se guarda en esta ubicación.

## Cargar el AcroForm

Para que esta demostración funcione en su sistema, debe crear una carpeta llamada `acroforms` en AEM Assets. Cargue AcroForm en esta `acroforms` carpeta.

>[!NOTE]
>
>El código de ejemplo busca el acroForm en esta carpeta. AcroForm es necesario para combinar los datos enviados del formulario adaptable.
