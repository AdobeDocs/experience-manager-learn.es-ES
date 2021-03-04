---
title: Desarrollo con Output y Forms Services en AEM Forms
seo-title: Desarrollo con Output y Forms Services en AEM Forms
description: Uso de la API de Output y Forms Service en AEM Forms
seo-description: Uso de la API de Output y Forms Service en AEM Forms
feature: Servicio de Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 1%

---


# Representación de PDF interactivo mediante Forms Services en AEM Forms

Uso de la API del servicio de formularios en AEM Forms para procesar un PDF interactivo

En este artículo echaremos un vistazo al siguiente servicio

* FormsService : Se trata de un servicio muy versátil que le permite exportar e importar datos desde y hacia un archivo PDF, así como generar pdf interactivo al combinar datos xml en una plantilla xdp

El javadoc oficial para la API de AEM Forms se enumera [aquí](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

El siguiente fragmento de código procesa pdf interactivo utilizando la operación renderPDFForm de FormsService. El schengen.xdp es una plantilla que se utiliza para combinar los datos xml.

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

Línea 1: Ubicación de la carpeta que contiene la plantilla xdp

Línea 2-4: Crear PDFFormRenderOptions y establecer sus propiedades

Línea 7: Generación de PDF interactivo mediante la operación del servicio renderPDFForm de FormsService

Línea 11: Devuelve el pdf interactivo generado a la aplicación que realiza la llamada

**Para probar el paquete de muestra en el sistema**
1. [Descargue e instale el paquete de muestra de DocumentServices mediante la Consola Web Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Descargue e instale el paquete mediante el gestor de paquetes AEM](assets/downloadinteractivepdffrommobileform.zip)



1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el filtro CSRF de Adobe Granite
1. Añada la siguiente ruta en las secciones excluidas y guarde
1. /bin/generateinteractivepdf
1. [Abrir el formulario móvil](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Complete un par de campos y luego haga clic en ***Download and fill ....*** botón
1. El pdf interactivo debe descargarse en el sistema local


El paquete de ejemplo contiene el perfil personalizado asociado al formulario móvil. Explore el archivo [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Este jsp extrae los datos del formulario móvil y realiza una solicitud POST al servlet montada en la ruta ***/bin/generateinteractivepdf***. El servlet devuelve el pdf interactivo a la aplicación que realiza la llamada. El código de customtoolbar.jsp descarga el archivo en el sistema local


