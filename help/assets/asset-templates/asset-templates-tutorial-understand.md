---
title: 'Explicación de los archivos InDesign y las plantillas de recursos en AEM Assets '
description: En este tutorial de vídeo se explica cómo definir un archivo InDesign y todas las consideraciones que lo acompañan para utilizarlo en la función de plantillas de recursos de AEM Assets.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# Explicación de los archivos InDesign y las plantillas de recursos en AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

En este tutorial de vídeo se explica cómo definir un archivo InDesign y todas las consideraciones que lo acompañan para utilizarlo en la función de plantillas de recursos de AEM Assets.

## Construcción del archivo de plantilla de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Descargar y abrir la [**plantilla de archivo de InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra el panel Etiquetas,** revise la convención de nomenclatura de etiquetas y tenga en cuenta que los elementos que se pueden crear en el archivo InDesign ya están etiquetados. Recuerde que solo los elementos etiquetados se pueden editar en AEM.

   * **Ventana > Utilidades > Etiquetas**

3. En Página, Añada un nuevo elemento de texto, proporcione el texto &quot;Encabezado&quot; y aplique el estilo de párrafo **Encabezado**.

   * **Ventana > Estilos > Estilos de párrafo**

   A continuación, cree y aplique una nueva etiqueta denominada **Page2Heading.**

4. Añada la imagen del logotipo de FPO ([proporcionada en el zip](assets/asset-templates-tutorial-video--supporting-files.zip)) al elemento Logotipo de la página de formato.

   * **Haga clic con el botón derecho** y **seleccione Encaje > Opciones de encaje de marco... > Ajuste de contenido > Rellenar marco proporcionalmente**
   [Obtenga más información sobre las opciones](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) de Encaje de marco y cuál es la adecuada para su caso de uso.

5. Copie el encabezado (Logotipo y Nombre de la Compañía) de la plantilla maestra en Página y página mediante Pegar en contexto.

   * En la página 1, Mayús-Cmd-Clic en macOS o Mayús-Alt-Clic en Windows, para seleccionar el encabezado expuesto en la página de formato y eliminarlo.
   * En la página de formato, copie el encabezado en la página 1 mediante Pegar en contexto
   * Repita los pasos para la página 2

6. Abra el panel Estructura, haciendo clic con el doble en cada uno de ellos, para asegurarse de que todos los elementos estructurales corresponden a elementos reales del archivo InDesign. Elimine los elementos no utilizados o no necesarios. Asegúrese de que todo el etiquetado sea semántico y que los elementos estén etiquetados correctamente.

   >[!NOTE]
   >
   >Recuerde que un archivo InDesign mal construido es la causa más común de problemas con las plantillas de recursos de AEM, por lo que debe asegurarse de que el etiquetado y la estructura sean correctos y estén limpios.

## Creación y creación de una plantilla de recursos en AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Puerto** Serveron inicio InDesign 8080.
2. Asegúrese de que la instancia de **AEM Author está configurada para interactuar con su InDesign Server** (y viceversa).

   * [Configuración del Cloud Service de IDS Worker](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuración del Cloud Service proxy de nube](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuración de OSGi de AEM Externalizer](http://localhost:4502/system/console/configMgr)

3. **Se ha cargado el archivo InDesign en AEM** Recursos y permite que AEM flujo de trabajo y InDesign Server procesen los recursos por completo.
4. **Cree una nueva** plantilla en  **Recursos >** Plantillas y seleccione el archivo de InDesign cargado en AEM en el paso 4.
5. **Edite la** plantilla de recursos creada en el paso 5 y cree los campos editables.
6. Haga clic en **Listo** para generar las representaciones finales de alta fidelidad de la plantilla de recursos.
7. Haga clic en la tarjeta Plantilla de recursos para abrirla y revise las Representaciones de recursos para descargar las representaciones de alta fidelidad.

## Recursos adicionales {#additional-resources}

Archivo de plantilla de InDesign e imágenes compatibles

Descargar [archivo de plantilla de InDesign e imágenes de soporte](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Descarga de prueba de InDesign CC](https://creative.adobe.com/products/download/indesign)
* [Descarga de prueba de InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
