---
title: Explicación de las prácticas recomendadas del sistema de estilos con AEM Sites
description: Un artículo detallado que explica las prácticas recomendadas para implementar el sistema de estilos con Adobe Experience Manager Sites.
feature: style-system
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1538'
ht-degree: 2%

---


# Explicación de las prácticas recomendadas del sistema de estilos{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Revise el contenido en [Explicación del código del sistema](style-system-technical-video-understand.md)de estilos para asegurarse de comprender las convenciones de tipo BEM utilizadas por AEM sistema de estilos.

Existen dos estilos o sabores principales que se implementan para el sistema de estilos AEM:

* **Estilos de diseño**
* **Mostrar estilos**

**Los estilos** de diseño afectan a muchos elementos de un componente para crear una representación bien definida e identificable (diseño y diseño) del componente, a menudo alineándose con un concepto de marca reutilizable específico. Por ejemplo, un componente Teaser puede presentarse en el diseño tradicional basado en tarjetas, en un estilo promocional horizontal o como un diseño Hero superpuesto en el texto de una imagen.

**Los estilos** de visualización se utilizan para afectar a las variaciones menores de los estilos de diseño. Sin embargo, no cambian la naturaleza o la intención fundamentales del estilo de diseño. Por ejemplo, un estilo de diseño Hero puede tener estilos de visualización que cambian la combinación de colores de la combinación de colores de la marca principal a la combinación de colores de la marca secundaria.

## Optimizaciones de la organización de estilos {#style-organization-best-practices}

Al definir los nombres de estilo disponibles para AEM autores, es mejor:

* Asignar nombres a los estilos con un vocabulario que comprendan los autores
* Minimizar el número de opciones de estilo
* Exponer solo las opciones de estilo y las combinaciones permitidas por los estándares de marca
* Exponer solo las combinaciones de estilo que tengan un efecto
   * Si se exponen combinaciones ineficaces, asegúrese de que al menos no tengan un efecto negativo

A medida que aumenta el número de posibles combinaciones de estilo disponibles para AEM autores, se producen más permutaciones que deben ser de control de calidad y validadas en relación con los estándares de marca. Demasiadas opciones también pueden confundir a los autores, ya que podría no estar claro qué opción o combinación se necesita para producir el efecto deseado.

### Nombres de estilo vs. clases CSS {#style-names-vs-css-classes}

Los nombres de estilo, o las opciones presentadas a AEM autores, y los nombres de clase CSS de implementación están disociados en AEM.

Esto permite que las opciones de estilo se etiqueten con un vocabulario claro y entendido por los autores AEM, pero permite a los desarrolladores de CSS nombrar las clases CSS de una manera semántica y de futura prueba. Por ejemplo:

Un componente debe tener las opciones para colorear con los colores **primarios** y **secundarios** de la marca; sin embargo, los autores de AEM conocen los colores como **verdes** y **amarillos**, en lugar del lenguaje de diseño de primaria y secundaria.

El sistema de estilos de AEM puede exponer estos estilos de visualización de color mediante etiquetas descriptivas para el autor **Verde** y **Amarillo**, al tiempo que permite a los desarrolladores de CSS utilizar la nomenclatura semántica de `.cmp-component--primary-color` y `.cmp-component--secondary-color` definir la implementación de estilo real en CSS.

El nombre de estilo de **Verde** se asigna a `.cmp-component--primary-color`y **Amarillo** a `.cmp-component--secondary-color`.

Si el color de la marca de la compañía cambia en el futuro, todo lo que debe cambiarse son las implementaciones únicas de `.cmp-component--primary-color` y `.cmp-component--secondary-color`, y los nombres de estilo.

## El componente Teaser como caso de uso de ejemplo {#the-teaser-component-as-an-example-use-case}

A continuación se muestra un ejemplo de caso de uso de un componente Teaser para aplicar estilo a varios estilos de presentación y visualización diferentes.

Esto explorará cómo se organizan los nombres de estilo (expuestos a los autores) y cómo se organizan las clases CSS de respaldo.

### Configuración de estilos de componente teaser {#component-styles-configuration}

La siguiente imagen muestra la configuración de [!UICONTROL estilos] del componente Teaser para las variaciones analizadas en el caso de uso.

Los nombres de grupos [!UICONTROL de] estilos, Presentación y Presentación, por casualidad, coinciden con los conceptos generales de estilos de visualización y estilos de diseño utilizados para categorizar conceptualmente los tipos de estilos en este artículo.

Los nombres de los grupos [!UICONTROL de] estilos y el número de grupos [!UICONTROL de] estilos deben adaptarse al caso de uso de los componentes y a las convenciones de estilo de los componentes específicos del proyecto.

