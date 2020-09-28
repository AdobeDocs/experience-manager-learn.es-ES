---
title: AEM Forms con marketing (parte 4)
seo-title: AEM Forms con marketing (parte 4)
description: Tutorial para integrar AEM Forms con Marketing mediante el Modelo de datos de formulario de AEM Forms.
seo-description: Tutorial para integrar AEM Forms con Marketing mediante el Modelo de datos de formulario de AEM Forms.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# Creación de un formulario adaptable mediante el modelo de datos de formulario

El siguiente paso es crear un formulario adaptable y basarlo en el modelo de datos de formulario creado en el paso anterior.
El usuario ingresará el ID de posible cliente y, al desplazarse por el servicio de marketing para obtener los leads por ID, se invocará. Los resultados de la operación de servicio se asignan a los campos correspondientes del Forms adaptable.

1. Crear un formulario adaptable y basarlo en &quot;Plantilla de formulario en blanco&quot;, asociarlo al modelo de datos de formulario creado en el paso anterior.
1. Abrir el formulario en modo de edición
1. Arrastre y suelte un componente TextField y un componente Panel en el formulario adaptable. Defina el título del componente TextField &quot;Enter Lead Id&quot; y defina su nombre en &quot;LeadId&quot;
1. Arrastre y suelte 2 componentes TextField en el componente Panel
1. Defina el Nombre y el Título de los 2 componentes de campo de texto como Nombre y Apellido
1. Configure el componente Panel para que sea un componente repetible estableciendo el valor Mínimo en 1 y Máximo en -1. Esto es necesario, ya que el servicio de marketing devuelve una matriz de objetos posibles clientes y debe tener un componente repetible para mostrar los resultados. Sin embargo, en este caso, solo se recuperará un objeto Lead porque estamos buscando objetos Lead por su ID.
1. Cree una regla en el campo LeadId como se muestra en la imagen siguiente
1. Previsualización el formulario e introduzca un ID de posible cliente válido en el campo ID de posible cliente y en el campo de tabulación. Los campos Nombre y Apellido deben rellenarse con los resultados de la llamada de servicio.

La siguiente captura de pantalla explica la configuración del editor de reglas

![ruleeditor](assets/ruleeditor.jfif)
