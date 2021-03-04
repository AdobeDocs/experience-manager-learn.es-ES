---
title: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
seo-title: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
description: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
seo-description: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
feature: Formularios adaptables
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 2%

---

# Prácticas recomendadas  

Los formularios de Adobe Experience Manager (AEM) pueden ayudarle a transformar transacciones complejas en experiencias digitales simples y atractivas. En el siguiente documento se describen algunas prácticas recomendadas adicionales que deben seguirse al desarrollar formularios adaptables. Este documento está pensado para utilizarse junto con [este documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenciones de nomenclatura

* **Paneles**
   * Los nombres de panel son mayúsculas y minúsculas que comienzan con mayúsculas.

* **Campos del formulario**
   * Los nombres de campo son mayúsculas y minúsculas que comienzan por caracteres en minúsculas.
   * No iniciar nombres de campo con números
   * No incluya guiones &quot;-&quot; en sus nombres. Esto equivaldrá a un signo menos en el código y actuará como operadores en el código.
   * Los nombres pueden contener letras, dígitos, guiones bajos y signos de dólar.
   * Los nombres deben comenzar por una letra
   * Los nombres distinguen entre mayúsculas y minúsculas
   * Las palabras reservadas (como palabras clave de JavaScript) no se pueden usar como nombres. Cuidado con otras palabras reservadas específicas para AF, como   como &quot;panel&quot;,&quot;nombre&quot;.
   * No incluya guiones &quot;-&quot; en sus nombres
* **Desarrollo de formularios**
   * Los fragmentos de formulario deben tenerse en cuenta al desarrollar formularios grandes. Habilitar la carga diferida de fragmentos de formulario para una carga más rápida   times
   * **Modelo de datos**
      * Se recomienda asociar el formulario adaptable con el modelo de datos apropiado
   * **Eventos de objeto**
      * El código relacionado con la visibilidad de un objeto debe colocarse siempre en el evento de visibilidad de dicho objeto.
   * **Script**
      * Si el código que está escribiendo dentro de un formulario adaptable se extiende más allá de las 5 líneas visibles, debe mover el código a una biblioteca de cliente. Lo ideal es agregar la función a la biblioteca del cliente y, a continuación, agregar las etiquetas jsdoc adecuadas para permitir que la función esté visible en el editor de reglas del formulario adaptable.