Por ejemplo, el nombre del grupo de estilos de **visualización** podría haberse denominado **Colores**.

![Mostrar grupo de estilos](assets/style-config.png)

### Menú de selección de estilo {#style-selection-menu}

La siguiente imagen muestra la interacción de los autores del menú [!UICONTROL Estilo] para seleccionar los estilos adecuados para el componente. Tenga en cuenta que los nombres de [!UICONTROL Estilos] , así como los nombres de Estilo, están expuestos al autor.

![Menú desplegable Estilo](assets/style-menu.png)

### Default style {#default-style}

El estilo predeterminado suele ser el estilo más utilizado del componente y la vista predeterminada sin estilo del teaser cuando se agrega a una página.

En función de las características comunes del estilo predeterminado, el CSS puede aplicarse directamente en el `.cmp-teaser` (sin modificadores) o en un `.cmp-teaser--default`.

Si las reglas de estilo predeterminadas se aplican con mayor frecuencia que no a todas las variaciones, es mejor utilizarlas `.cmp-teaser` como clases CSS del estilo predeterminado, ya que todas las variaciones deben heredarlas implícitamente, suponiendo que se siguen convenciones parecidas a BEM. Si no es así, deben aplicarse mediante el modificador predeterminado, como `.cmp-teaser--default`, que a su vez debe agregarse al campo Clases [CSS predeterminadas de la configuración de estilo del](#component-styles-configuration) componente; de lo contrario, estas reglas de estilo deberán anularse en cada variación.

Incluso es posible asignar un estilo &quot;con nombre&quot; como estilo predeterminado, por ejemplo, el estilo Hero `(.cmp-teaser--hero)` definido a continuación. Sin embargo, es más claro implementar el estilo predeterminado en las implementaciones de clase `.cmp-teaser` o `.cmp-teaser--default` CSS.

>[!NOTE]
>
>Observe que el estilo de diseño Predeterminado NO tiene un nombre de estilo de visualización; sin embargo, el autor podrá seleccionar una opción de visualización en la herramienta de selección Sistema de estilo de AEM.
>
>Esto infringe las mejores prácticas:
>
>**Exponer solo las combinaciones de estilo que tengan un efecto**
>
>Si un autor selecciona el estilo de visualización de **Verde** , no sucederá nada.
>
>En este caso de uso, reconoceremos esta infracción, ya que todos los demás estilos de diseño deben ser coloreables con los colores de la marca.
>
>En la sección **Promo (alineado a la derecha)** que aparece a continuación, veremos cómo evitar combinaciones de estilo no deseadas.

![estilo predeterminado](assets/default.png)

* **Estilo de diseño**
   * Predeterminado
* **Estilo de visualización**
   * Ninguna
* **Clases** CSS efectivas: `.cmp-teaser--promo` o `.cmp-teaser--default`

### Estilo de promoción {#promo-style}

El estilo **de diseño** Promo se utiliza para promocionar contenido de alto valor en el sitio y se coloca horizontalmente para ocupar una banda de espacio en la página web y debe ser estilo por colores de marca, con el estilo de diseño Promo predeterminado utilizando texto en negro.

Para conseguirlo, un estilo **de** diseño de **Promo** y los estilos **de** visualización de **Verde** y **Amarillo** se configuran en el sistema de estilo de AEM para el componente Teaser.

#### Promo predeterminado

![promo predeterminado](assets/promo-default.png)

* **Estilo de diseño**
   * Nombre del estilo: **nombre**
   * Clase de CSS: `cmp-teaser--promo`
* **Estilo de visualización**
   * Ninguna
* **Clases** CSS efectivas: `.cmp-teaser--promo`

#### Principal de promoción

![principal de promoción](assets/promo-primary.png)

* **Estilo de diseño**
   * Nombre del estilo: **nombre**
   * Clase de CSS: `cmp-teaser--promo`
* **Estilo de visualización**
   * Nombre del estilo: **: Verde**
   * Clase de CSS: `cmp-teaser--primary-color`
* **Clases** CSS efectivas: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promo Secundario

![Promo Secundario](assets/promo-secondary.png)

* **Estilo de diseño**
   * Nombre del estilo: **Promoción**
   * Clase de CSS: `cmp-teaser--promo`
* **Estilo de visualización**
   * Nombre del estilo: **Amarillo**
   * Clase de CSS: `cmp-teaser--secondary-color`
* **Clases** CSS efectivas: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Promo Estilo alineado a la derecha {#promo-r-align}

El estilo de diseño **Promo alineado** a la derecha es una variación del estilo Promo que cambia la ubicación de la imagen y el texto (imagen a la derecha, texto a la izquierda).

La alineación derecha, en su núcleo, es un estilo de visualización; se puede introducir en el sistema de estilos de AEM como un estilo de visualización que se selecciona junto con el estilo de diseño de promoción. Esto infringe la mejor práctica de:

**Exponer solo las combinaciones de estilo que tengan un efecto**

..que ya se ha infringido en el estilo [](#default-style)Predeterminado.

Dado que la alineación a la derecha solo afecta al estilo de diseño Promo y no a los otros 2 estilos de diseño: de forma predeterminada y primordial, podemos crear un nuevo estilo de diseño Promo (alineado a la derecha) que incluya la clase CSS que alinea a la derecha el contenido de los estilos de diseño Promo: `cmp -teaser--alternate`.

Esta combinación de varios estilos en una sola entrada de estilo también puede ayudar a reducir el número de estilos disponibles y las permutaciones de estilo, lo que resulta más adecuado para minimizar.

Observe que el nombre de la clase CSS, `cmp-teaser--alternate`, no tiene que coincidir con la nomenclatura descriptiva del autor de &quot;alineado a la derecha&quot;.

#### Promo alineado a la derecha predeterminado

![promoción alineada a la derecha](assets/promo-alternate-default.png)

* **Estilo de diseño**
   * Nombre del estilo: **Promo (alineado a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de visualización**
   * Ninguna
* **Clases** CSS efectivas: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Promo alineado a la derecha principal

![Promo alineado a la derecha principal](assets/promo-alternate-primary.png)

* **Estilo de diseño**
   * Nombre del estilo: **Promo (alineado a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de visualización**
   * Nombre del estilo: **Verde**
   * Clase de CSS: `cmp-teaser--primary-color`
* **Clases** CSS efectivas: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Promo alineado a la derecha secundario

![Promo alineado a la derecha secundario](assets/promo-alternate-secondary.png)

* **Estilo de diseño**
   * Nombre del estilo: **Promo (alineado a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de visualización**
   * Nombre del estilo: **Amarillo**
   * Clase de CSS: `cmp-teaser--secondary-color`
* **Clases** CSS efectivas: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Estilo Hero {#hero-style}

El estilo de diseño Hero muestra la imagen de los componentes como fondo con el título y el vínculo superpuestos. El estilo de la maquetación Hero, al igual que el estilo de la maquetación Promo, debe ser colorible con colores de marca.

Para colorear el estilo de diseño Hero con colores de marca, se pueden aprovechar los mismos estilos de visualización utilizados para el estilo de diseño Promo.

Por componente, el nombre del estilo se asigna al conjunto único de clases CSS, lo que significa que los nombres de clase CSS que colorean el fondo del estilo de diseño Promo deben colorear el texto y el vínculo del estilo de diseño Hero.

Esto se puede lograr trivialmente si se establecen las reglas CSS. Sin embargo, esto requiere que los desarrolladores de CSS comprendan cómo se implementarán estas permutaciones en AEM.

CSS para colorear el fondo del estilo de diseño **Promote** con el color principal (verde):

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS para colorear el texto del estilo de diseño **Hero** con el color principal (verde):

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Valor predeterminado de Héroe

![Estilo de héroe](assets/hero.png)

* **Estilo de diseño**
   * Nombre del estilo: **Héroe**
   * Clase de CSS: `cmp-teaser--hero`
* **Estilo de visualización**
   * Ninguna
* **Clases** CSS efectivas: `.cmp-teaser--hero`

#### Primario Héroe

![Primario Héroe](assets/hero-primary.png)

* **Estilo de diseño**
   * Nombre del estilo: **Promoción**
   * Clase de CSS: `cmp-teaser--hero`
* **Estilo de visualización**
   * Nombre del estilo: **Verde**
   * Clase de CSS: `cmp-teaser--primary-color`
* **Clases** CSS efectivas: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero Secundario

![Hero Secundario](assets/hero-secondary.png)

* **Estilo de diseño**
   * Nombre del estilo: **nombre**
   * Clase de CSS: `cmp-teaser--hero`
* **Estilo de visualización**
   * Nombre del estilo: **Amarillo**
   * Clase de CSS: `cmp-teaser--secondary-color`
* **Clases** CSS efectivas: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Recursos adicionales {#additional-resources}

* [Documentación del sistema de estilos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creación de bibliotecas de AEM cliente](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sitio web de documentación de BEM (Modificador de elementos de bloque)](https://getbem.com/)
* [Sitio web de documentación de LESS](https://lesscss.org/)
* [Sitio Web de jQuery](https://jquery.com/)
