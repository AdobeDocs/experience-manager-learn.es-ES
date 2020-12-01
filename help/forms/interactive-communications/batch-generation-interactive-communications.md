---
title: Uso de la API por lotes para generar documentos de comunicación interactiva
description: Recursos de ejemplo para generar documentos de canal de impresión mediante API por lotes
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 3dc1bd3f2f7b6324c53640f01a263fa0728d439c
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---


# API de lote

Puede utilizar la API por lotes para generar varias comunicaciones interactivas a partir de una plantilla. La plantilla es una comunicación interactiva sin datos. La API de lote combina datos con una plantilla para generar una comunicación interactiva. La API es útil en la producción masiva de comunicaciones interactivas. Por ejemplo: facturas telefónicas, extractos de tarjetas de crédito para varios clientes.

[Obtenga más información sobre la API de generación de lotes](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Este artículo proporciona recursos de ejemplo para generar documentos de Interactive Communications mediante la API por lotes.

## Generación de lotes con la carpeta vigilada

* Importe la [plantilla de comunicación interactiva](assets/Beneficiaries-confirmation.zip) en su servidor de AEM Forms.
* Importe la configuración de [carpeta controlada](assets/batch-generation-api.zip). Esto creará una carpeta llamada `batchAPI` en la unidad C.

**Si está ejecutando AEM Forms en un sistema operativo que no es Windows, siga los 3 pasos que se mencionan a continuación:**

1. [Abrir carpeta vigilada](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Seleccione BatchAPIWatchedFolder y haga clic en Editar.
3. Cambie la ruta para que coincida con el sistema operativo.

![path](assets/watched-folder-batch-api-basic.PNG)

* Descargue y extraiga el contenido del [archivo zip](assets/jsonfile.zip). El archivo zip contiene una carpeta denominada `jsonfile` que contiene `beneficiaries.json` archivo. Este archivo tiene los datos para generar 3 documentos.

* Coloque la carpeta `jsonfile` en la carpeta de entrada de la carpeta vigilada.
* Una vez que la carpeta se haya seleccionado para su procesamiento, compruebe la carpeta de resultados de la carpeta vigilada. Debe ver 3 archivos PDF generados

## Generación de lotes mediante solicitudes REST

Puede invocar la [API por lotes](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) mediante solicitudes REST. Puede exponer los extremos REST para que otras aplicaciones invoquen la API para generar documentos.
Los recursos de muestra proporcionados exponen el punto final de REST para generar documentos de comunicación interactiva. El servlet acepta los siguientes parámetros:

* fileName: ubicación del archivo de datos en el sistema de archivos.
* templatePath: ruta de la plantilla IC
* saveLocation: ubicación para guardar los documentos generados en el sistema de archivos
* channelType: Print,Web o ambos
* recordId - Ruta JSON al elemento para establecer el nombre de una comunicación interactiva

La siguiente captura de pantalla muestra los parámetros y sus valores
![solicitud de muestra](assets/generate-ic-batch-servlet.PNG)

## Implementar recursos de muestra en el servidor

* Importar [ICTemplate](assets/ICTemplate.zip) mediante [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [controlador de envío personalizado](assets/BatchAPICustomSubmit.zip) mediante [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [Formulario adaptable](assets/BatchGenerationAPIAF.zip) mediante la [interfaz de Forms y Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Implementar y inicio [paquete OSGI personalizado](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) mediante [consola web Felix](http://localhost:4502/system/console/bundles)
* [Activar generación de lotes enviando el formulario](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
