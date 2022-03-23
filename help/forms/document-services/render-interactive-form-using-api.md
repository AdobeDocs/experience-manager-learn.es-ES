---
title: Renderización de un PDF interactivo mediante Forms Services en AEM Forms
description: Uso de la API del servicio de Forms en AEM Forms para procesar un PDF interactivo
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---

# Renderización de un PDF interactivo mediante Forms Services en AEM Forms

Uso de la API del servicio de Forms en AEM Forms para procesar un PDF interactivo

En este artículo echaremos un vistazo al siguiente servicio

* FormsService : Se trata de un servicio muy versátil que le permite exportar e importar datos desde y hacia un archivo PDF, así como generar pdf interactivo al combinar datos xml en una plantilla xdp

Se muestra el javadoc oficial para la API de AEM Forms [here](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

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

Línea 7: Generación de un PDF interactivo mediante la operación del servicio renderPDFForm de FormsService

Línea 11: Devuelve el pdf interactivo generado a la aplicación que realiza la llamada

**Para probar el paquete de muestra en el sistema**
1. [Descargue e instale el paquete de muestra de DocumentServices mediante la Consola Web Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Descargue e instale el paquete mediante el administrador de paquetes de AEM](assets/downloadinteractivepdffrommobileform.zip)



1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el filtro de Adobe Granite CSRF
1. Añada la siguiente ruta en las secciones excluidas y guarde
1. /bin/generateinteractivepdf
1. [Abrir el formulario móvil](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Complete un par de campos y luego haga clic en el ***Descargar y rellenar ....*** botón
1. El pdf interactivo debe descargarse en el sistema local


El paquete de ejemplo contiene el perfil personalizado asociado al formulario de Mobile. Explore el [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) archivo. Este jsp extrae los datos del formulario móvil y realiza una solicitud de POST para el servlet montada en ***/bin/generateinteractivepdf*** ruta. El servlet devuelve el pdf interactivo a la aplicación que realiza la llamada. El código de customtoolbar.jsp descarga el archivo en el sistema local
