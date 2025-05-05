---
title: Cómo codificar para el sistema de estilos de AEM
description: En este vídeo analizaremos la anatomía de CSS (o LESS) y JavaScript utilizados para aplicar estilo al componente de título principal de Adobe Experience Manager mediante el sistema de estilos, así como la forma en que estos estilos se aplican al HTML y al DOM.
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1005
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 0%

---

# Explicación de cómo codificar para el sistema de estilos{#understanding-how-to-code-for-the-aem-style-system}

En este vídeo analizaremos la estructura del CSS (o [!DNL LESS]) y JavaScript utilizados para aplicar estilo al componente de título principal de Experience Manager mediante el sistema de estilos, así como la forma en que se aplican estos estilos al HTML y al DOM.


## Explicación de cómo codificar para el sistema de estilos {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/40162?quality=12&learn=on&captions=spa)

El paquete de AEM proporcionado (**technical-review.sites.style-system-1.0.0.zip**) instala el estilo de título de ejemplo, las directivas de ejemplo para los componentes Contenedor de diseño y Título de We.Retail y una página de muestra.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### El CSS {#the-css}

La siguiente es la definición de [!DNL LESS] para el estilo de ejemplo encontrado en:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Para aquellos que prefieren CSS, debajo de este fragmento de código está el CSS que este [!DNL LESS] compila en.

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

Experience Manager compila de forma nativa el [!DNL LESS] anterior en el CSS siguiente.

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### El JavaScript {#example-javascript}

La siguiente JavaScript recopila e inserta la fecha y la hora de la última modificación de la página actual debajo del texto del título cuando se aplica el estilo de ejemplo al componente Título.

El uso de jQuery es opcional, así como las convenciones de nomenclatura utilizadas.

La siguiente es la definición de [!DNL LESS] para el estilo de ejemplo encontrado en:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## Prácticas recomendadas de desarrollo {#development-best-practices}

### Prácticas recomendadas de HTML {#html-best-practices}

* HTML (generado mediante HTL) debe ser lo más estructuralmente semántico posible; evitando la agrupación/anidación innecesaria de elementos.
* Los elementos de HTML deben poder direccionarse mediante clases CSS de estilo BEM.

**Bueno**: todos los elementos del componente se pueden dirigir mediante la notación BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Malo**: los elementos de lista y lista solo se pueden dirigir por nombre de elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Es mejor exponer más datos y ocultarlos que exponer demasiado pocos datos que requieran un futuro desarrollo back-end para exponerlos.

   * La implementación de alternancias de contenido con capacidad de creación puede ayudar a mantener esta HTML ágil, mediante la cual los autores pueden seleccionar qué elementos de contenido se escriben en HTML. puede ser especialmente importante a la hora de escribir imágenes en HTML que pueden no utilizarse en todos los estilos.
   * La excepción a esta regla se produce cuando los recursos caros, como las imágenes, se exponen de forma predeterminada, ya que las imágenes de evento ocultas por CSS se recuperan, en este caso, de forma innecesaria.

      * Los componentes de imagen modernos suelen utilizar JavaScript para seleccionar y cargar la imagen más adecuada para el caso de uso (ventanilla).

### Prácticas recomendadas de CSS {#css-best-practices}

