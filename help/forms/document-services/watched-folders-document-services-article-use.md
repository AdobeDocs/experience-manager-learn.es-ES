---
title: Uso de carpetas inspeccionadas en AEM Forms
description: Configuración y uso de carpetas vigiladas en AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
duration: 94
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 25%

---

# Uso de carpetas inspeccionadas en AEM Forms{#using-watched-folders-in-aem-forms}

Un administrador puede configurar una carpeta de red, conocida como carpeta inspeccionada, de modo que cuando un usuario coloque un archivo (como un archivo PDF) en la carpeta inspeccionada, se inicie un flujo de trabajo preconfigurado, un servicio o una operación de script para procesar el archivo agregado. Una vez que el servicio realiza la operación especificada, guarda el archivo de resultados en una carpeta de salida especificada. Para obtener más información sobre el flujo de trabajo, servicio y script.

Para obtener más información sobre la creación de una carpeta vigilada, [haga clic aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Las carpetas inspeccionadas se utilizan para generar documentos en modo por lotes. Mediante el mecanismo de carpetas inspeccionadas, puede generar comunicaciones interactivas para el canal Imprimir o utilizar el servicio de salida para combinar datos con la plantilla.

Este artículo tratará el caso de uso de la combinación de datos con una plantilla mediante el servicio de salida a través del mecanismo de carpetas vigiladas.

El servicio Output es un servicio OSGi que forma parte de AEM Document Services. El servicio Output admite varios formatos de salida y funciones de diseño de salida de AEM Forms Designer. El servicio Output puede convertir plantillas XFA y datos XML para generar documentos de impresión en varios formatos.

Para obtener más información sobre el servicio Output, [haga clic aquí](https://helpx.adobe.com/aem-forms/6/output-service.html).
Para configurar una carpeta vigilada en el sistema, siga los pasos a continuación:
* [Descargue y extraiga el contenido del archivo zip](assets/outputservicewatchedfolderkt.zip).Este archivo zip contiene un paquete para crear archivos de carpeta inspeccionada y archivos de muestra para probar el servicio de salida mediante el mecanismo de carpeta inspeccionada
   * Sistema Windows

      * AEM Importe outputservicewatchedfolder.zip en mediante el administrador de paquetes para la creación de un grupo de informes
      * Esto crea una carpeta vigilada llamada outputservicewatchedfolder en la unidad C.
   * Sistema que no es Windows
      * [Abra el ajuste de configuración de la carpeta vigilada](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Establezca la propiedad de ruta de la carpeta del nodo de salida para que apunte a una ubicación adecuada
      * Guarde los cambios
      * La ubicación mencionada anteriormente es su carpeta vigilada.

Coloque las carpetas SamplePdfFile y SampleXdpFile en la carpeta de entrada de la carpeta vigilada. Si el procesamiento de los archivos se realiza correctamente, los resultados se colocan en la carpeta de resultados de la carpeta vigilada.


>[!NOTE]
>
>Si la secuencia de comandos asociada a la carpeta inspeccionada necesita más de un archivo, debe crear una carpeta y colocar todos los archivos necesarios en la carpeta y soltar la carpeta en la carpeta de entrada de la carpeta inspeccionada.
>
>Si la secuencia de comandos asociada a la carpeta inspeccionada solo necesita un archivo de entrada, puede soltar el archivo directamente en la carpeta de entrada de la carpeta inspeccionada
