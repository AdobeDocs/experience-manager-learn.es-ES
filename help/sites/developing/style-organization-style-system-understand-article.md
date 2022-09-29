---
title: Explicación de las prácticas recomendadas del sistema de estilos con AEM Sites
description: Un artículo detallado que explica las prácticas recomendadas a la hora de implementar el sistema de estilos con Adobe Experience Manager Sites.
feature: Style System
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1536'
ht-degree: 3%

---

# Comprender las prácticas recomendadas del sistema de estilos{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Revise el contenido en [Cómo codificar para el sistema de estilos](style-system-technical-video-understand.md), para garantizar una comprensión de las convenciones de tipo BEM que utiliza AEM sistema de estilos.

Existen dos sabores o estilos principales que se implementan para el sistema de estilos de AEM:

* **Estilos de diseño**
* **Mostrar estilos**

**Estilos de diseño** afectan a muchos elementos de un componente para crear una representación bien definida e identificable (diseño y diseño) del componente, a menudo alineada con un concepto de marca reutilizable específico. Por ejemplo, un componente Teaser puede presentarse en el diseño tradicional basado en tarjetas, en un estilo promocional horizontal o como un diseño Hero que superponga el texto en una imagen.

**Mostrar estilos** se utilizan para afectar a las variaciones menores de los estilos de diseño, pero no cambian la naturaleza ni la intención fundamentales del estilo de diseño. Por ejemplo, un estilo de diseño principal puede tener estilos de visualización que cambian la combinación de colores de la combinación de colores de la marca principal al esquema de colores de la marca secundaria.

## Prácticas recomendadas de la organización de estilos {#style-organization-best-practices}

Al definir los nombres de estilo disponibles para AEM autores, es mejor:

* Asigne un nombre a los estilos utilizando un vocabulario entendido por los autores
* Minimizar el número de opciones de estilo
* Mostrar solo las opciones de estilo y las combinaciones permitidas por los estándares de marca
* Mostrar solo las combinaciones de estilos que tengan un efecto
   * Si se exponen combinaciones ineficaces, asegúrese de que al menos no tengan un efecto negativo

A medida que aumenta el número de combinaciones de estilos posibles disponibles para los autores de AEM, se producen más permutaciones que deben ser de control de calidad y validadas según los estándares de marca. Demasiadas opciones también pueden confundir a los autores, ya que puede que no esté claro qué opción o combinación se requiere para producir el efecto deseado.

### Nombres de estilo vs. clases CSS {#style-names-vs-css-classes}

Los nombres de estilo, o las opciones presentadas a AEM autores y los nombres de clase CSS implementados están disociados en AEM.

Esto permite que las opciones de Estilo se etiqueten con un vocabulario claro y entendido por los autores AEM, pero permite a los desarrolladores de CSS nombrar las clases CSS de una forma semántica y a prueba del futuro. Por ejemplo:

Un componente debe tener las opciones para colorearlo con la marca **principal** y **secundario** los colores, sin embargo, los autores AEM conocen los colores como **green** y **amarillo**, en lugar del idioma de diseño de primaria y secundaria.

El sistema de estilos de AEM puede exponer estos estilos de visualización de color utilizando etiquetas compatibles con el autor **Verde** y **Amarillo**, permitiendo a los desarrolladores de CSS utilizar la nomenclatura semántica de `.cmp-component--primary-color` y `.cmp-component--secondary-color` para definir la implementación de estilo real en CSS.

El nombre de estilo de **Verde** está asignado a `.cmp-component--primary-color`y **Amarillo** a `.cmp-component--secondary-color`.

Si el color de la marca de la empresa cambia en el futuro, todo lo que debe cambiarse son las implementaciones únicas de `.cmp-component--primary-color` y `.cmp-component--secondary-color`y los nombres de estilo.

## El componente Teaser como ejemplo de caso de uso {#the-teaser-component-as-an-example-use-case}

A continuación se muestra un ejemplo de uso de estilo de un componente Teaser para tener varios estilos de Diseño y Visualización diferentes.

Esto explorará cómo se organizan los nombres de estilo (expuestos a los autores) y cómo se organizan las clases CSS de respaldo.

### Configuración de estilos de componentes de teaser {#component-styles-configuration}

La siguiente imagen muestra la variable [!UICONTROL Estilos] configuración del componente Teaser para las variaciones tratadas en el caso de uso.

La variable [!UICONTROL Grupo de estilos] los nombres, el diseño y la visualización, por casualidad, coinciden con los conceptos generales de los estilos de visualización y el diseño utilizados para categorizar conceptualmente los tipos de estilos de este artículo.