>[!NOTE]
>
>El sistema de estilos presenta una pequeña divergencia técnica con [BEM](https://en.bem.info/), ya que `BLOCK` y `BLOCK--MODIFIER` no se aplican al mismo elemento, como se especifica en [BEM](https://en.bem.info/).
>
>En su lugar, debido a restricciones del producto, `BLOCK--MODIFIER` se aplica al elemento principal del elemento `BLOCK`.
>
>Todos los demás inquilinos de [BEM](https://en.bem.info/) deben estar alineados con.

* Use preprocesadores como [LESS](https://lesscss.org/) (compatible con AEM de forma nativa) o [SCSS](https://sass-lang.com/) (requiere un sistema de compilación personalizado) para permitir una definición CSS clara y la reutilización.

* Mantenga la uniformidad en cuanto a peso/especificidad del selector. Esto ayuda a evitar y resolver conflictos en cascada de CSS difíciles de identificar.
* Organice cada estilo en un archivo discreto.
   * Estos archivos se pueden combinar con LESS/SCSS `@imports` o si se requiere CSS sin procesar, mediante la inclusión de archivos de la biblioteca de cliente de HTML o sistemas personalizados de compilación de recursos front-end.
* Evite mezclar muchos estilos complejos.
   * Cuantos más estilos se puedan aplicar a un componente en un solo momento, mayor será la variedad de permutaciones. Esto puede resultar difícil de mantener, realizar controles de calidad o garantizar la alineación de la marca.
* Utilice siempre clases CSS (siguiendo la notación de BEM) para definir reglas CSS.
   * Si es absolutamente necesario seleccionar elementos sin clases CSS (es decir, elementos vacíos), muévalos más arriba en la definición CSS para dejar claro que tienen una especificidad menor que cualquier conflicto con elementos de ese tipo que no tienen clases CSS seleccionables.
* Evite aplicar estilo a `BLOCK--MODIFIER` directamente ya que está adjunto a la cuadrícula adaptable. Cambiar la visualización de este elemento puede afectar al procesamiento y la funcionalidad de la cuadrícula adaptable, por lo que solo aplicar estilo en este nivel cuando la intención es cambiar el comportamiento de la cuadrícula adaptable.
* Aplicar ámbito de estilo utilizando `BLOCK--MODIFIER`. `BLOCK__ELEMENT--MODIFIERS` se puede usar en el componente, pero como `BLOCK` representa el componente y el componente es el que tiene estilo, el estilo se &quot;define&quot; y se establece en el ámbito a través de `BLOCK--MODIFIER`.

Ejemplo de estructura del selector de CSS debe ser la siguiente:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Selector de primer nivel</p> <p>BLOQUE: MODIFICADOR</p> </td> 
   <td valign="bottom"><p>Selector de segundo nivel</p> <p>BLOQUEO</p> </td> 
   <td valign="bottom"><p>Selector de 3er nivel</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Selector CSS efectivo</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list-dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list-dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> color: azul;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image-hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image-hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> color: rojo;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

En el caso de los componentes anidados, la profundidad del selector CSS para estos elementos de componente anidados excederá el selector de tercer nivel. Repita el mismo patrón para el componente anidado, pero con ámbito del componente principal `BLOCK`. En otras palabras, inicie `BLOCK` del componente anidado en el tercer nivel y `ELEMENT` del componente anidado se encuentre en el cuarto nivel de selector.

### Prácticas recomendadas de JavaScript {#javascript-best-practices}

Las prácticas recomendadas definidas en esta sección pertenecen a &quot;style-JavaScript&quot; o a JavaScript, que está diseñado específicamente para manipular el componente con fines estilísticos en lugar de funcionales.

* Style-JavaScript debe utilizarse con prudencia y es un caso de uso minoritario.
* Style-JavaScript debe utilizarse principalmente para manipular el DOM del componente para admitir el estilo mediante CSS.
* Vuelva a evaluar el uso de Javascript si los componentes aparecerán muchas veces en una página y comprenda el coste de cálculo/extracción y de volver a dibujar.
* Vuelva a evaluar el uso de Javascript si extrae nuevos datos o contenido de forma asíncrona (mediante AJAX) cuando el componente puede aparecer muchas veces en una página.
* Gestionar las experiencias de publicación y creación.
* Vuelva a utilizar style-Javascript cuando sea posible.
   * Por ejemplo, si varios estilos de un componente requieren que su imagen se mueva a una imagen de fondo, el style-JavaScript se puede implementar una vez y adjuntarse a varios `BLOCK--MODIFIERs`.
* Separe style-JavaScript de JavaScript funcional cuando sea posible.
* Evalúe el coste de JavaScript frente a la manifestación de estos cambios de DOM en HTML directamente a través de HTL.
   * Cuando un componente que utiliza style-JavaScript requiera una modificación del lado del servidor, evalúe si la manipulación de JavaScript se puede realizar en este momento y cuáles son los efectos/ramificaciones para el rendimiento y la compatibilidad del componente.

#### Consideraciones de rendimiento {#performance-considerations}

* Style-JavaScript debe mantenerse ligero y delgado.
* Para evitar parpadeos y volver a dibujar innecesarios, oculte inicialmente el componente mediante `BLOCK--MODIFIER BLOCK` y muéstrelo cuando se hayan completado todas las manipulaciones de DOM en JavaScript.
* El rendimiento de las manipulaciones de estilo de JavaScript es similar a los complementos básicos de jQuery que se adjuntan y modifican elementos en DOMReady.
* Asegúrese de que las solicitudes estén comprimidas en gzip y de que CSS y JavaScript estén minificados.

## Recursos adicionales {#additional-resources}

* [Documentación del sistema de estilos](https://helpx.adobe.com/es/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creando bibliotecas de cliente de AEM](https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sitio web de documentación de BEM (Modificador de elementos de bloque)](https://getbem.com/)
* [MENOS sitio web de documentación](https://lesscss.org/)
* [sitio web jQuery](https://jquery.com/)
