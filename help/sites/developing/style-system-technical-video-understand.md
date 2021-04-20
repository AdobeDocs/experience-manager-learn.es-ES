---
title: Cómo codificar para el sistema de estilos de AEM
description: En este vídeo, echaremos un vistazo a la anatomía de CSS (o LESS) y JavaScript que se usan para aplicar estilo al componente de título principal de Adobe Experience Manager mediante el sistema de estilos, así como a cómo se aplican estos estilos al HTML y al DOM.
feature: Style System
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 2%

---


# Cómo se codifica el sistema de estilos{#understanding-how-to-code-for-the-aem-style-system}

En este vídeo, echaremos un vistazo a la anatomía de CSS (o [!DNL LESS]) y JavaScript que se usan para aplicar estilo al componente de título principal de Experience Manager mediante el sistema de estilos, así como a cómo se aplican estos estilos al HTML y al DOM.

>[!NOTE]
>
>El sistema de estilos de AEM se introdujo con [AEM 6.3 SP1](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp1-release-notes.html) + [Feature Pack 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593).
>
>El vídeo supone que el componente Título We.Retail se ha actualizado para heredar de [Componentes principales v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases).

## Cómo se codifica el sistema de estilos {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

El paquete AEM proporcionado (**technical-review.sites.style-system-1.0.0.zip**) instala el estilo de título de ejemplo, las políticas de muestra para los componentes Contenedor y Título de diseño de We.Retail y una página de muestra.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

La siguiente es la definición [!DNL LESS] para el estilo de ejemplo que se encuentra en:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Para los que prefieren CSS, bajo este fragmento de código está el CSS en el que [!DNL LESS] se compila.

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

### JavaScript {#example-javascript}

El siguiente JavaScript recopila e inyecta la fecha y hora de la última modificación de la página actual debajo del texto del título cuando se aplica el estilo Ejemplo al componente Título .

El uso de jQuery es opcional, así como las convenciones de nomenclatura utilizadas.

La siguiente es la definición [!DNL LESS] para el estilo de ejemplo que se encuentra en:

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

### Prácticas recomendadas HTML {#html-best-practices}

* HTML (generado mediante HTL) debe ser lo más estructuralmente semántico posible; evitar la agrupación/anidación innecesaria de elementos.
* Los elementos HTML deben abordarse a través de clases CSS de estilo BEM.

**Bueno** : todos los elementos del componente se pueden dirigir mediante notación BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Incorrecto** : los elementos de lista y lista solo se pueden dirigir por nombre de elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Es mejor exponer más datos y ocultarlos que exponer demasiado pocos datos que requieran un futuro desarrollo del back-end para exponerlos.

   * La implementación de toggles de contenido que se pueden crear puede ayudar a mantener esta imagen HTML, por lo que los autores pueden seleccionar qué elementos de contenido se escriben en el HTML. El puede ser especialmente importante al escribir imágenes en HTML que no se puedan usar en todos los estilos.
   * La excepción a esta regla es cuando se exponen de forma predeterminada recursos costosos, por ejemplo imágenes, ya que las imágenes de evento ocultas por CSS se recuperarán innecesariamente en este caso.

      * Los componentes de imagen modernos suelen utilizar JavaScript para seleccionar y cargar la imagen más adecuada para el caso de uso (ventanilla).

### Prácticas recomendadas de CSS {#css-best-practices}

