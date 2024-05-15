---
title: Representar un PDF interactivo con los servicios de Forms en AEM Forms
description: Uso de la API de servicio de Forms en AEM Forms para procesar el PDF interactivo
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 75
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Representar un PDF interactivo con los servicios de Forms en AEM Forms

Uso de la API de servicio de Forms en AEM Forms para procesar el PDF interactivo

En este artículo echaremos un vistazo al siguiente servicio

* FormsService: se trata de un servicio muy versátil que le permite exportar e importar datos desde y hacia archivos de PDF, así como generar PDF interactivos combinando datos xml en plantillas xdp

El funcionario [la API de javadoc para AEM Forms se enumera aquí](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

El siguiente fragmento de código procesa el PDF interactivo mediante la operación renderPDFForm de FormsService. El archivo schengen.xdp es una plantilla que se utiliza para combinar los datos xml.

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

Línea 1: ubicación de la carpeta que contiene la plantilla xdp

Línea2-4: Crear PDFFormRenderOptions y establecer sus propiedades

Línea 7: Generar un PDF interactivo mediante la operación del servicio renderPDFForm de FormsService

Línea 11: Devuelve el PDF interactivo generado a la aplicación que realiza la llamada

**Para probar el paquete de muestra en el sistema**
1. [Descargar e instalar el paquete DevelopersWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Descargue e instale el paquete de ejemplo DocumentServices mediante la consola web Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [AEM Descargue e instale el paquete mediante el administrador de paquetes de la](assets/downloadinteractivepdffrommobileform.zip)

1. [Inicie sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Búsqueda de Adobe Granite CSRF Filter
1. Añada la siguiente ruta en las secciones excluidas y guarde
1. /bin/generateinteractivepdf
1. Buscar por _Servicio de asignador de usuarios del servicio Apache Sling_ y haga clic en para abrir las propiedades
   1. Haga clic en *+* icono (más) para añadir la siguiente asignación de servicio
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Haga clic en Guardar
1. [Abra el formulario móvil](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Rellene un par de campos y haga clic en el botón ***Descargar y rellenar ....*** botón
1. El PDF interactivo debe descargarse en el sistema local


El paquete de ejemplo contiene el perfil personalizado asociado al formulario móvil. Explore la [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) archivo. Este jsp extrae los datos del formulario móvil y realiza una solicitud de POST al servlet montado en ***/bin/generateinteractivepdf*** ruta. El servlet devuelve el PDF interactivo a la aplicación que realiza la llamada. A continuación, el código de customtoolbar.jsp descarga el archivo en el sistema local
