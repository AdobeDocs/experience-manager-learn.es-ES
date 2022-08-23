---
title: Se adapta a AEM Forms
description: Un tutorial que recorre la creación de un formulario adaptable mediante Acrobat y la combinación de los datos para obtener un PDF. El PDF con los datos combinados se puede enviar para su firma mediante Adobe Sign.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 2%

---


# Creación de Forms adaptable a partir de formularios

Las organizaciones tienen una amplia variedad de formas. Algunos de estos formularios se crean en Microsoft Word y se convierten en PDF. De forma predeterminada, estos formularios no se pueden rellenar con Adobe Reader o Acrobat. Para que estos formularios se puedan rellenar con Acrobat o Reader, es necesario convertir estos formularios en Acrobat. Los formularios son formularios creados con Acrobat. Este artículo explica cómo crear un formulario adaptable a partir de Acrobat y cómo volver a combinar los datos en Acrobat para obtener el PDF. El PDF con los datos combinados también se puede enviar para firmar con Adobe Sign.

>[!NOTE]
>
>Si utiliza AEM Forms 6.5, utilice la función de Automated forms conversion.

## Requisitos previos

* AEM Forms 6.3 o 6.4 instalado y configurado
* Acceso a Adobe Acrobat
* Familiaridad con AEM/AEM Forms.

### Para que esta capacidad funcione en su sistema, es necesario lo siguiente

* Descargue e implemente los paquetes utilizando la variable [Consola Web Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Descargue e importe este paquete en AEM](assets/acro-form-aem-form.zip). Este paquete contiene el flujo de trabajo de ejemplo y la página html para crear XSD a partir de acroform
* Abra el [configMgr](http://localhost:4502/system/console/configMgr)
   * Busque el servicio Apache Sling Service User Mapper y haga clic en para abrir las propiedades
   * Haga clic en el `+` icono (más) para añadir la siguiente asignación de servicios
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Haga clic en &quot;Guardar&quot;
