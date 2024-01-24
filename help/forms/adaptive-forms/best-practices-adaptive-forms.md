---
title: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
description: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
duration: 65
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 7%

---

# Prácticas recomendadas

Los formularios de Adobe Experience Manager (AEM) pueden ayudarle a transformar transacciones complejas en experiencias digitales simples y atractivas. En el siguiente documento se describen algunas prácticas recomendadas adicionales que deben seguirse al desarrollar Forms adaptable. Este documento está diseñado para utilizarse junto con [este documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenciones de nomenclatura

* **Paneles**
   * Los nombres de los paneles son mayúsculas y minúsculas y comienzan con un carácter en mayúsculas.

* **Campos de formulario**
   * Los nombres de campo son mayúsculas y minúsculas y comienzan con un carácter en minúsculas.
   * No comience los nombres de los campos con números
   * No incluya guiones &quot;-&quot; en sus nombres. Esto equivale a un signo menos en el código y actuará como operadores en el código.
   * Los nombres pueden contener letras, dígitos, guiones bajos y signos de dólar.
   * Los nombres deben comenzar por una letra
   * Los nombres distinguen entre mayúsculas y minúsculas
   * Las palabras reservadas (como las palabras clave de JavaScript) no se pueden usar como nombres. Tenga cuidado con otras palabras reservadas específicas de AF, como &quot;panel&quot; o &quot;nombre&quot;.
   * No incluya guiones &quot;-&quot; en sus nombres
* **Desarrollo de Forms**
   * Los fragmentos de formulario deben tenerse en cuenta al desarrollar formularios grandes. Habilitar la carga diferida de fragmentos de formulario para tiempos de carga más rápidos
   * **DataModel**
      * Se recomienda asociar el formulario adaptable con el modelo de datos adecuado

   * **Eventos de objeto**
      * El código relacionado con la visibilidad de un objeto siempre debe colocarse en el evento de visibilidad de dicho objeto.
   * **Script**
      * Si el código que está escribiendo en un formulario adaptable se extiende más allá de las 5 líneas visibles, debe moverlo a una biblioteca de cliente. Lo ideal es añadir la función a la biblioteca de cliente y, a continuación, añadir las etiquetas jsdoc adecuadas para permitir que la función sea visible en el editor de reglas del formulario adaptable.
