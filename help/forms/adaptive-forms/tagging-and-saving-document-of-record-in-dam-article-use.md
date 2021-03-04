---
title: Etiquetado y almacenamiento de AEM Forms DoR en DAM
seo-title: Etiquetado y almacenamiento de AEM Forms DoR en DAM
description: Este artículo tratará el caso de uso de almacenar y etiquetar el DoR generado por AEM Forms en AEM DAM. El etiquetado del documento se realiza en función de los datos de formulario enviados.
seo-description: Este artículo tratará el caso de uso de almacenar y etiquetar el DoR generado por AEM Forms en AEM DAM. El etiquetado del documento se realiza en función de los datos de formulario enviados.
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: Formularios adaptables,Flujo de trabajo
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---


# Etiquetado y almacenamiento de AEM Forms DoR en DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Este artículo tratará el caso de uso de almacenar y etiquetar el DoR generado por AEM Forms en AEM DAM. El etiquetado del documento se realiza en función de los datos de formulario enviados.

Una solicitud habitual de los clientes es almacenar y etiquetar el documento de registro (DoR) generado por AEM Forms en AEM DAM. El etiquetado del documento debe basarse en los datos enviados por los formularios adaptables. Por ejemplo, si el estado de empleo en los datos enviados es &quot;Retirado&quot;, queremos etiquetar el documento con la etiqueta &quot;Retirado&quot; y almacenarlo en DAM.

El caso de uso es el siguiente:

* Un usuario rellena el formulario adaptable. En la forma adaptativa, se captura el estado civil (ex Single) del usuario y el estado laboral (Ex Retirado).
* Al enviar el formulario, se activa un flujo de trabajo de AEM. Este flujo de trabajo etiqueta el documento con el estado civil (único) y el estado de empleo (retirado) y almacena el documento en DAM.
* Una vez que el documento está almacenado en DAM, el administrador debe poder buscar en el documento mediante estas etiquetas. Por ejemplo, la búsqueda en Único o Retirado obtendría los documentos de referencia adecuados.

Para satisfacer este caso de uso, se escribió un paso de proceso personalizado. En este paso, recuperamos los valores de los elementos de datos adecuados de los datos enviados. A continuación, construimos el mosaico de la etiqueta con este valor. Por ejemplo, si el valor del elemento de estado civil es &quot;Single&quot;, el título de la etiqueta se convierte en **Peak:EmployStatus/Single. **Utilizando la API TagManager , encontramos la etiqueta y la aplicamos al DoR.

El siguiente fragmento de código muestra cómo encontrar la etiqueta y aplicarla al documento.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Para que este ejemplo funcione en su sistema, siga los pasos que se indican a continuación:
* [Implementar el paquete de usuario Desarrollo con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que establece las etiquetas de los datos del formulario enviado.

* [Descargar el formulario adaptable de ejemplo](assets/tag-and-store-in-dam-assets.zip)

* [Ir a Formularios y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Haga clic en Crear | Archivo Cargar y cargar el archivo sampleadaptiveform.zip

* [Importe el ](assets/tag-and-store-in-dam-assets.zip) recurso de artículo utilizando el gestor de paquetes AEM
* Abra el [formulario de ejemplo en modo de vista previa](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Complete la sección Personas y envíe el formulario.
* [Vaya a la carpeta Pico de DAM](http://localhost:4502/assets.html/content/dam/Peak). Debería ver DoR en la carpeta Peak. Compruebe las propiedades del documento. Debe etiquetarse adecuadamente.
Felicitaciones!! Ha instalado correctamente el ejemplo en su sistema

* Exploremos el [flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) que se activa al enviar el formulario.
* El primer paso en el flujo de trabajo crea un nombre de archivo único concatenando el nombre del solicitante y el condado de residencia.
* El segundo paso del flujo de trabajo pasa la jerarquía de etiquetas y los elementos de campos de formulario que deben etiquetarse. El paso de proceso extrae el valor de los datos enviados y construye el título de la etiqueta que debe etiquetar el documento.
* Si desea almacenar DoR en una carpeta diferente de DAM, especifique la ubicación de la carpeta utilizando las propiedades de configuración especificadas en la captura de pantalla siguiente.

Los otros dos parámetros son específicos de DoR y Ruta del archivo de datos, tal como se especifica en las opciones de envío del formulario adaptable. Asegúrese de que los valores especificados aquí coincidan con los valores especificados en las opciones de envío del formulario adaptable.

![Puerta de etiqueta](assets/tag_dor_service_configuration.gif)

