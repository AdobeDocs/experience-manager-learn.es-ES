---
title: Generación de Documentos de Canal de impresión mediante la carpeta vigilada
seo-title: Generación de Documentos de Canal de impresión mediante la carpeta vigilada
description: Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones para el canal de impresión. En esta parte, generaremos documentos de canal de impresión utilizando el mecanismo de carpetas vigiladas.
seo-description: Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones para el canal de impresión. En esta parte, generaremos documentos de canal de impresión utilizando el mecanismo de carpetas vigiladas.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---


# Generación de Documentos de Canal de impresión mediante la carpeta vigilada

En esta parte, generaremos documentos de canal de impresión utilizando el mecanismo de carpetas vigiladas.

Después de crear y probar su documento de canal de impresión, necesitamos un mecanismo para generar estos documentos en modo por lotes o a petición. Generalmente, estos tipos de documentos se generan en modo por lotes y el mecanismo más común es utilizar carpetas vigiladas.

Al configurar una carpeta vigilada en AEM, se asocia una secuencia de comandos ECMA o un código Java que se ejecuta cuando se coloca un archivo en la carpeta vigilada. En este artículo, nos centraremos en la secuencia de comandos ECMA que generará documentos de canal de impresión y los guardará en el sistema de archivos.

La configuración de la carpeta observada y la secuencia de comandos ECMA forman parte de los recursos importados al [principio de este tutorial](introduction.md)

El archivo de entrada que se suelta en la carpeta controlada tiene la siguiente estructura. La secuencia de comandos ECMA lee los números de cuenta y genera documentos de canal de impresión para cada una de estas cuentas.

Para obtener más información sobre la secuencia de comandos ECMA para generar documentos, [consulte este artículo](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

Para generar el documento de canal de impresión mediante el mecanismo de carpetas vigiladas, siga los pasos a continuación:

* [Siga los pasos mencionados en este documento](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Inicie sesión en crx y vaya a /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Asegúrese de que la ruta a interactiveCommunicationsDocument apunte al documento correcto que desea imprimir.(Línea 1)
* Anote saveLocation(Línea 2).Puede cambiarlo según sus necesidades.
* Asegúrese de que el parámetro de entrada del Modelo de datos de formulario está enlazado al atributo de solicitud y su valor de enlace está establecido en &quot;accountnumber&quot;. Consulte la captura de pantalla de abajo.
   ![solicitud](assets/requestattributeprintchannel.gif)

* Cree un archivo accountnumber.xml con el siguiente contenido

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* Coloque el archivo xml en C:\RenderPrintChannel\input

* Compruebe los archivos PDF en la ubicación de almacenamiento, tal como se especifica en la secuencia de comandos ECMA.




