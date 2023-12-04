---
title: Modelos de flujo de trabajo de AEM Forms reutilizables
description: Aprenda a crear modelos de flujo de trabajo independientes de Forms adaptable.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 83
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Crear modelos de flujo de trabajo de AEM Forms reutilizables{#create-re-usable-aem-forms-workflow-models}

A partir de la versión 6.5 de AEM Forms, ahora podemos crear modelos de flujo de trabajo que no estén vinculados a un formulario adaptable específico. Con esta capacidad, ahora puede crear un modelo de flujo de trabajo que se puede invocar en diferentes envíos de formularios adaptables. Con esta capacidad, puede tener un flujo de trabajo genérico para gestionar todos los envíos de formularios adaptables para su revisión y aprobación.

Para diseñar un flujo de trabajo de este tipo, realice los siguientes pasos

1. AEM Iniciar sesión en el sitio de
1. Dirija el explorador a [modelo de flujo de trabajo](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Clic __Crear > Crear modelo__ para agregar un modelo de flujo de trabajo
1. Proporcione el Nombre y el Título adecuados al modelo de flujo de trabajo y, a continuación, haga clic en Listo
1. Abra el modelo recién creado en el modo Edición
1. Arrastre y suelte el componente Asignar tarea en el modelo de flujo de trabajo
1. Abra las propiedades de configuración del componente Asignar tarea
1. Vaya a la pestaña Forms y documentos
1. Seleccione el tipo: formulario adaptable o formulario adaptable de solo lectura.

Existen tres formas de especificar la ruta del formulario

1. Disponible en ruta absoluta: esto significa que el flujo de trabajo está estrechamente emparejado con el formulario adaptable. Esto no es lo que queremos aquí
1. **Enviado al flujo de trabajo** : Esto significa que, cuando se envía el formulario adaptable, el motor de flujo de trabajo extrae el nombre del formulario de los datos enviados. Esta es la opción que debe seleccionarse
1. Disponible en una ruta en una variable: esto significa que el formulario adaptable se recoge de la variable de flujo de trabajo. La siguiente captura de pantalla muestra la opción correcta que necesita elegir para desacoplar el flujo de trabajo del formulario adaptable

![Modelos de flujo de trabajo de AEM Forms reutilizables](assets/workflomodel.PNG)
