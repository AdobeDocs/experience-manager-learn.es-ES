---
title: Desarrollo con Output y Forms Services en AEM Forms
seo-title: Desarrollo con Output y Forms Services en AEM Forms
description: Uso de Output y Forms Service API en AEM Forms
seo-description: Uso de Output y Forms Service API en AEM Forms
feature: forms-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---


# Representación de archivos PDF interactivos con los servicios de Forms en AEM Forms

Uso de Forms Service API en AEM Forms para procesar un PDF interactivo

En este artículo echaremos un vistazo al siguiente servicio

* FormsService: Se trata de un servicio muy versátil que le permite exportar e importar datos desde y en archivos PDF y también generar PDF interactivos combinando datos xml en plantillas xdp

El javadoc oficial para la API de AEM Forms se muestra [aquí](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

El siguiente fragmento de código procesa un PDF interactivo mediante la operación procesePDFForm de FormsService. La plantilla schSchengen.xdp se utiliza para combinar los datos xml.

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

Línea2-4: Cree PDFFormRenderOptions y defina sus propiedades

Línea 7: Generar un PDF interactivo mediante la operación de servicio de renderizarPDForm de FormsService

Línea 11: Devuelve el PDF interactivo generado a la aplicación que realiza la llamada

**Prueba del paquete de muestra en el sistema**
1. [Descargar e instalar el paquete de muestra de DocumentServices mediante la consola web Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Descargue e instale el paquete mediante el administrador de paquetes de AEM](assets/downloadinteractivepdffrommobileform.zip)



1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el filtro CSRF de granito de Adobe
1. Añada la siguiente ruta en las secciones excluidas y guarde
1. /bin/generateinteractivepdf
1. [Abrir el formulario móvil](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Rellene un par de campos y luego haga clic en ***Descargar y rellenar ....*** botón
1. El PDF interactivo debe descargarse en el sistema local


El paquete de ejemplo contiene el perfil personalizado que está asociado con el formulario móvil. Explore el archivo [custom toolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Este jsp extrae los datos del formulario móvil y realiza una solicitud de POST para el servlet montado en la ruta ***/bin/generateinteractivepdf***. El servlet devuelve el PDF interactivo a la aplicación que llama. El código de custom toolbar.jsp descarga el archivo en el sistema local


