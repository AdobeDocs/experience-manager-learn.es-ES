---
title: Uso de la API por lotes para generar documentos de comunicación interactiva
description: Activos de muestra para generar documentos de canal de impresión mediante la API por lotes
feature: Comunicación interactiva
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 3%

---


# API por lotes

Puede utilizar la API por lotes para producir varias comunicaciones interactivas a partir de una plantilla. La plantilla es una comunicación interactiva sin datos. La API por lotes combina datos con una plantilla para producir una comunicación interactiva. La API es útil en la producción masiva de comunicaciones interactivas. Por ejemplo, facturas telefónicas, extractos de tarjetas de crédito para varios clientes.

[Más información sobre la API de generación de lotes](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Este artículo proporciona recursos de ejemplo para generar documentos de Interactive Communications mediante la API por lotes.

## Generación de lotes con carpeta vigilada

* Importe la [plantilla de comunicación interactiva](assets/Beneficiaries-confirmation.zip) en el servidor de AEM Forms.
* Importe la [configuración de carpeta observada](assets/batch-generation-api.zip). Esto creará una carpeta llamada `batchAPI` en su unidad C.

**Si está ejecutando AEM Forms en un sistema operativo que no es de Windows, siga los 3 pasos que se indican a continuación:**

1. [Abrir carpeta vigilada](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Seleccione BatchAPIWatchedFolder y haga clic en Editar.
3. Cambie Path para que coincida con su sistema operativo.

![path](assets/watched-folder-batch-api-basic.PNG)

* Descargue y extraiga el contenido del [archivo zip](assets/jsonfile.zip). El archivo zip contiene la carpeta denominada `jsonfile` que contiene el archivo `beneficiaries.json`. Este archivo tiene los datos para generar 3 documentos.

* Coloque la carpeta `jsonfile` en la carpeta de entrada de la carpeta vigilada.
* Una vez que la carpeta se haya seleccionado para su procesamiento, compruebe la carpeta de resultados de la carpeta vigilada. Debería ver 3 archivos PDF generados

## Generación de lotes mediante solicitudes REST

Puede invocar la [API por lotes](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) mediante solicitudes REST. Puede exponer los extremos de REST para que otras aplicaciones invoquen la API para generar documentos.
Los recursos de ejemplo proporcionados exponen el extremo REST para generar documentos de comunicación interactiva. El servlet acepta los siguientes parámetros:

* fileName : ubicación del archivo de datos en el sistema de archivos.
* templatePath: ruta de la plantilla IC
* saveLocation : ubicación para guardar los documentos generados en el sistema de archivos
* channelType: Print, Web o ambas
* recordId - Ruta JSON al elemento para establecer el nombre de una comunicación interactiva

La siguiente captura de pantalla muestra los parámetros y sus valores
![solicitud de ejemplo](assets/generate-ic-batch-servlet.PNG)

## Implementar recursos de ejemplo en el servidor

* Importar [ICTemplate](assets/ICTemplate.zip) utilizando [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [Controlador de envío personalizado](assets/BatchAPICustomSubmit.zip) utilizando [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [Formulario adaptable](assets/BatchGenerationAPIAF.zip) mediante la [interfaz de Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Implementar e iniciar [paquete OSGI personalizado](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) utilizando [Consola web Felix](http://localhost:4502/system/console/bundles)
* [Activar la generación de lotes enviando el formulario](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
