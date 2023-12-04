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
duration: 946
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

En este tutorial de vídeo se explica cómo definir un archivo de InDesign, junto con todas las consideraciones que lo acompañan, para utilizarlo en la función de plantillas de recursos de AEM Assets.

## Construcción del archivo de plantilla de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Descargue y abra [**plantilla de archivo de InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra el panel Etiquetas,** revise la convención de nomenclatura de etiquetas y tenga en cuenta que los elementos que se pueden crear en el archivo de InDesign ya están etiquetados. AEM Recuerde, solo los elementos etiquetados se pueden editar en la.

   * **Ventana > Utilidades > Etiquetas**

3. En la página, agregue un nuevo elemento de texto, proporcione el texto &quot;Header&quot; y aplique la variable **Encabezado** Estilo de párrafo.

   * **Ventana > Estilos > Estilos de párrafo**

   A continuación, cree y aplique una nueva etiqueta denominada **Page2Heading.**

4. Agregar la imagen del logotipo de FPO ([indicado en el zip](assets/asset-templates-tutorial-video--supporting-files.zip)) al elemento Logotipo de la página maestra.

   * **Clic derecho** y seleccione **Ajuste > Opciones de conexión de marco... > Ajuste de contenido > Rellenar marco proporcionalmente**

   [Más información sobre las Opciones de ajuste de cuadro](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames), y que es adecuado para su caso de uso.

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
2. Asegúrese de que **AEM Se ha configurado la instancia de autor de la para que interactúe con el InDesign Server**(y viceversa).

   * [Configuración del Cloud Service IDS Worker](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuración del Cloud Service de proxy de nube](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Configuración de OSGi del externalizador](http://localhost:4502/system/console/configMgr)

3. **Se ha cargado el archivo de InDesign en AEM Assets** AEM y permitir que el flujo de trabajo y el InDesign Server de la procesen completamente los recursos.
4. **Crear nueva plantilla** bajo **Recursos > Plantillas** y seleccione el archivo de InDesign AEM cargado en el #4 de la etapa de.
5. **Editar la plantilla de recursos** creado en el paso #5 y cree los campos editables.
6. Clic **Listo** para generar las representaciones finales de alta fidelidad de la plantilla de recursos.
7. Haga clic en la tarjeta Plantilla de recurso para abrirla y revise las representaciones de recursos para descargar las representaciones de alta fidelidad.

## Recursos adicionales {#additional-resources}

Archivo de plantilla de InDesign e imágenes auxiliares

Descargar [Archivo de plantilla de InDesign e imágenes auxiliares](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Descarga de versión de prueba de InDesign CC](https://creative.adobe.com/products/download/indesign)
* La versión de prueba de InDesign Server se puede descargar desde [Sitio de prelanzamiento de Adobe](https://www.adobeprerelease.com/) o [Los clientes de CC Enterprise pueden comunicarse con su representante de cuentas para solicitar la licencia de prueba de AEM InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
