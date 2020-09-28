---
title: Se ajusta a AEM Forms
seo-title: Combinar datos de formulario adaptable con Acroform
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 1%

---


# Probar esta capacidad en el sistema

[Descargue e importe este paquete en AEM](assets/acro-form-aem-form.zip)Este paquete contiene el flujo de trabajo de muestra y la página html que le permite crear el esquema a partir de la Acrobat cargada.

## Configurar flujo de trabajo

1. [Abra el modelo de workflow en modo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)de edición.
2. Abra las propiedades de configuración del paso MergeAcroformData.
3. Haga clic en la ficha Proceso.
4. Asegúrese de que los argumentos que está pasando son una carpeta válida en el servidor.
5. Guarde los cambios.

## Crear formulario adaptable

1. Cree un formulario adaptable con el esquema creado en el paso anterior.
2. Arrastre y suelte algunos elementos de esquema en el formulario adaptable.
3. Configure la acción de envío del formulario adaptable para enviarla a AEM flujo de trabajo (MergeAcroformData).
4. **Asegúrese de especificar la ruta del archivo de datos como &quot;Data.xml&quot;. Esto es muy importante, ya que el código de muestra busca un archivo llamado Data.xml en la carga útil del flujo de trabajo.**
5. Formulario adaptable de previsualización, rellene el formulario y envíe.
6. Debe ver el PDF con los datos combinados guardados en la carpeta especificada en el paso 4 en el flujo de trabajo de configuración

>[!NOTE]
>
>El archivo PDF generado por la combinación de datos con el formulario se guarda como pdfdocument.pdf en la carpeta de carga útil del flujo de trabajo. Este documento se puede utilizar para seguir procesando como parte del flujo de trabajo
