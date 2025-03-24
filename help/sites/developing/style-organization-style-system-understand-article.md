---
title: Explicación de las prácticas recomendadas del sistema de estilos con AEM Sites
description: Artículo detallado que explica las prácticas recomendadas a la hora de implementar el sistema de estilos con Adobe Experience Manager Sites.
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Article
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
duration: 328
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# Prácticas recomendadas para el sistema de estilos{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Revise el contenido en [Explicación de cómo codificar para el sistema de estilos](style-system-technical-video-understand.md), para garantizar una comprensión de las convenciones de tipo BEM utilizadas por el sistema de estilos de AEM.

Existen dos tipos o estilos principales implementados para el sistema de estilos de AEM:

* **Estilos de diseño**
* **Estilos de visualización**

**Los estilos de diseño** afectan a muchos elementos de un componente para crear una representación bien definida e identificable (diseño y diseño) del componente, a menudo alineándose con un concepto de marca reutilizable específico. Por ejemplo, un componente Teaser puede presentarse en el diseño tradicional basado en tarjetas, en un estilo promocional horizontal o como un diseño Hero que superponga el texto en una imagen.

**Los estilos de presentación** se utilizan para modificar variaciones menores de los estilos de presentación, pero no cambian la naturaleza ni la intención fundamentales del estilo de presentación. Por ejemplo, un estilo de diseño Hero puede tener estilos de presentación que cambian el esquema de colores de la combinación de colores de marca principal a la combinación de colores de marca secundaria.

## Prácticas recomendadas de organización de estilos {#style-organization-best-practices}

Al definir los nombres de estilo disponibles para los autores de AEM, lo mejor es:

* Nombrar estilos utilizando un vocabulario comprendido por los autores
* Minimizar el número de opciones de estilo
* Exponer solo las opciones de estilo y las combinaciones permitidas por los estándares de marca
* Exponer sólo las combinaciones de estilos que tienen un efecto
   * Si se exponen combinaciones ineficaces, hay que asegurarse de que al menos no tengan un efecto perjudicial

A medida que aumenta el número de combinaciones de estilos posibles disponibles para los autores de AEM, existen más permutaciones que deben someterse a un control de calidad y validarse frente a los estándares de marca. Demasiadas opciones también pueden confundir a los autores, ya que puede resultar confuso qué opción o combinación se necesita para producir el efecto deseado.

### Nombres de estilo frente a clases CSS {#style-names-vs-css-classes}

Los nombres de estilo, o las opciones presentadas a los autores de AEM, y los nombres de clase CSS de implementación están disociados en AEM.

Esto permite que las opciones de Estilo se etiqueten con un vocabulario claro y entendido por los autores de AEM, pero permite a los desarrolladores de CSS nombrar las clases CSS de una manera semántica y preparada para el futuro. Por ejemplo:

Un componente debe tener las opciones para que se le apliquen colores con los colores **principal** y **secundario** de la marca; sin embargo, los autores de AEM conocen los colores como **verde** y **amarillo**, en lugar del idioma de diseño principal y secundario.

El sistema de estilos de AEM puede exponer estos estilos de presentación para colorear mediante las etiquetas **Verde** y **Amarillo** que admiten el autor, a la vez que permite a los desarrolladores de CSS usar nombres semánticos de `.cmp-component--primary-color` y `.cmp-component--secondary-color` para definir la implementación de estilo real en CSS.

El nombre de estilo de **Green** está asignado a `.cmp-component--primary-color` y **Yellow** a `.cmp-component--secondary-color`.

Si el color de marca de la compañía cambia en el futuro, todo lo que debe cambiarse son las implementaciones únicas de `.cmp-component--primary-color` y `.cmp-component--secondary-color`, y los nombres de Style.

## El componente Teaser como ejemplo de uso {#the-teaser-component-as-an-example-use-case}

A continuación se muestra un ejemplo de uso del estilo de un componente Teaser para tener varios estilos diferentes de Diseño y Visualización.

Esto explorará cómo se organizan los nombres de estilo (expuestos a autores) y las clases CSS de respaldo.

### Configuración de estilos del componente Teaser {#component-styles-configuration}

La siguiente imagen muestra la configuración de [!UICONTROL Styles] para el componente Teaser para las variaciones analizadas en el caso de uso.

Los nombres, el diseño y la presentación del grupo de estilos [!UICONTROL Style Group] coinciden, por casualidad, con los conceptos generales de los estilos de presentación y los estilos de presentación utilizados para categorizar conceptualmente los tipos de estilos en este artículo.

