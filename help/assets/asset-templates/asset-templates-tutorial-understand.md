---
title: 'Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets '
description: Este tutorial de vídeo explica cómo definir un archivo de InDesign y todas las consideraciones que lo acompañan para utilizarlo en la función de plantillas de recursos de AEM Assets.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---


# Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Este tutorial de vídeo explica cómo definir un archivo de InDesign y todas las consideraciones que lo acompañan para utilizarlo en la función de plantillas de recursos de AEM Assets.

## Construcción del archivo de plantilla de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Descargue y abra la [**plantilla de archivo de InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra el panel Etiquetas,** revise la convención de nomenclatura de etiquetas y tenga en cuenta que los elementos que se pueden crear en el archivo de InDesign ya están etiquetados. Recuerde que solo los elementos etiquetados se pueden editar en AEM.

   * **Ventana > Utilidades > Etiquetas**

3. En Página, agregue un nuevo elemento de texto, proporcione el texto &quot;Encabezado&quot; y aplique el estilo de párrafo **Encabezado**.

   * **Ventana > Estilos > Estilos de párrafo**

   A continuación, cree y aplique una nueva etiqueta denominada **Page2Heading.**

4. Agregue la imagen del logotipo de FPO ([que se proporciona en el zip](assets/asset-templates-tutorial-video--supporting-files.zip)) al elemento Logotipo de la página de formato.

   * **Haga clic con el botón derecho** y **seleccione Ajuste > Opciones de ajuste de marco... > Ajuste de contenido > Rellenar marco proporcionalmente**
   [Obtenga más información sobre las opciones](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) de ajuste de fotogramas y cuál es la adecuada para su caso de uso.

5. Copie el encabezado (Logotipo y Nombre de la empresa) de la plantilla maestra en Página y página mediante Pegar en contexto.

   * En la página 1, pulse Mayús-Cmd y haga clic en macOS o Mayús-Alt-Clic en Windows para seleccionar el encabezado expuesto en la página de formato y eliminarlo.
   * En la página de formato, copie el encabezado en la página 1 mediante Pegar en contexto .
   * Repita los pasos para la Página 2

6. Abra el panel Estructura haciendo doble clic en cada uno de ellos para asegurarse de que todos los elementos estructurales se corresponden con elementos reales del archivo de InDesign. Elimine cualquier elemento no utilizado o que no sea necesario. Asegúrese de que todo el etiquetado sea semántico y de que los elementos estén etiquetados correctamente.

   >[!NOTE]
   >
   >Recuerde que un archivo de InDesign mal construido es la causa más común de problemas con las plantillas de AEM Asset, por lo que asegúrese de que el etiquetado y la estructura sean correctos y limpios.

## Creación y creación de una plantilla de recursos en AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Inicie el puerto 8080 de InDesign** Server.
2. Asegúrese de que la instancia **AEM Author está configurada para interactuar con su InDesign Server** (y viceversa).

   * [Configuración de IDS Worker Cloud Service](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuración de Cloud Proxy Cloud Service](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuración OSGi de AEM Externalizer](http://localhost:4502/system/console/configMgr)

3. **Se ha cargado el archivo de InDesign en AEM** Assets y se permite que AEM Workflow e InDesign Server procesen completamente los recursos.
4. **Cree una nueva** plantilla en  **Assets > Plantillas** y seleccione el archivo de InDesign cargado a AEM en el paso 4.
5. **Edite la** plantilla de recursos creada en el paso 5 y cree los campos editables.
6. Haga clic en **Listo** para generar las representaciones finales de alta fidelidad de la plantilla de recursos.
7. Haga clic en la tarjeta Plantilla de recursos para abrirla y revise las Representaciones de recursos para descargar las representaciones de alta fidelidad.

## Recursos adicionales {#additional-resources}

Archivo de plantilla de InDesign e imágenes auxiliares

Descargar [El archivo de plantilla de InDesign y las imágenes compatibles](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Descarga de prueba de InDesign CC](https://creative.adobe.com/products/download/indesign)
* [Descarga de prueba de InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
