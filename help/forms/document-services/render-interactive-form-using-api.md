---
title: Representar PDF interactivo con los servicios de Forms en AEM Forms
description: Uso de la API de servicio de Forms en AEM Forms para procesar PDF interactivo
feature: Forms Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Representar PDF interactivo con los servicios de Forms en AEM Forms

Uso de la API de servicio de Forms en AEM Forms para procesar PDF interactivo

En este artículo echaremos un vistazo al siguiente servicio

* FormsService: se trata de un servicio muy versátil que le permite exportar e importar datos desde y hacia archivos PDF, así como generar PDF interactivos combinando datos xml en plantillas xdp

El [javadoc oficial para la API de AEM Forms se enumera aquí](https://helpx.adobe.com/es/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

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

Línea 7: Generar PDF interactivo mediante la operación del servicio renderPDFForm de FormsService

Línea 11: Devuelve el PDF interactivo generado a la aplicación que realiza la llamada

**Para probar el paquete de muestra en el sistema**
1. [Descargar e instalar el paquete DevelopersWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Descargue e instale el paquete de ejemplo DocumentServices mediante la consola web Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Descargue e instale el paquete mediante el administrador de paquetes de AEM](assets/downloadinteractivepdffrommobileform.zip)

1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Búsqueda del filtro CSRF de Adobe Granite
1. Añada la siguiente ruta en las secciones excluidas y guarde
1. /bin/generateinteractivepdf
1. Busque _Servicio de asignador de usuarios del servicio Apache Sling_ y haga clic para abrir las propiedades
   1. Haga clic en el icono *+* (más) para agregar la siguiente asignación de servicio
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Haga clic en Guardar
1. [Abrir el formulario móvil](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Rellene un par de campos y, a continuación, haga clic en el .... ***Descargar y rellenar*** botón
1. El PDF interactivo debe descargarse en el sistema local


El paquete de ejemplo contiene el perfil personalizado asociado al formulario móvil. Explore el archivo [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Este jsp extrae los datos del formulario móvil y realiza una petición POST al servlet montado en la ruta ***/bin/generateinteractivepdf***. El servlet devuelve el PDF interactivo a la aplicación que realiza la llamada. A continuación, el código de customtoolbar.jsp descarga el archivo en el sistema local
