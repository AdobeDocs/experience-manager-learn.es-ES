---
title: Uso de carpetas vigiladas en AEM Forms
seo-title: Uso de carpetas vigiladas en AEM Forms
description: Configuración y uso de carpetas vigiladas en AEM Forms
seo-description: Configuración y uso de carpetas vigiladas en AEM Forms
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# Uso de carpetas vigiladas en AEM Forms{#using-watched-folders-in-aem-forms}

Un administrador puede configurar una carpeta de red, conocida como carpeta vigilada, de modo que cuando un usuario coloque un archivo (como un archivo PDF) en la carpeta vigilada, se inicie una operación de flujo de trabajo, servicio o secuencia de comandos preconfigurada para procesar el archivo agregado. Una vez que el servicio realiza la operación especificada, guarda el archivo de resultados en una carpeta de salida especificada. Para obtener más información sobre el flujo de trabajo, el servicio y la secuencia de comandos.

Para obtener más información sobre la creación de una carpeta vigilada, [haga clic aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Las carpetas vigiladas se utilizan para generar documentos en modo de lote. Mediante el mecanismo de carpetas vigiladas, puede generar comunicaciones interactivas para el canal de impresión o utilizar el servicio de salida para combinar datos con la plantilla.

Este artículo tratará el caso de uso de combinar datos con una plantilla mediante el servicio de salida a través de un mecanismo de carpetas vigiladas.

El servicio de salida es un servicio OSGi que forma parte de AEM Documento Services. El servicio Output admite varios formatos de salida y funciones de diseño de salida de AEM Forms Designer. El servicio Output puede convertir plantillas XFA y datos XML para generar documentos de impresión en diversos formatos.

Para obtener más información sobre el servicio de salida, [haga clic aquí](https://helpx.adobe.com/aem-forms/6/output-service.html).
Para configurar la carpeta vigilada en el sistema, siga los pasos a continuación:
* [Descargue y extraiga el contenido del archivo](assets/outputservicewatchedfolderkt.zip) zip.Este archivo zip contiene un paquete para crear archivos de ejemplo y carpetas vigiladas para probar el servicio de salida mediante el mecanismo de carpetas vigiladas
   * Sistema Windows

      * Importe outputservicewatchedfolder.zip en AEM mediante el administrador de paquetes
      * Esto creará una carpeta vigilada llamada outputservicewatchedfolder en la unidad C.
   * Sistema no Windows
      * [Abra la configuración de la carpeta vigilada](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Establezca la propiedad de ruta de carpeta del nodo outservice para que señale a una ubicación adecuada
      * Guardar los cambios
      * La ubicación mencionada anteriormente será la carpeta vigilada.

Coloque las carpetas SamplePdfFile y SampleXdpFile en la carpeta de entrada de la carpeta controlada. Al procesar correctamente los archivos, los resultados se colocan en la carpeta de resultados de la carpeta vigilada.


>[!NOTE]
>
>Si la secuencia de comandos asociada a la carpeta vigilada necesita más de un archivo, debe crear una carpeta y colocar todos los archivos necesarios en la carpeta y colocar la carpeta en la carpeta de entrada de la carpeta vigilada.
>
>Si la secuencia de comandos asociada a la carpeta vigilada solo necesita un archivo de entrada, puede colocarlo directamente en la carpeta de entrada de la carpeta vigilada

