---
title: Crear modelos de flujo de trabajo de AEM Forms reutilizables.
description: modelos de flujo de trabajo independientes de Forms adaptable.
feature: Flujo de trabajo
version: 6.5
topic: Desarrollo
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# Crear modelos de flujo de trabajo de AEM Forms reutilizables{#create-re-usable-aem-forms-workflow-models}

A partir de la versión 6.5 de AEM Forms, ahora podemos crear modelos de flujo de trabajo que no estén vinculados a un formulario adaptable específico. Con esta capacidad, ahora puede crear un modelo de flujo de trabajo que se pueda invocar en diferentes envíos de formularios adaptables. Con esta capacidad, puede tener un flujo de trabajo genérico para gestionar todos los envíos de formularios adaptables para su revisión y aprobación.

Para diseñar un flujo de trabajo de este tipo, realice los siguientes pasos

1. Iniciar sesión en AEM
1. Apunte el navegador al [modelo de flujo de trabajo](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Haga clic en Crear | Crear modelo para añadir modelo de flujo de trabajo
1. Proporcione el nombre y el título adecuados al modelo de flujo de trabajo y, a continuación, haga clic en Finalizado
1. Abra el modelo recién creado en modo de edición
1. Arrastre y suelte el componente Asignar tarea en el modelo de flujo de trabajo
1. Abra las propiedades de configuración del componente Asignar tarea
1. Tabulación a la ficha Forms y documentos
1. Seleccione Tipo - Formulario adaptable o Formulario adaptable de solo lectura.

Existen 3 maneras de especificar la ruta del formulario

1. Disponible en una ruta absoluta : esto significa que el flujo de trabajo se asociará estrechamente con la forma adaptativa. Esto no es lo que queremos aquí
1. **Enviado al flujo de trabajo** : esto significa que, cuando se envía el formulario adaptable, el motor de flujo de trabajo extraerá el nombre del formulario de los datos enviados. Esta es la opción que debe seleccionarse
1. Disponible en una ruta en una variable: esto significa que el formulario adaptable se recogerá de la variable de flujo de trabajo
La siguiente captura de pantalla muestra la opción correcta que debe elegir para desacoplar el flujo de trabajo de un formulario adaptable

![modelo de flujo de trabajo](assets/workflomodel.PNG)