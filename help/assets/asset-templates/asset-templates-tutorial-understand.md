---
title: Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets
description: En este tutorial de vídeo se explica cómo definir un archivo de InDesign, junto con todas las consideraciones que lo acompañan, para utilizarlo en la función de plantillas de recursos de AEM Assets.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 909
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

En este tutorial de vídeo se explica cómo definir un archivo de InDesign, junto con todas las consideraciones que lo acompañan, para utilizarlo en la función de plantillas de recursos de AEM Assets.

## Construcción del archivo de plantilla de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Descargar y abrir la [**plantilla de archivo de InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra el panel Etiquetas,** revise la convención de nomenclatura de etiquetas y tenga en cuenta que los elementos que se pueden crear en el archivo de InDesign ya están etiquetados. AEM Recuerde, solo los elementos etiquetados se pueden editar en la.

   * **Ventana > Utilidades > Etiquetas**

3. En la página, agregue un nuevo elemento de texto, proporcione el texto &quot;Header&quot; y aplique el estilo de párrafo **Heading**.

   * **Ventana > Estilos > Estilos de párrafo**

   A continuación, cree y aplique una nueva etiqueta denominada **Page2Heading.**

4. Agregue la imagen del logotipo de FPO ([proporcionada en el zip](assets/asset-templates-tutorial-video--supporting-files.zip)) al elemento Logo de la página maestra.

   * **Haga clic con el botón derecho en** y seleccione **Ajuste > Opciones de ajuste de marco... > Ajuste de contenido > Rellenar marco proporcionalmente**

   [Obtenga más información acerca de las opciones de ajuste de marco](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) y cuál es la adecuada para su caso de uso.

5. Copie el encabezado (logotipo y nombre de la empresa) de la plantilla Patrón en Página y Página mediante Pegar en contexto.

   * En la página 1, pulse Mayús-Cmd y haga clic en macOS o Mayús-Alt y haga clic en Windows para seleccionar el encabezado expuesto en la página maestra y elimínelo.
   * Desde la página maestra, copie el encabezado en la página 1 mediante Pegar en contexto
   * Repita los pasos para la página 2

6. Abra el panel Estructura haciendo doble clic en cada uno, para asegurarse de que todos los elementos estructurales corresponden a elementos reales en el archivo de InDesign. Elimine los elementos que no utilice o que no necesite. Asegúrese de que todo el etiquetado sea semántico y de que los elementos estén etiquetados correctamente.

   >[!NOTE]
   >
   >Recuerde, un archivo de InDesign AEM mal construido es la causa más común de problemas con las plantillas de recursos de la, por lo que asegúrese de que el etiquetado y la estructura sean limpios y correctos.

## Creación de una plantilla de recursos en AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **Iniciar InDesign Server** en el puerto 8080.
2. AEM Asegúrese de que la instancia de autor de **esté configurada para interactuar con el InDesign Server** (y viceversa).

   * [Configuración del Cloud Service de trabajo IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuración del Cloud Service de proxy de nube](http://localhost:4502/etc/cloudservices/proxy.html)
   * AEM [Configuración OSGi del externalizador de](http://localhost:4502/system/console/configMgr)

3. **Se cargó el archivo de InDesign en AEM Assets AEM** y se permitió que el flujo de trabajo y el InDesign Server de trabajo de los recursos se procesaran por completo.
4. **Cree una nueva plantilla** en **Assets > Plantillas** y seleccione el archivo de InDesign AEM cargado en el paso #4 a la.
5. **Edite la plantilla de recursos** creada en el paso #5 y cree los campos editables.
6. Haga clic en **Listo** para generar las representaciones finales de alta fidelidad de la plantilla de recursos.
7. Haga clic en la tarjeta Plantilla de recurso para abrirla y revise las representaciones de recursos para descargar las representaciones de alta fidelidad.

## Recursos adicionales {#additional-resources}

Archivo de plantilla de InDesign e imágenes auxiliares

Descargar [archivo de plantilla de InDesign e imágenes auxiliares](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Descarga de versión de prueba de InDesign CC](https://creative.adobe.com/products/download/indesign)
* La versión de prueba de InDesign Server se puede descargar desde el [sitio de prelanzamiento de Adobe](https://www.adobeprerelease.com/) o [los clientes de CC Enterprise pueden comunicarse con su representante de cuentas para solicitar la licencia de prueba de InDesign Server de AEM](https://www.adobe.com/products/indesignserver/faq.html)
