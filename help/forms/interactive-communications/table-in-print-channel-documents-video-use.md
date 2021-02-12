---
title: Uso del componente Tabla en el Documento del Canal de impresión de AEM Forms
seo-title: Uso del componente Tabla en el Documento del Canal de impresión de AEM Forms
description: El siguiente vídeo recorre los pasos necesarios para utilizar el componente de tabla en Interactive Communications para documentos de canal de impresión.
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Uso del componente Tabla en el Documento del Canal de impresión de AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

El siguiente vídeo recorre los pasos necesarios para utilizar el componente de tabla en Interactive Communications para documentos de canal de impresión.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Las tablas se utilizan para mostrar los datos de manera tabular. Las filas de la tabla deben crecer o reducirse en función de los datos devueltos por el origen de datos. Para utilizar una tabla en el documento de canal de impresión, es necesario crear un archivo de diseño (archivo xdp) con AEM Forms Designer. En este archivo de diseño, agregamos la tabla con el número necesario de columnas. Asegúrese de que el tipo de objeto de campo de columna sea TextField o Numeric Field según sus necesidades. Para cada columna, los campos se aseguran de que el enlace de datos está establecido en Usar nombre.

>[!NOTE]
>
>Para que la tabla sea dinámica, asegúrese de que ha marcado la fila como repetida.

**Pruébelo en su propio servidor**

* [Descargue y descomprima el archivo de recursos en su disco duro](assets/usingtablesinprintchannel.zip)

* Importar los dos archivos zip en AEM mediante el administrador de paquetes

* Los recursos asociados con este artículo se incluyen en los siguientes:

   * Fragmento de diseño

   * Modelo de datos de formulario

   * Documento de comunicación interactiva
   * sampleretirementaccountdata.json

* Abra el Documento de comunicación interactiva en [modo de edición](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Añada el fragmento de diseño TableDemo a la sección de contribuciones.
* Enlace las celdas de la tabla a los elementos correspondientes del Modelo de datos de formulario, como se muestra en el vídeo

* Previsualización Documento de comunicación interactiva con el archivo de datos json de ejemplo que se le ha proporcionado

