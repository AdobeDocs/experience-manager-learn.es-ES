---
title: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
seo-title: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
description: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
seo-description: Convenciones de nomenclatura y prácticas recomendadas que se deben seguir al crear formularios adaptables
feature: adaptive-forms
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 5b05dbe45babfcfcfc81995d9d48bc9b26b9b6c8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 1%

---

# Prácticas recomendadas  

Los formularios de Adobe Experience Manager (AEM) pueden ayudarle a transformar transacciones complejas en experiencias digitales sencillas y atractivas. En el siguiente documento se describen algunas prácticas recomendadas adicionales que deben seguirse al desarrollar un Forms adaptable. Este documento está diseñado para utilizarse junto con [este documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenciones de nomenclatura

* **Paneles**
   * Los nombres de los paneles son mayúsculas y minúsculas que comienzan con un carácter en mayúsculas.

* **Campos del formulario**
   * Los nombres de los campos son mayúsculas y minúsculas.
   * No inicio nombres de campo con números
   * No incluya guiones &quot;-&quot; en los nombres. Esto equivaldrá a un signo menos en el código y actuará como operadores en el código.
   * Los nombres pueden contener letras, dígitos, caracteres de subrayado y signos de dólar.
   * Los nombres deben comenzar con una letra
   * Los nombres distinguen entre mayúsculas y minúsculas
   * Las palabras reservadas (como palabras clave de JavaScript) no se pueden usar como nombres. Tenga cuidado con otras palabras reservadas específicas para AF como   como &quot;panel&quot;,&quot;name&quot;.
   * No incluya guiones &quot;-&quot; en los nombres
* **Desarrollo de Forms**
   * Los fragmentos de formulario deben tenerse en cuenta al desarrollar formularios grandes. Habilitar la carga diferida de fragmentos de formulario para una carga más rápida   tiempos
   * **Modelo de datos**
      * Se recomienda asociar el formulario adaptable con el modelo de datos adecuado
   * **Eventos de objetos**
      * El código relacionado con la visibilidad de un objeto debe colocarse siempre en el evento de visibilidad de dicho objeto.
   * **Script**
      * Si el código que está escribiendo dentro de un formulario adaptable se extiende más allá de 5 líneas visibles, debe mover el código a una biblioteca de clientes. Idealmente, agregue su función a la biblioteca del cliente y, a continuación, agregue las etiquetas jsdoc correspondientes para permitir que la función esté visible en el editor de reglas de formulario adaptable.


