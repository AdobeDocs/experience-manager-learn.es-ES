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
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Crear formularios adaptables mediante el modelo de datos de formulario

El siguiente paso es crear un formulario adaptable y basarlo en el modelo de datos de formulario creado en el paso anterior.
El usuario introduce el ID de posible cliente y al tabular el servicio de Marketo para obtener los posibles clientes por ID se invoca. Los resultados de la operación de servicio se asignan a los campos adecuados de la Forms adaptable.

1. Cree un formulario adaptable y base en &quot;Plantilla de formulario en blanco&quot;, asócielo al modelo de datos de formulario creado en el paso anterior.
1. Abra el formulario en modo de edición
1. Arrastre y suelte un componente TextField y un componente Panel en el formulario adaptable. Establezca el título del componente TextField &quot;Introducir ID de posible cliente&quot; y su nombre en &quot;LeadId&quot;
1. Arrastre y suelte 2 componentes TextField en el componente Panel
1. Establezca el Nombre y el Título de los 2 componentes Textfield como FirstName y LastName
1. Configure el componente Panel para que sea un componente repetible; para ello, establezca el Mínimo en 1 y el Máximo en -1. Esto es necesario, ya que el servicio Marketo devuelve una matriz de objetos de posible cliente y necesita tener un componente repetible para mostrar los resultados. Sin embargo, en este caso, solo se devuelve un objeto de posible cliente porque se busca en objetos de posible cliente por su ID.
1. Cree una regla en el campo LeadId como se muestra en la siguiente imagen
1. Obtenga una vista previa del formulario, introduzca un ID de posible cliente válido en el campo ID de posible cliente y cierre la pestaña. Los campos Nombre y Apellidos deben rellenarse con los resultados de la llamada de servicio.

La siguiente captura de pantalla explica la configuración del editor de reglas

![ruleeditor](assets/ruleeditor.jfif)

## Depuración

Si utiliza los paquetes que se proporcionan con este artículo, es posible que desee habilitar [registros de depuración](http://localhost:4502/system/console/slinglog) para las siguientes clases:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`

## Felicitaciones

Se ha integrado correctamente AEM Forms con Marketo mediante el modelo de datos de formulario de AEM Forms.