Los nombres de [!UICONTROL Style Group] y el número de [!UICONTROL Style Groups] deben adaptarse al caso de uso del componente y a las convenciones de estilo de componentes específicas del proyecto.

Por ejemplo, el nombre del grupo de estilos **Pantalla** podría haberse denominado **Colores**.

![Grupo de estilos de visualización](assets/style-config.png)

### Menú de selección de estilo {#style-selection-menu}

La siguiente imagen muestra el menú [!UICONTROL Estilo] con el que interactúan los autores para seleccionar los estilos adecuados para el componente. Tenga en cuenta que los nombres [!UICONTROL Style Graph], así como los nombres Style, se exponen al autor.

![Menú desplegable Estilo](assets/style-menu.png)

### Estilo predeterminado {#default-style}

El estilo predeterminado suele ser el estilo más utilizado del componente y la vista predeterminada sin estilo del teaser cuando se agrega a una página.

Según lo común del estilo predeterminado, CSS se puede aplicar directamente en `.cmp-teaser` (sin ningún modificador) o en `.cmp-teaser--default`.

Si las reglas de estilo predeterminadas se aplican con mayor frecuencia que no a todas las variaciones, es mejor utilizar `.cmp-teaser` como las clases CSS del estilo predeterminado, ya que todas las variaciones deben heredarlas implícitamente, suponiendo que se sigan convenciones de tipo BEM. Si no es así, se deben aplicar mediante el modificador predeterminado, como `.cmp-teaser--default`, que a su vez debe agregarse al campo Clases CSS predeterminadas](#component-styles-configuration) de la configuración de estilo del componente [de lo contrario, estas reglas de estilo deberán anularse en cada variación.

Incluso es posible asignar un estilo &quot;con nombre&quot; como estilo predeterminado, por ejemplo, el estilo Hero `(.cmp-teaser--hero)` definido a continuación, aunque es más claro implementar el estilo predeterminado en las implementaciones de clase CSS `.cmp-teaser` o `.cmp-teaser--default`.

>[!NOTE]
>
>Observe que el estilo de diseño predeterminado NO tiene un nombre de estilo de visualización, sin embargo, el autor puede seleccionar una opción de visualización en la herramienta de selección Sistema de estilos de AEM.
>
>Esto infringe la práctica recomendada:
>
>**Exponer solo las combinaciones de estilos que tienen un efecto**
>
>Si un autor selecciona el estilo de visualización **Verde**, no ocurrirá nada.
>
>En este caso de uso, admitiremos esta infracción, ya que todos los demás estilos de diseño deben ser coloreables con los colores de la marca.
>
>En la sección **Promoción (alineada a la derecha)** a continuación veremos cómo evitar combinaciones de estilos no deseadas.

![estilo predeterminado](assets/default.png)

* **Estilo de diseño**
   * Predeterminado
* **Estilo de visualización**
   * Ninguno
* **Clases CSS efectivas**: `.cmp-teaser--promo` o `.cmp-teaser--default`

### Estilo de promoción {#promo-style}

El **estilo de diseño de Promoción** se usa para promocionar contenido de alto valor en el sitio, se coloca horizontalmente para ocupar una banda de espacio en la página web y debe poder aplicar estilos con colores de marca, con el estilo de diseño de Promoción predeterminado que utiliza texto en negro.

Para conseguirlo, un **estilo de diseño** de **Promoción** y los **estilos de visualización** de **Verde** y **Amarillo** están configurados en el sistema de estilos de AEM para el componente Teaser.

#### Predeterminado de promoción

![promo predeterminada](assets/promo-default.png)

* **Estilo de diseño**
   * Nombre de estilo: **Promoción**
   * Clase CSS: `cmp-teaser--promo`
* **Estilo de visualización**
   * Ninguno
* **Clases CSS efectivas**: `.cmp-teaser--promo`

#### Promoción principal

![promoción principal](assets/promo-primary.png)

* **Estilo de diseño**
   * Nombre de estilo: **Promoción**
   * Clase CSS: `cmp-teaser--promo`
* **Estilo de visualización**
   * Nombre de estilo: **Verde**
   * Clase CSS: `cmp-teaser--primary-color`
* **Clases CSS efectivas**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promoción secundaria

![Promoción secundaria](assets/promo-secondary.png)

* **Estilo de diseño**
   * Nombre de estilo: **Promoción**
   * Clase CSS: `cmp-teaser--promo`
* **Estilo de visualización**
   * Nombre de estilo: **Amarillo**
   * Clase CSS: `cmp-teaser--secondary-color`
* **Clases CSS efectivas**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Promoción Estilo alineado a la derecha {#promo-r-align}

El estilo de diseño **Promoción alineada a la derecha** es una variación del estilo Promoción que gira la ubicación de la imagen y el texto (imagen a la derecha, texto a la izquierda).

La alineación correcta, en su núcleo, es un estilo de visualización, podría introducirse en el sistema de estilos de AEM como un estilo de visualización que se selecciona junto con el estilo de diseño Promoción. Esto infringe la práctica recomendada de:

**Exponer solo las combinaciones de estilos que tienen un efecto**

..que ya se violó en el [estilo predeterminado](#default-style).

Dado que la alineación correcta solo afecta al estilo de diseño Promoción y no a los otros 2 estilos de diseño: predeterminado y a pantalla completa, podemos crear un nuevo estilo de diseño Promoción (alineado a la derecha) que incluya la clase CSS que alinea a la derecha el contenido de los estilos de diseño Promoción: `cmp -teaser--alternate`.

Esta combinación de varios estilos en una sola entrada de estilo también puede ayudar a reducir el número de estilos disponibles y las permutaciones de estilo, lo que es mejor minimizar.

Observe que el nombre de la clase CSS, `cmp-teaser--alternate`, no tiene que coincidir con la nomenclatura compatible con el autor de &quot;alineado a la derecha&quot;.

#### Promoción predeterminada alineada a la derecha

![promoción alineada a la derecha](assets/promo-alternate-default.png)

* **Estilo de diseño**
   * Nombre de estilo: **Promoción (alineada a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de visualización**
   * Ninguno
* **Clases CSS efectivas**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Promoción principal alineada a la derecha

![Promoción principal alineada a la derecha](assets/promo-alternate-primary.png)

* **Estilo de diseño**
   * Nombre de estilo: **Promoción (alineada a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de visualización**
   * Nombre de estilo: **Verde**
   * Clase CSS: `cmp-teaser--primary-color`
* **Clases CSS efectivas**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Promoción secundaria alineada a la derecha

![Promoción secundaria alineada a la derecha](assets/promo-alternate-secondary.png)

* **Estilo de diseño**
   * Nombre de estilo: **Promoción (alineada a la derecha)**
   * Clases CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de visualización**
   * Nombre de estilo: **Amarillo**
   * Clase CSS: `cmp-teaser--secondary-color`
* **Clases CSS efectivas**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Estilo Hero {#hero-style}

El estilo de diseño Hero muestra la imagen de los componentes como fondo con el título y el vínculo superpuestos. El estilo de diseño Hero, como el estilo de diseño Promoción, debe ser coloreable con colores de marca.

Para aplicar color al estilo Diseño principal con colores de marca, se pueden aprovechar los mismos estilos de visualización utilizados para el estilo de diseño Promoción.

Por componente, el nombre del estilo se asigna al único conjunto de clases CSS, lo que significa que los nombres de clase CSS que dan color al fondo del estilo del diseño Promoción deben dar color al texto y al vínculo del estilo del diseño Hero.

Esto se puede lograr trivialmente estableciendo un ámbito para las reglas CSS, sin embargo, esto requiere que los desarrolladores de CSS entiendan cómo se implementan estas permutaciones en AEM.

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

#### Hero predeterminado

![Estilo Hero](assets/hero.png)

* **Estilo de diseño**
   * Nombre de estilo: **Hero**
   * Clase CSS: `cmp-teaser--hero`
* **Estilo de visualización**
   * Ninguno
* **Clases CSS efectivas**: `.cmp-teaser--hero`

#### Hero principal

![Héroe principal](assets/hero-primary.png)

* **Estilo de diseño**
   * Nombre de estilo: **Promoción**
   * Clase CSS: `cmp-teaser--hero`
* **Estilo de visualización**
   * Nombre de estilo: **Verde**
   * Clase CSS: `cmp-teaser--primary-color`
* **Clases CSS efectivas**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero secundario

![Héroe secundario](assets/hero-secondary.png)

* **Estilo de diseño**
   * Nombre de estilo: **Promoción**
   * Clase CSS: `cmp-teaser--hero`
* **Estilo de visualización**
   * Nombre de estilo: **Amarillo**
   * Clase CSS: `cmp-teaser--secondary-color`
* **Clases CSS efectivas**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Recursos adicionales {#additional-resources}

* [Documentación del sistema de estilos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creando bibliotecas de cliente de AEM](https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sitio web de documentación de BEM (Modificador de elementos de bloque)](https://getbem.com/)
* [MENOS sitio web de documentación](https://lesscss.org/)
* [sitio web jQuery](https://jquery.com/)
