---
title: Crear modelos de flujo de trabajo de AEM Forms reutilizables.
seo-title: Crear modelos de flujo de trabajo de AEM Forms reutilizables.
description: modelos de flujo de trabajo independientes de Forms adaptable.
seo-description: Modelos de flujo de trabajo independientes de Forms adaptable.
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---


# Crear modelos de flujo de trabajo de AEM Forms reutilizables{#create-re-usable-aem-forms-workflow-models}

A partir de la versión 6.5 de AEM Forms, ahora podemos crear modelos de flujo de trabajo que no están vinculados a un formulario adaptable específico. Con esta función, ahora puede crear un modelo de flujo de trabajo que se pueda invocar en diferentes envíos de formularios adaptables. Con esta función, puede disponer de un flujo de trabajo genérico para gestionar todos los envíos de formularios adaptables para su revisión y aprobación.

Para diseñar un flujo de trabajo de este tipo, realice los siguientes pasos

1. Iniciar sesión en AEM
1. Apunte el navegador al modelo [de flujo de trabajo](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Haga clic en Crear | Crear modelo para Añadir modelo de flujo de trabajo
1. Proporcione el nombre y el título adecuados al modelo de flujo de trabajo y, a continuación, haga clic en Finalizado
1. Abrir el modelo recién creado en modo de edición
1. Arrastre y suelte Asignar componente de Tarea en el modelo de flujo de trabajo
1. Abrir las propiedades de configuración del componente Asignar Tarea
1. Ficha de la ficha Forms y Documentos
1. Seleccione Tipo: Formulario adaptable o Formulario adaptable de sólo lectura.

Existen 3 formas de especificar la ruta del formulario

1. Disponible en una ruta absoluta: Esto significa que el flujo de trabajo se acoplará estrechamente con un formulario adaptable. Esto no es lo que queremos aquí
1. **Enviado al flujo de trabajo** : esto significa que cuando se envía el formulario adaptable, el motor de flujos de trabajo extraerá el nombre del formulario de los datos enviados. Esta es la opción que debe seleccionarse
1. Disponible en una ruta de acceso en una variable- Esto significa que el formulario adaptable se recogerá de la variable de flujo de trabajoLa siguiente captura de pantalla muestra la opción correcta que debe elegir para desacoplar el flujo de trabajo de un formulario adaptable

![modelo de flujo de trabajo](assets/workflomodel.PNG)