La variable [!UICONTROL Grupo de estilos] nombres y número de [!UICONTROL Grupos de estilos] deben adaptarse al caso de uso de los componentes y a las convenciones de estilo de los componentes específicas del proyecto.

Por ejemplo, la variable **Mostrar** puede que se haya nombrado el nombre del grupo de estilos **Colores**.

![Mostrar grupo de estilos](assets/style-config.png)

### Menú Selección de estilo {#style-selection-menu}

La imagen siguiente muestra la variable [!UICONTROL Estilo] los autores del menú interactúan con para seleccionar los estilos adecuados para el componente. Tenga en cuenta que [!UICONTROL Gráfico de estilos] los nombres, así como los nombres de estilo, están expuestos al autor.

![Menú desplegable Estilo](assets/style-menu.png)

### Estilo predeterminado {#default-style}

El estilo predeterminado suele ser el estilo más utilizado del componente y la vista predeterminada sin estilo del teaser cuando se añade a una página.

Según la comunidad del estilo predeterminado, el CSS puede aplicarse directamente en la variable `.cmp-teaser` (sin modificadores) o en un `.cmp-teaser--default`.

Si las reglas de estilo predeterminadas se aplican con mayor frecuencia que no a todas las variaciones, es mejor utilizar `.cmp-teaser` como clases CSS del estilo predeterminado, ya que todas las variaciones deben heredarlas implícitamente, suponiendo que se sigan convenciones como BEM. Si no es así, se deben aplicar mediante el modificador predeterminado, como `.cmp-teaser--default`, que a su vez debe añadirse al [Clases CSS predeterminadas de la configuración de estilo del componente](#component-styles-configuration) , de lo contrario, estas reglas de estilo tendrán que anularse en cada variación.

Incluso es posible asignar un estilo &quot;con nombre&quot; como estilo predeterminado, por ejemplo, el estilo Hero `(.cmp-teaser--hero)` definida a continuación, sin embargo, es más claro implementar el estilo predeterminado frente a la variable `.cmp-teaser` o `.cmp-teaser--default` Implementaciones de clases CSS.

>[!NOTE]
>
>Observe que el estilo de diseño predeterminado NO tiene un nombre de estilo de visualización; sin embargo, el autor puede seleccionar una opción de visualización en la herramienta de selección Sistema de estilos de AEM.
>
>Esto infringe las prácticas recomendadas:
>
>**Mostrar solo las combinaciones de estilos que tengan un efecto**
>
>Si un autor selecciona el estilo de visualización de **Verde** no pasará nada.
>
>En este caso de uso, reconoceremos esta infracción, ya que todos los demás estilos de Diseño deben ser coloreables utilizando los colores de la marca.
>
>En el **Promoción (alineado a la derecha)** a continuación, se muestra cómo evitar combinaciones de estilos no deseadas.

![estilo predeterminado](assets/default.png)

* **Estilo de presentación**
   * Predeterminado
* **Mostrar estilo**
   * Ninguno
* **Clases CSS efectivas**: `.cmp-teaser--promo` o `.cmp-teaser--default`

### Estilo de promoción {#promo-style}

La variable **Estilo de diseño de promoción** se utiliza para promocionar contenido de alto valor en el sitio, se coloca horizontalmente para ocupar una banda de espacio en la página web y debe ser estilo por colores de marca, con el estilo de diseño Promo predeterminado utilizando texto negro.

Para lograrlo, **estilo de presentación** de **Promoción** y **mostrar estilos** de **Verde** y **Amarillo** se configuran en el sistema de estilos de AEM para el componente teaser.

#### Promo predeterminado

![promo predeterminado](assets/promo-default.png)

* **Estilo de presentación**
   * Nombre del estilo: **Promoción**
   * Clase de CSS: `cmp-teaser--promo`
* **Mostrar estilo**
   * Ninguno
* **Clases CSS efectivas**: `.cmp-teaser--promo`

#### Principal de promoción

![principal de promoción](assets/promo-primary.png)

* **Estilo de presentación**
   * Nombre del estilo: **Promoción**
   * Clase de CSS: `cmp-teaser--promo`
* **Mostrar estilo**
   * Nombre del estilo: **Verde**
   * Clase de CSS: `cmp-teaser--primary-color`
* **Clases CSS efectivas**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promo secundario

![Promo secundario](assets/promo-secondary.png)

* **Estilo de presentación**
   * Nombre del estilo: **Promoción**
   * Clase de CSS: `cmp-teaser--promo`
* **Mostrar estilo**
   * Nombre del estilo: **Amarillo**
   * Clase de CSS: `cmp-teaser--secondary-color`
* **Clases CSS efectivas**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Promo Estilo alineado a la derecha {#promo-r-align}

La variable **Promoción alineada a la derecha** el estilo de presentación es una variación del estilo de promoción que cambia la ubicación de la imagen y el texto (imagen a la derecha, texto a la izquierda).

La alineación derecha, en su núcleo, es un estilo de visualización; podría introducirse en el sistema de estilos de AEM como un estilo de visualización seleccionado junto con el estilo de diseño de promoción. Esto infringe la práctica recomendada de:

**Mostrar solo las combinaciones de estilos que tengan un efecto**

...que ya se ha violado en el [Estilo predeterminado](#default-style).

Dado que la alineación derecha solo afecta al estilo de diseño de la promoción y no a los otros 2 estilos de diseño: De forma predeterminada y como héroe, podemos crear una nueva promoción de estilo de diseño (alineada a la derecha) que incluya la clase CSS que alinea a la derecha el contenido de los estilos de diseño de promoción: `cmp -teaser--alternate`.

Esta combinación de varios estilos en una sola entrada de estilo también puede ayudar a reducir el número de estilos disponibles y las permutaciones de estilo, que es mejor minimizar.

Observe el nombre de la clase CSS, `cmp-teaser--alternate`, no tiene que coincidir con la nomenclatura de &quot;alineado a la derecha&quot;, fácil de usar para el autor.

#### Promo Alinear a la derecha de forma predeterminada

![promoción alineada a la derecha](assets/promo-alternate-default.png)

* **Estilo de presentación**
   * Nombre del estilo: **Promoción (alineado a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Mostrar estilo**
   * Ninguno
* **Clases CSS efectivas**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Promo Primario alineado a la derecha

![Promo Primario alineado a la derecha](assets/promo-alternate-primary.png)

* **Estilo de presentación**
   * Nombre del estilo: **Promoción (alineado a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Mostrar estilo**
   * Nombre del estilo: **Verde**
   * Clase de CSS: `cmp-teaser--primary-color`
* **Clases CSS efectivas**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Promo Secundario alineado a la derecha

![Promo Secundario alineado a la derecha](assets/promo-alternate-secondary.png)

* **Estilo de presentación**
   * Nombre del estilo: **Promoción (alineado a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Mostrar estilo**
   * Nombre del estilo: **Amarillo**
   * Clase de CSS: `cmp-teaser--secondary-color`
* **Clases CSS efectivas**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Estilo principal {#hero-style}

El estilo de diseño principal muestra la imagen de los componentes como fondo, con el título y el vínculo superpuestos. El estilo de la presentación principal, al igual que el estilo de la presentación promocional, debe ser coloreado con colores de marca.

Para colorear el estilo de diseño principal con colores de marca, se pueden aprovechar los mismos estilos de visualización utilizados para el estilo de diseño promocional.

Por componente, el nombre del estilo se asigna al conjunto único de clases CSS, lo que significa que los nombres de las clases CSS que colorean el fondo del estilo de diseño de promoción deben colorear el texto y el vínculo del estilo de diseño principal.

Esto se puede lograr trivialmente si se definen las reglas CSS. Sin embargo, esto requiere que los desarrolladores de CSS comprendan cómo se implementan estas permutaciones en AEM.

CSS para colorear el fondo del **Promocionar** estilo de presentación con el color principal (verde):

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS para colorear el texto del **Hero** estilo de presentación con el color principal (verde):

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero predeterminado

![Estilo principal](assets/hero.png)

* **Estilo de presentación**
   * Nombre del estilo: **Hero**
   * Clase de CSS: `cmp-teaser--hero`
* **Mostrar estilo**
   * Ninguno
* **Clases CSS efectivas**: `.cmp-teaser--hero`

#### Primario principal

![Primario principal](assets/hero-primary.png)

* **Estilo de presentación**
   * Nombre del estilo: **Promoción**
   * Clase de CSS: `cmp-teaser--hero`
* **Mostrar estilo**
   * Nombre del estilo: **Verde**
   * Clase de CSS: `cmp-teaser--primary-color`
* **Clases CSS efectivas**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Secundario principal

![Secundario principal](assets/hero-secondary.png)

* **Estilo de presentación**
   * Nombre del estilo: **Promoción**
   * Clase de CSS: `cmp-teaser--hero`
* **Mostrar estilo**
   * Nombre del estilo: **Amarillo**
   * Clase de CSS: `cmp-teaser--secondary-color`
* **Clases CSS efectivas**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Recursos adicionales {#additional-resources}

* [Documentación del sistema de estilos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creación de bibliotecas de cliente AEM](https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sitio web de documentación de BEM (Modificador de elemento de bloque)](https://getbem.com/)
* [Sitio web de documentación de LESS](https://lesscss.org/)
* [Sitio web de jQuery](https://jquery.com/)