>[!NOTE]
>
>El sistema de estilos crea una pequeña divergencia técnica con respecto a [BEM](https://en.bem.info/), en el sentido de que `BLOCK` y `BLOCK--MODIFIER` no se aplican al mismo elemento, como se especifica en [BEM](https://en.bem.info/).
>
>En su lugar, debido a restricciones de producto, el `BLOCK--MODIFIER` se aplica al elemento principal del `BLOCK` elemento.
>
>Todos los demás inquilinos de [BEM](https://en.bem.info/) deben alinearse con.

* Utilice preprocesadores como [LESS](https://lesscss.org/) (compatible con AEM de forma nativa) o [SCSS](https://sass-lang.com/) (requiere sistema de compilación personalizado) para permitir una definición CSS clara y la reutilización.

* Mantener uniforme el peso/especificidad del selector; Esto ayuda a evitar y resolver conflictos en cascada de CSS difíciles de identificar.
* Organice cada estilo en un archivo discreto.
   * Estos archivos se pueden combinar utilizando LESS/SCSS `@imports` o si se requiere CSS sin procesar, mediante la inclusión de archivos de la biblioteca de cliente HTML o sistemas personalizados de creación de recursos front-end.
* Evite mezclar muchos estilos complejos.
   * Cuantos más estilos se puedan aplicar a un componente a la vez, mayor será la variedad de permutaciones. Esto puede resultar difícil de mantener/realizar controles de calidad/garantizar la alineación de la marca.
* Utilice siempre clases CSS (que sigan la notación BEM) para definir reglas CSS.
   * Si la selección de elementos sin clases CSS (es decir, elementos vacíos) es absolutamente necesaria, muévalos más arriba en la definición CSS para dejar claro que tienen una especificidad menor que cualquier conflicto con elementos de ese tipo que tienen clases CSS seleccionables.
* Evite aplicar estilo al `BLOCK--MODIFIER` directamente, ya que está adjunto a la cuadrícula interactiva. Cambiar la visualización de este elemento puede afectar a la renderización y funcionalidad de la cuadrícula interactiva, por lo que solo el estilo de este nivel cuando la intención es cambiar el comportamiento de la cuadrícula interactiva.
* Aplique un ámbito de estilo utilizando `BLOCK--MODIFIER`. El `BLOCK__ELEMENT--MODIFIERS` se puede usar en el componente, pero como el `BLOCK` representa el componente y el componente es el estilo, el estilo se &quot;define&quot; y se examina mediante `BLOCK--MODIFIER`.

La estructura del selector de CSS de ejemplo debe ser la siguiente:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Selector de primer nivel</p> <p>BLOQUE: MODIFICADOR</p> </td> 
   <td valign="bottom"><p>Selector de segundo nivel</p> <p>BLOQUE</p> </td> 
   <td valign="bottom"><p>Selector de tercer nivel</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Selector CSS efectivo</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list: oscuro</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list: oscuro</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item {  </span></strong></p> <p><strong> color: azul;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—héroe</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—héroe</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> color: rojo;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

En el caso de los componentes anidados, la profundidad del selector de CSS para estos elementos de componente anidados superará el selector de tercer nivel. Repita el mismo patrón para el componente anidado, pero con el alcance `BLOCK` del componente principal. O, en otras palabras, inicie el `BLOCK` del componente anidado en el tercer nivel y el `ELEMENT` del componente anidado estará en el cuarto nivel de selector.

### Prácticas recomendadas de JavaScript {#javascript-best-practices}

Las prácticas recomendadas definidas en esta sección están relacionadas con &quot;style-JavaScript&quot; o JavaScript, que están especialmente pensados para manipular el componente con fines estilísticos y no funcionales.

* Style-JavaScript debe utilizarse con cautela y es un caso de uso minoritario.
* Style-JavaScript debe usarse principalmente para manipular el DOM del componente para admitir el estilo mediante CSS.
* Vuelva a evaluar el uso de Javascript si los componentes aparecerán muchas veces en una página, y comprenda el coste de cálculo y redibujo.
* Vuelva a evaluar el uso de Javascript si obtiene nuevos datos/contenido asincrónicamente (mediante AJAX) cuando el componente puede aparecer muchas veces en una página.
* Gestione las experiencias de publicación y creación.
* Vuelva a utilizar style-Javascript cuando sea posible.
   * Por ejemplo, si varios estilos de un componente requieren que su imagen se mueva a una imagen de fondo, el estilo de JavaScript se puede implementar una vez y adjuntar a varios `BLOCK--MODIFIERs`.
* Separe el JavaScript de estilo del JavaScript funcional siempre que sea posible.
* Evalúe el coste de JavaScript y manifieste estos cambios de DOM en el HTML directamente a través de HTL.
   * Cuando un componente que utiliza estilo-JavaScript requiera una modificación del lado del servidor, evalúe si se puede incorporar la manipulación de JavaScript en este momento y cuáles son los efectos/ramificaciones para el rendimiento y la compatibilidad del componente.

#### Consideraciones de rendimiento {#performance-considerations}

* Style-JavaScript debe mantenerse ligero y delgado.
* Para evitar parpadeos y redibujamientos innecesarios, oculte inicialmente el componente mediante `BLOCK--MODIFIER BLOCK` y muéstrelo cuando se hayan completado todas las manipulaciones DOM en JavaScript.
* El rendimiento de las manipulaciones de estilo de JavaScript es similar a los complementos básicos de jQuery que se adjuntan y modifican elementos en DOMReady.
* Asegúrese de que las solicitudes estén comprimidas y de que CSS y JavaScript se minifiquen.

## Recursos adicionales {#additional-resources}

* [Documentación del sistema de estilos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creación de bibliotecas de cliente AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sitio web de documentación de BEM (Modificador de elemento de bloque)](https://getbem.com/)
* [Sitio web de documentación de LESS](https://lesscss.org/)
* [Sitio web de jQuery](https://jquery.com/)
