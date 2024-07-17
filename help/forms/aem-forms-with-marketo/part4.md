---
title: AEM Forms con Marketo (parte 4)
description: Tutorial para integrar AEM Forms con Marketo mediante el modelo de datos de formulario de AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 68
source-git-commit: 8bde459ae9a6e261cfc3aff308babe9de6e56059
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---

# Prueba de la integración

Probaremos la integración creando una búsqueda de formulario sencilla y mostraremos un objeto de posible cliente de Market.
>[!NOTE]
>
>Esta funcionalidad se probó en formularios basados en componentes de base.

## Crear formulario adaptable

1. Cree un formulario adaptable y base en &quot;Plantilla de formulario en blanco&quot;, asócielo al modelo de datos de formulario creado en el paso anterior.
1. Abra el formulario en modo de edición.
1. Arrastre y suelte un componente TextField y un componente Panel en el formulario adaptable. Establezca el título del componente TextField &quot;Introducir ID de posible cliente&quot; y su nombre en &quot;LeadId&quot;
1. Arrastre y suelte 2 componentes TextField en el componente Panel
1. Establezca el Nombre y el Título de los 2 componentes Textfield como FirstName y LastName
1. Configure el componente Panel para que sea un componente repetible; para ello, establezca el Mínimo en 1 y el Máximo en -1. Esto es necesario, ya que el servicio Marketo devuelve una matriz de objetos de posible cliente y necesita tener un componente repetible para mostrar los resultados. Sin embargo, en este caso, solo se devuelve un objeto de posible cliente porque se busca en objetos de posible cliente por su ID.
1. Cree una regla en el campo LeadId como se muestra en la siguiente imagen
1. Obtenga una vista previa del formulario, introduzca un ID de posible cliente válido en el campo ID de posible cliente y cierre la pestaña. Los campos Nombre y Apellidos deben rellenarse con los resultados de la llamada de servicio.

La siguiente captura de pantalla explica la configuración del editor de reglas

![editor de reglas](assets/ruleeditor.png)


## Felicitaciones

Se ha integrado correctamente AEM Forms con Marketo mediante el modelo de datos de formulario de AEM Forms.