---
title: Se adapta a AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 3 de un tutorial que integra Acroforms con AEM Forms. Pruebe el flujo de trabajo y el formulario adaptable en su sistema.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 2%

---


# Probar esta capacidad en el sistema

[Descargue e importe este paquete en AEM](assets/acro-form-aem-form.zip)
Este paquete contiene el flujo de trabajo de ejemplo y la página html que permite crear el esquema a partir de la Acrobat cargada.

## Configurar flujo de trabajo

1. [Abrir el modelo de flujo de trabajo en modo de edición](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Abra las propiedades de configuración del paso MergeAcrobatData .
3. Haga clic en la ficha Proceso .
4. Asegúrese de que los argumentos que está pasando sean una carpeta válida en su servidor.
5. Guarde los cambios.

## Crear formulario adaptable

1. Cree un formulario adaptable utilizando el esquema creado en el paso anterior.
2. Arrastre y suelte algunos elementos de esquema en el formulario adaptable.
3. Configure la acción de envío del formulario adaptable para enviarlo a AEM flujo de trabajo (MergeAcrobatData).
4. **Asegúrese de especificar la ruta del archivo de datos como &quot;Data.xml&quot;. Esto es muy importante, ya que el código de ejemplo busca un archivo llamado Data.xml en la carga útil del flujo de trabajo.**
5. Vista previa del formulario adaptable, rellene el formulario y envíelo.
6. Debería ver PDF con los datos combinados guardados en la carpeta especificada en el paso 4 del flujo de trabajo de configuración

>[!NOTE]
>
>El pdf generado por la combinación de datos con el acroform se guarda como pdfdocument.pdf en la carpeta de carga del flujo de trabajo. Este documento se puede utilizar para un procesamiento posterior como parte del flujo de trabajo
