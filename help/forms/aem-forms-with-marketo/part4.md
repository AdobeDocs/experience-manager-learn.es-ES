---
title: AEM Forms con Marketo (parte 4)
description: Tutorial para integrar AEM Forms con Marketo mediante el Modelo de datos de formulario de AEM Forms.
feature: Forms adaptable, Modelo de datos de formulario
version: 6.3,6.4,6.5
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---


# Creación de un formulario adaptable mediante el modelo de datos de formulario

El siguiente paso es crear un formulario adaptable y basarlo en el modelo de datos de formulario creado en el paso anterior.
El usuario introduce el ID de posible cliente y, al desplazarse por el servicio de Marketo para obtener los posibles clientes mediante el ID, se invoca. Los resultados de la operación de servicio se asignan a los campos correspondientes de la Forms adaptable.

1. Crear un formulario adaptable y basarlo en &quot;Plantilla de formulario en blanco&quot;, asociarlo al Modelo de datos de formulario creado en el paso anterior.
1. Abrir el formulario en modo de edición
1. Arrastre y suelte un componente TextField y un componente Panel en el formulario adaptable. Defina el título del componente TextField &quot;Enter Lead Id&quot; y establezca su nombre en &quot;LeadId&quot;
1. Arrastre y suelte 2 componentes TextField en el componente Panel
1. Defina el Nombre y el Título de los 2 componentes de campo de texto como Nombre y Apellido
1. Configure el componente Panel para que sea un componente repetible ajustando Mínimo a 1 y Máximo a -1. Esto es necesario, ya que el servicio de Marketo devuelve una matriz de objetos de posible cliente y debe tener un componente repetible para mostrar los resultados. Sin embargo, en este caso, solo se recuperará un objeto Lead porque estamos buscando objetos Lead por su ID.
1. Cree una regla en el campo LeadId como se muestra en la imagen siguiente
1. Obtenga una vista previa del formulario e introduzca un ID de posible cliente válido en el campo ID de posible cliente y en la pestaña de salida. Los campos Nombre y Apellido deben rellenarse con los resultados de la llamada de servicio.

La siguiente captura de pantalla explica la configuración del editor de reglas

![editor de reglas](assets/ruleeditor.jfif)
