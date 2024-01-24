---
title: Uso del componente Tabla en el documento del canal de impresión de AEM Forms
description: El siguiente vídeo muestra los pasos necesarios para utilizar el componente de tabla en Interactive Communications para documentos del canal de impresión.
feature: Interactive Communication
doc-type: technical video
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
duration: 285
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Uso del componente Tabla en el documento del canal de impresión de AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

El siguiente vídeo muestra los pasos necesarios para utilizar el componente de tabla en Interactive Communications para documentos del canal de impresión.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

Las tablas se utilizan para mostrar datos en forma de tabla. Las filas de la tabla deben aumentar o reducirse según los datos devueltos por el origen de datos. Para utilizar una tabla en un documento del canal de impresión, se debe crear un archivo de diseño (archivo xdp) con AEM Forms Designer. En este archivo de diseño, agregamos la tabla con el número requerido de columnas. Asegúrese de que el tipo de objeto del campo de columna sea TextField o Numeric Field según sus necesidades. Para cada una de las columnas, los campos garantizan que el enlace de datos esté configurado como Usar nombre.

>[!NOTE]
>
>Para hacer que la tabla sea dinámica, asegúrese de haber marcado la fila como repetida.

**Probar en su propio servidor**

* [Descargue y descomprima el archivo de recursos en el disco duro](assets/usingtablesinprintchannel.zip)

* AEM Importe los dos archivos zip a mediante el administrador de paquetes de la interfaz de usuario de

* Los recursos asociados a este artículo incluyen lo siguiente:

   * Fragmento de diseño

   * Modal de datos de formulario

   * Documento de comunicación interactiva
   * sampleretirementaccountdata.json

* Abra el documento de comunicación interactiva en [modo de edición](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Agregue el fragmento de diseño TableDemo a la sección de contribuciones.
* Enlace las celdas de la tabla a los elementos adecuados del modelo de datos de formulario, tal como se muestra en el vídeo

* Vista previa del documento de comunicación interactiva con el archivo de datos json de ejemplo que se le ha proporcionado
