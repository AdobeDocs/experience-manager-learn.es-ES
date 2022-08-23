---
title: Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets
description: Este tutorial de vídeo explica cómo definir un archivo de InDesign y todas las consideraciones que lo acompañan para utilizarlo en la función de plantillas de recursos de AEM Assets.
version: 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 1%

---

# Explicación de los archivos de InDesign y las plantillas de recursos en AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Este tutorial de vídeo explica cómo definir un archivo de InDesign y todas las consideraciones que lo acompañan para utilizarlo en la función de plantillas de recursos de AEM Assets.

## Construcción del archivo de plantilla de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Descargue y abra el [**Plantilla del archivo de InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra el panel Etiquetas .** revise la convención de nomenclatura de etiquetas y tenga en cuenta que los elementos que pueden crear en el archivo InDesign ya están etiquetados. Recuerde que solo los elementos etiquetados se pueden editar en AEM.

   * **Ventana > Utilidades > Etiquetas**

3. En la página, añada un nuevo elemento de texto, proporcione el texto &quot;Encabezado&quot; y aplique el **Encabezado** Estilo de párrafo.

   * **Ventana > Estilos > Estilos de párrafo**

   A continuación, cree y aplique una nueva etiqueta denominada **Page2Heading.**

4. Añadir la imagen del logotipo de FPO ([proporcionado en el zip](assets/asset-templates-tutorial-video--supporting-files.zip)) al elemento Logotipo de la página de formato.

   * **Clic con el botón derecho** y seleccione **Ajuste > Opciones de ajuste de marco... > Ajuste de contenido > Rellenar marco proporcionalmente**
   [Más información sobre las opciones de ajuste de marco](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames), y que es adecuado para su caso de uso.

5. Copie el encabezado (Logotipo y Nombre de la empresa) de la plantilla maestra en Página y página mediante Pegar en contexto.

   * En la página 1, pulse Mayús-Cmd y haga clic en macOS o Mayús-Alt-Clic en Windows para seleccionar el encabezado expuesto en la página de formato y eliminarlo.
   * En la página de formato, copie el encabezado en la página 1 mediante Pegar en contexto .
   * Repita los pasos para la Página 2

6. Abra el panel Estructura haciendo doble clic en cada uno de ellos para asegurarse de que todos los elementos estructurales se corresponden con elementos reales del archivo InDesign. Elimine cualquier elemento no utilizado o que no sea necesario. Asegúrese de que todo el etiquetado sea semántico y de que los elementos estén etiquetados correctamente.

   >[!NOTE]
   >
   >Recuerde que un archivo de InDesign mal construido es la causa más común de problemas con las plantillas de recursos de AEM, por lo que asegúrese de que el etiquetado y la estructura sean correctos y limpios.

## Creación y creación de plantillas de recursos en AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Iniciar InDesign Server** en el puerto 8080.
2. Asegúrese de que la variable **La instancia de autor de AEM está configurada para interactuar con el InDesign Server**(y viceversa).

   * [Configuración del Cloud Service de IDS Worker](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuración del Cloud Service del proxy de la nube](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuración de OSGi AEM externalizador](http://localhost:4502/system/console/configMgr)

3. **Se ha cargado el archivo de InDesign en AEM Assets** y permitir que AEM flujo de trabajo y InDesign Server procesen completamente los recursos.
4. **Crear una plantilla nueva** under **Recursos > Plantillas** y seleccione el archivo de InDesign cargado a AEM en el paso 4.
5. **Editar la plantilla de recursos** creado en el paso 5 y cree los campos editables.
6. Haga clic en **Listo** para generar las representaciones finales de alta fidelidad de la plantilla de recursos.
7. Haga clic en la tarjeta Plantilla de recursos para abrirla y revise las Representaciones de recursos para descargar las representaciones de alta fidelidad.

## Recursos adicionales {#additional-resources}

Archivo de plantilla de InDesign e imágenes auxiliares

Descargar [Archivo de plantilla de InDesign e imágenes auxiliares](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Descarga de prueba de InDesign CC](https://creative.adobe.com/products/download/indesign)
* La versión de prueba del InDesign Server se puede descargar desde [Sitio de prelanzamiento de Adobe](https://www.adobeprerelease.com/) o [Los clientes de CC Enterprise pueden ponerse en contacto con su ejecutivo de cuentas para solicitar una licencia de prueba de InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
