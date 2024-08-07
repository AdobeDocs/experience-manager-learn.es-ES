---
title: Usar la API por lotes para generar documentos de comunicación interactiva
description: Recursos de muestra para generar documentos de canal de impresión mediante API por lotes
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
duration: 77
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 16%

---

# API por lotes

Puede utilizar la API por lotes para generar varias comunicaciones interactivas a partir de una plantilla. La plantilla es una comunicación interactiva sin ningún tipo de datos. La API por lotes combina datos con una plantilla para generar una comunicación interactiva. La API resulta muy útil a la hora de producir comunicaciones interactivas de forma masiva, como facturas telefónicas y extractos de tarjetas de crédito para varios clientes.

[Más información sobre la API de generación por lotes](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Este artículo proporciona recursos de ejemplo para generar documentos de comunicaciones interactivas mediante la API por lotes.

## Generación de lotes mediante una carpeta inspeccionada

* Importe la [plantilla de comunicación interactiva](assets/Beneficiaries-confirmation.zip) en su servidor de AEM Forms.
* Importe la [configuración de la carpeta vigilada](assets/batch-generation-api.zip). Esto creará una carpeta llamada `batchAPI` en la unidad C.

**Si está ejecutando AEM Forms en un sistema operativo que no sea Windows, siga los 3 pasos que se mencionan a continuación:**

1. [Abrir carpeta vigilada](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Seleccione BatchAPIWatchedFolder y haga clic en Editar.
3. Cambie la Path para que coincida con su sistema operativo.

![ruta](assets/watched-folder-batch-api-basic.PNG)

* Descargue y extraiga el contenido de [archivo zip](assets/jsonfile.zip). El archivo zip contiene la carpeta denominada `jsonfile`, que contiene el archivo `beneficiaries.json`. Este archivo tiene los datos para generar 3 documentos.

* Coloque la carpeta `jsonfile` en la carpeta de entrada de la carpeta vigilada.
* Una vez que la carpeta se recoja para procesarla, compruebe la carpeta de resultados de la carpeta vigilada. Debería ver 3 archivos de PDF generados

## Generación por lotes mediante solicitudes REST

Puede invocar la [API por lotes](https://helpx.adobe.com/es/experience-manager/6-5/forms/javadocs/index.html) mediante solicitudes REST. Puede exponer los extremos REST para que otras aplicaciones invoquen la API para generar documentos.
Los recursos de ejemplo proporcionados exponen el extremo REST para generar documentos de comunicación interactiva. El servlet acepta los siguientes parámetros:

* fileName: ubicación del archivo de datos en el sistema de archivos.
* templatePath: ruta de la plantilla IC
* saveLocation: ubicación para guardar los documentos generados en el sistema de archivos
* channelType: Imprimir, Web o ambos
* recordId: ruta JSON al elemento para establecer el nombre de una comunicación interactiva

La siguiente captura de pantalla muestra los parámetros y sus valores
![solicitud de muestra](assets/generate-ic-batch-servlet.PNG)

## Implementación de recursos de ejemplo en el servidor

* Importar [ICTemplate](assets/ICTemplate.zip) mediante [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [controlador de envío personalizado](assets/BatchAPICustomSubmit.zip) mediante [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [formulario adaptable](assets/BatchGenerationAPIAF.zip) mediante la [interfaz de Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Implementar e iniciar [paquete OSGI personalizado](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) con [Consola web Felix](http://localhost:4502/system/console/bundles)
* [Generación de lotes de Déclencheur enviando el formulario](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
