---
title: Uso del componente Tabla en el documento de canal de impresión de AEM Forms
seo-title: Uso del componente Tabla en el documento de canal de impresión de AEM Forms
description: El siguiente vídeo muestra los pasos necesarios para utilizar el componente de tabla en comunicaciones interactivas para imprimir documentos de canal.
feature: Interactive Communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 2%

---


# Uso del componente Tabla en el documento de canal de impresión de AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

El siguiente vídeo muestra los pasos necesarios para utilizar el componente de tabla en comunicaciones interactivas para imprimir documentos de canal.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Las tablas se utilizan para mostrar los datos de forma tabular. Las filas de la tabla deben crecer o reducirse según los datos devueltos por el origen de datos. Para utilizar una tabla en un documento de canal de impresión, es necesario crear un archivo de diseño (archivo xdp) mediante AEM Forms Designer. En este archivo de diseño, se añade la tabla con el número necesario de columnas. Asegúrese de que el tipo de objeto de campo de columna sea TextField o Numeric Field según sus necesidades. Para cada una de las columnas, los campos se aseguran de que el enlace de datos esté establecido en Usar nombre.

>[!NOTE]
>
>Para que la tabla sea dinámica, asegúrese de que ha marcado la fila como repetida.

**Pruébelo en su propio servidor**

* [Descargue y descomprima el archivo de recursos en su disco duro](assets/usingtablesinprintchannel.zip)

* Importar los dos archivos zip en AEM mediante el administrador de paquetes

* En los recursos asociados con este artículo se incluyen los siguientes:

   * Fragmento de diseño

   * Modelo de datos del formulario

   * Documento de comunicación interactiva
   * sampleretirementaccountdata.json

* Abra el documento de comunicación interactiva en [modo de edición](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Agregue el fragmento de diseño TableDemo a la sección de contribuciones.
* Enlace las celdas de la tabla a los elementos correspondientes del Modelo de datos de formulario, como se muestra en el vídeo

* Vista previa del documento de comunicación interactiva con el archivo de datos json de ejemplo que se le ha proporcionado

