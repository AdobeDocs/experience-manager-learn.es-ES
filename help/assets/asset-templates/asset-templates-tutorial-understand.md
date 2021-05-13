---
title: 'Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets '
description: Este tutorial de vídeo explica cómo definir un archivo de InDesign y todas las consideraciones que lo acompañan para utilizarlo en la función de plantillas de recursos de AEM Assets.
version: 6.3, 6.4, 6.5
topic: Administración de contenido
role: Business Practitioner
level: Intermediate
source-git-commit: b4fa992abe22e3a546d651e465d6ffc9e415aee2
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---


# Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Este tutorial de vídeo explica cómo definir un archivo de InDesign y todas las consideraciones que lo acompañan para utilizarlo en la función de plantillas de recursos de AEM Assets.

## Construcción del archivo de plantilla de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Descargar y abrir la [**plantilla del archivo de InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra el panel Etiquetas,** revise la convención de nomenclatura de etiquetas y tenga en cuenta que los elementos que pueden crear en el archivo InDesign ya están etiquetados. Recuerde que solo los elementos etiquetados se pueden editar en AEM.

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

6. Abra el panel Estructura haciendo doble clic en cada uno de ellos para asegurarse de que todos los elementos estructurales se corresponden con elementos reales del archivo InDesign. Elimine cualquier elemento no utilizado o que no sea necesario. Asegúrese de que todo el etiquetado sea semántico y de que los elementos estén etiquetados correctamente.

   >[!NOTE]
   >
   >Recuerde que un archivo de InDesign mal construido es la causa más común de problemas con las plantillas de recursos de AEM, por lo que asegúrese de que el etiquetado y la estructura sean correctos y limpios.

## Creación y creación de una plantilla de recursos en AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Inicie el** puerto 8080 del servidor de InDesign.
2. Asegúrese de que la **instancia de autor de AEM esté configurada para interactuar con el InDesign Server** (y viceversa).

   * [Configuración del Cloud Service de IDS Worker](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuración del Cloud Service del proxy de la nube](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuración de OSGi AEM externalizador](http://localhost:4502/system/console/configMgr)

3. **Se ha cargado el archivo de InDesign en AEM** Recursos y se permite que AEM flujo de trabajo y InDesign Server procesen completamente los recursos.
4. **Cree una nueva** plantilla en  **Assets >** Plantillas y seleccione el archivo de InDesign cargado en AEM en el paso 4.
5. **Edite la** plantilla de recursos creada en el paso 5 y cree los campos editables.
6. Haga clic en **Listo** para generar las representaciones finales de alta fidelidad de la plantilla de recursos.
7. Haga clic en la tarjeta Plantilla de recursos para abrirla y revise las Representaciones de recursos para descargar las representaciones de alta fidelidad.

## Recursos adicionales {#additional-resources}

Archivo de plantilla de InDesign e imágenes auxiliares

Descargar [archivo de plantilla de InDesign y imágenes](assets/asset-templates-tutorial-video--supporting-files-1.zip) compatibles

* [Descarga de prueba de InDesign CC](https://creative.adobe.com/products/download/indesign)
* [Los clientes de CC Enterprise pueden ponerse en contacto con su ejecutivo de cuentas para solicitar una licencia de prueba de InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
