---
title: Desarrollo con servicios Output y Forms en AEM Forms
description: Obtenga información sobre el desarrollo con la salida y la API de servicio de Forms en AEM Forms.
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
duration: 122
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 0%

---

# Desarrollo con servicios Output y Forms en AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Obtenga información sobre el desarrollo con la salida y la API de servicio de Forms en AEM Forms.

En este artículo echaremos un vistazo a lo siguiente

* [Servicio de salida](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html): normalmente este servicio se usa para combinar datos xml con la plantilla xdp o el pdf para generar un pdf aplanado.
* [FormsService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html): se trata de un servicio muy versátil que le permite procesar xdp como pdf y exportar/importar datos desde y hacia el archivo PDF.


El siguiente fragmento de código exporta datos desde un archivo de PDF

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

La línea 1 extrae el archivo .pd de la solicitud

La línea 2 extrae saveLocation de la solicitud

La línea 5 se hace con FormsService

La línea 6 exporta los xmlData desde el archivo del PDF

**Para probar el paquete de muestra en el sistema**

[AEM Descargue e instale el paquete mediante el administrador de paquetes de la](assets/using-output-and-form-service-api.zip)




**Después de instalar el paquete, tendrá que lista de permitidos las siguientes direcciones URL en el filtro CSRF de Adobe Granite.**

1. Siga los pasos que se mencionan a continuación para lista de permitidos las rutas mencionadas anteriormente.
1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Búsqueda de Adobe Granite CSRF Filter
1. Añada las 3 rutas siguientes en las secciones excluidas y guarde
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. Busque &quot;Filtro de referente de Sling&quot;
1. Marque la casilla &quot;Permitir vacío&quot;. (Esta configuración solo debe ser para fines de prueba)

## Prueba de las muestras

Existen varias formas de probar el código de ejemplo. La forma más rápida y sencilla de usar una aplicación de Postman es. Postman le permite realizar solicitudes de POST al servidor.

* Instale la aplicación de Postman en su sistema.
* Inicie la aplicación e introduzca la dirección URL adecuada
* Asegúrese de haber seleccionado &quot;POST&quot; en la lista desplegable
* Asegúrese de especificar &quot;Autorización&quot; como &quot;Autenticación básica&quot;. AEM Especifique el nombre de usuario y la contraseña del servidor de
* Especifique los parámetros de solicitud en la pestaña del cuerpo
* Haga clic en el botón Send

El paquete contiene 4 muestras. En los siguientes párrafos se explica cuándo utilizar el servicio de salida o el servicio Forms, la dirección URL del servicio y los parámetros de entrada que espera cada servicio

## Usar OutputService para combinar datos con la plantilla xdp

* Utilice el servicio Output para combinar datos con documentos xdp o pdf para generar PDF aplanados
* **URL DE POST**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parámetros de solicitud -**

   * **xdp_or_pdf_file** : archivo xdp o pdf con el que desea combinar los datos
   * **xmlfile**: archivo de datos xml que se combina con xdp_or_pdf_file
   * **saveLocation**: la ubicación para guardar el documento procesado en el sistema de archivos. Por ejemplo, c:\\documents\\sample.pdf

### Usar la API de FormsService

#### Importar datos

* Utilice FormsService importData para importar datos en un archivo de PDF
* **URL DE POST** - http://localhost:4502/content/AemFormsSamples/mergedata.html

* **Parámetros de solicitud:**

   * **archivo PDF** : El archivo PDF con el que desea combinar los datos
   * **xmlfile**: archivo de datos xml que se combina con el archivo pdf
   * **saveLocation**: la ubicación para guardar el documento procesado en el sistema de archivos. Por ejemplo, `c:\\outputsample.pdf`.

#### Exportar datos

* Utilice la API exportData de FormsService para exportar datos desde el archivo del PDF
* **URL DE POST** - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parámetros de solicitud:**

   * **archivo PDF** : El archivo PDF desde el que desea exportar los datos
   * **saveLocation**: La ubicación para guardar los datos exportados en el sistema de archivos. Por ejemplo, c:\\documents\\export_data.xml

#### Procesar XDP

* Procesar una plantilla XDP como PDF estático/dinámico
* Utilice la API renderPDFForm de FormsService para procesar la plantilla xdp como PDF
* **URL DE POST** - http://localhost:4502/content/AemFormsSamples/renderxdp?xdpName=f1040.xdp
* Parámetro de solicitud:
   * xdpName: nombre del archivo xdp que se procesará como pdf

[Puede importar esta colección de Postman para probar la API](assets/UsingDocumentServicesInAEMForms.postman_collection.json)
