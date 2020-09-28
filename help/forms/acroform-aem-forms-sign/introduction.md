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
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# Creación de Forms adaptable a partir de formularios

Las organizaciones tienen una amplia variedad de formularios. Algunos de estos formularios se crean en Microsoft Word y se convierten en PDF. De forma predeterminada, estos formularios no se pueden rellenar con Adobe Reader o Acrobat. Para que estos formularios se puedan rellenar con Acrobat o Reader, es necesario convertirlos en formularios de Acrobat. Las formas son formularios creados con Acrobat. En este artículo se explica cómo crear un formulario adaptable a partir de Acrobat y cómo volver a combinar los datos en Acrobat para obtener el PDF. El archivo PDF con los datos combinados también se puede enviar para firmar con Adobe Sign.

>[!NOTE]
>
>Si utiliza AEM Forms 6.5, utilice la función Conversión automatizada de Forms.

## Requisitos previos

* AEM Forms 6.3 o 6.4 instalado y configurado
* Acceso a Adobe Acrobat
* Familiaridad con AEM/AEM Forms.

### Para que esta capacidad funcione en el sistema, es necesario lo siguiente

* Descargar e implementar los paquetes mediante la consola web [Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Descargue e importe este paquete en AEM](assets/acro-form-aem-form.zip). Este paquete contiene el flujo de trabajo de muestra y la página html para crear XSD a partir de acroform
* Abra [configMgr](http://localhost:4502/system/console/configMgr)
   * Busque &#39;Apache Sling Service User Mapper Service&#39; y haga clic para abrir las propiedades
   * Haga clic en el `+` icono (más) para agregar la siguiente asignación de servicios
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Haga clic en &#39;Guardar&#39;
