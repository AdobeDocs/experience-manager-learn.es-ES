---
title: Uso de carpetas vigiladas en AEM Forms
seo-title: Uso de carpetas vigiladas en AEM Forms
description: Configuración y uso de carpetas vigiladas en AEM Forms
seo-description: Configuración y uso de carpetas vigiladas en AEM Forms
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: Servicio de salida
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%

---


# Uso de carpetas vigiladas en AEM Forms{#using-watched-folders-in-aem-forms}

Un administrador puede configurar una carpeta de red, conocida como carpeta vigilada, de modo que cuando un usuario coloque un archivo (como un archivo PDF) en la carpeta vigilada, se inicie un flujo de trabajo, servicio o operación de secuencia de comandos preconfigurados para procesar el archivo añadido. Una vez que el servicio realiza la operación especificada, guarda el archivo de resultados en una carpeta de salida especificada. Para obtener más información sobre el flujo de trabajo, el servicio y la secuencia de comandos.

Para obtener más información sobre la creación de una carpeta vigilada, [haga clic aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Las carpetas vigiladas se utilizan para generar documentos en modo por lotes. Mediante el mecanismo de carpetas vigiladas, puede generar comunicaciones interactivas para el canal de impresión o utilizar el servicio de salida para combinar datos con la plantilla.

Este artículo tratará el caso de uso de combinar datos con una plantilla mediante el servicio de salida a través del mecanismo de carpetas vigiladas.

El servicio de salida es un servicio OSGi que forma parte de AEM Document Services. El servicio de salida admite varios formatos de salida y funciones de diseño de salida de AEM Forms Designer. El servicio de salida puede convertir plantillas XFA y datos XML para generar documentos de impresión en varios formatos.

Para obtener más información sobre el servicio de salida, [haga clic aquí](https://helpx.adobe.com/aem-forms/6/output-service.html).
Para configurar la carpeta vigilada en su sistema, siga los pasos a continuación:
* [Descargue y extraiga el contenido del archivo zip](assets/outputservicewatchedfolderkt.zip). Este archivo zip contiene el paquete para crear carpetas vigiladas y archivos de muestra para probar el servicio de salida mediante el mecanismo de carpetas vigiladas
   * Sistema Windows

      * Importe outputservicewatchedfolder.zip en AEM mediante el administrador de paquetes
      * Esto creará una carpeta vigilada llamada outputservicewatchedfolder en su unidad C.
   * Sistema no Windows
      * [Abra la configuración de la carpeta vigilada](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Establezca la propiedad de ruta de carpeta del nodo de servicio saliente para que apunte a una ubicación adecuada
      * Guarde los cambios
      * La ubicación mencionada anteriormente será su carpeta vigilada.

Coloque las carpetas SamplePdfFile y SampleXdpFile en la carpeta de entrada de la carpeta vigilada. Cuando el procesamiento de los archivos se realiza correctamente, los resultados se colocan en la carpeta de resultados de la carpeta vigilada.


>[!NOTE]
>
>Si la secuencia de comandos asociada a la carpeta observada necesita más de un archivo, debe crear una carpeta, colocar todos los archivos necesarios en la carpeta y soltar la carpeta en la carpeta de entrada de la carpeta vigilada.
>
>Si la secuencia de comandos asociada a la carpeta vigilada solo necesita un archivo de entrada, puede soltar el archivo directamente en la carpeta de entrada de la carpeta vigilada

