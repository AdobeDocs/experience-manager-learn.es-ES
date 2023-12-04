---
title: Generar documentos de canal de impresión mediante carpeta inspeccionada
description: Esta es la parte 10 del tutorial de varios pasos para crear el primer documento de comunicaciones interactivas para el canal Imprimir. En esta parte, generaremos documentos del canal de impresión utilizando el mecanismo de carpetas vigiladas.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
duration: 98
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Generar documentos de canal de impresión mediante carpeta inspeccionada

En esta parte, generaremos documentos del canal de impresión utilizando el mecanismo de carpetas vigiladas.

Después de crear y probar el documento del canal de impresión, necesitamos un mecanismo para generar estos documentos en modo por lotes o bajo demanda. Normalmente, estos tipos de documentos se generan en modo por lotes y el mecanismo más común es utilizar una carpeta vigilada.

AEM Cuando configura una carpeta inspeccionada en el, asocia un script ECMA o un código java que se ejecuta cuando se suelta un archivo en la carpeta inspeccionada. En este artículo, nos centraremos en el script ECMA que generará documentos del canal de impresión y los guardará en el sistema de archivos.

La configuración de la carpeta inspeccionada y el script ECMA forman parte de los recursos importados en [inicio de este tutorial](introduction.md)

El archivo de entrada que se coloca en la carpeta vigilada tiene la siguiente estructura. El script ECMA lee los números de cuenta y genera un documento de canal de impresión para cada una de estas cuentas.

Para obtener más información sobre el script ECMA para generar documentos, [consulte este artículo](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

Para generar un documento de canal de impresión mediante el mecanismo de carpeta vigilada, siga los pasos a continuación:

* [Siga los pasos mencionados en este documento](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Inicie sesión en crx y vaya a /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Asegúrese de que la ruta a interactiveCommunicationsDocument apunte al documento correcto que desea imprimir.( Línea 1)
* Tome nota de saveLocation(Línea 2). Puede cambiarla según sus necesidades.
* Asegúrese de que el parámetro de entrada del modelo de datos de formulario está enlazado al atributo de solicitud y que su valor de enlace está establecido en &quot;accountnumber&quot;. Consulte la siguiente captura de pantalla.
  ![solicitud](assets/requestattributeprintchannel.gif)

* Cree el archivo accountnumbers.xml con el siguiente contenido

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

* Compruebe los archivos PDF en la ubicación de guardado según se especifica en el script ECMA.

## Pasos siguientes

[Abrir la interfaz de usuario del agente al enviar el formulario](./opening-agent-ui-on-form-submission.md)