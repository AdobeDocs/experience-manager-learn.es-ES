---
title: Acroforms con AEM Forms
description: Parte 3 de un tutorial que integra AcroForms con AEM Forms. Pruebe el flujo de trabajo y el formulario adaptable en el sistema.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 3%

---


# Pruebe esta capacidad en su sistema

AEM [Descargar e importar este paquete en el espacio de trabajo {10000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000
Este paquete contiene el flujo de trabajo de ejemplo y la página html que le permite crear el esquema a partir del AcroForm cargado.](assets/acro-form-aem-form.zip)

## Configurar flujo de trabajo

1. [Abrir el modelo de flujo de trabajo en modo de edición](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Abra las propiedades de configuración del paso MergeAcrobatData.
3. Haga clic en la pestaña Proceso.
4. Asegúrese de que los argumentos que está pasando sean una carpeta válida del servidor.
5. Guarde los cambios.

## Crear formulario adaptable

1. Cree un formulario adaptable con el esquema creado en el paso anterior.
2. Arrastre y suelte algunos elementos de esquema en el formulario adaptable.
3. AEM Configure la acción de envío del formulario adaptable para enviarlo al flujo de trabajo de la (MergeAcroFormData).
4. **Asegúrese de especificar la ruta del archivo de datos como &quot;Data.xml&quot;. Esto es muy importante, ya que el código de ejemplo busca un archivo llamado Data.xml en la carga útil del flujo de trabajo.**
5. Obtenga una vista previa del formulario adaptable, rellene el formulario y envíelo.
6. Debería ver el PDF con los datos combinados guardados en la carpeta especificada en el paso 4 en el flujo de trabajo de configuración

>[!NOTE]
>
>El PDF generado al combinar datos con acroform se guarda como pdfdocument.pdf en la carpeta de carga útil del flujo de trabajo. Este documento se puede utilizar para un procesamiento posterior como parte del flujo de trabajo
