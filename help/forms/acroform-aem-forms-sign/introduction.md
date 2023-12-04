---
title: Acroforms con AEM Forms
description: Un tutorial que explica la creación de un formulario adaptable mediante AcroForm y la combinación de los datos para obtener un PDF. El PDF con los datos combinados se puede enviar para su firma con Acrobat Sign.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 69
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Crear Forms adaptable a partir de AcroForms

Las organizaciones tienen una amplia variedad de formularios. Algunos de estos formularios se crean en Microsoft Word y se convierten en PDF. De forma predeterminada, estos formularios no se pueden rellenar con Adobe Reader ni Acrobat. Para que estos formularios se puedan rellenar con Acrobat o Reader, debemos convertirlos en AcroForm. Los acroforms son formularios creados con Acrobat. Este artículo explica cómo crear un formulario adaptable desde AcroForm y volver a combinar los datos en AcroForm para obtener el PDF. El PDF con los datos combinados también se puede enviar para su firma con Acrobat Sign.

>[!NOTE]
>
>Si utiliza AEM Forms 6.5, utilice la capacidad de Automated forms conversion.

## Requisitos previos

* AEM Forms 6.3 o 6.4 instalado y configurado
* Acceso a Adobe Acrobat
* AEM Familiaridad con la tecnología de la información y la AEM Forms.

### Para que esta capacidad funcione en su sistema, es necesario lo siguiente

* Descargue e implemente los paquetes mediante [Consola web Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DesarrollarConUsuarioDeServicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [AEM Descargue e importe este paquete en el servidor de correo](assets/acro-form-aem-form.zip). Este paquete contiene el flujo de trabajo de ejemplo y la página html para crear XSD a partir de un formulario
* Abra el [configMgr](http://localhost:4502/system/console/configMgr)
   * Busque &quot;Servicio de asignador de usuarios del servicio Apache Sling&quot; y haga clic para abrir las propiedades
   * Haga clic en `+` icono (más) para añadir la siguiente asignación de servicio
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Haga clic en &quot;Guardar&quot;
