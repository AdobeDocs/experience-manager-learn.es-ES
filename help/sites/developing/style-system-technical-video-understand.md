---
title: Cómo codificar para el sistema de estilos AEM
description: En este vídeo veremos la anatomía de CSS (o LESS) y JavaScript que se utilizan para aplicar estilo al componente de título principal de Adobe Experience Manager mediante el sistema de estilos, así como la forma en que estos estilos se aplican al HTML y al DOM.
feature: style-system
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 664d3964df796d508973067f8fa4fe5ef83c5fec
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 1%

---


# Cómo codificar el sistema de estilos{#understanding-how-to-code-for-the-aem-style-system}

En este vídeo veremos la anatomía de CSS (o [!DNL LESS]) y JavaScript utilizados para aplicar estilo al componente de título principal de Experience Manager mediante el sistema de estilos, así como la forma en que estos estilos se aplican al HTML y al DOM.

>[!NOTE]
>
>El sistema de estilo de AEM se introdujo con [AEM 6.3 SP1](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp1-release-notes.html) + [Feature Pack 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593).
>
>El vídeo supone que el componente Título We.Retail se ha actualizado para heredar de Componentes [principales v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases).

## Cómo codificar el sistema de estilos {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

El paquete de AEM proporcionado (**Technical-review.sites.style-system-1.0.0.zip**) instala el estilo de título de ejemplo, las directivas de muestra para los componentes de título y Contenedor de diseño We.Retail y una página de muestra.

[Technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

La siguiente es la [!DNL LESS] definición del estilo de ejemplo que se encuentra en:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Para los que prefieren CSS, debajo de este fragmento de código se encuentra la CSS en la que se compila [!DNL LESS] .

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

Lo anterior [!DNL LESS] se compila de forma nativa mediante Experience Manager a la siguiente CSS.

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

El siguiente código JavaScript recopila e inyecta la fecha y hora de la última modificación de la página actual debajo del texto del título cuando se aplica el estilo Ejemplo al componente Título.

El uso de jQuery es opcional, así como las convenciones de nombres utilizadas.

La siguiente es la [!DNL LESS] definición del estilo de ejemplo que se encuentra en:

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

## Prácticas recomendadas para el desarrollo {#development-best-practices}

### Prácticas recomendadas de HTML {#html-best-practices}

* HTML (generado a través de HTL) debe ser lo más estructuralmente semántico posible; evitar la agrupación/anidación innecesaria de elementos.
* Los elementos HTML deben ser abordados mediante clases CSS de estilo BEM.

**Bueno** : todos los elementos del componente se pueden dirigir mediante notación BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Malo** : los elementos de lista y lista solo se pueden dirigir por nombre de elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Es mejor exponer más datos y ocultarlos que exponer datos demasiado pequeños que requieran un desarrollo posterior futuro para poder revelarlos.

   * La implementación de toques de contenido con capacidad de creación puede ayudar a mantener esta impresión HTML, por la que los autores pueden seleccionar qué elementos de contenido se escriben en el HTML. El puede ser especialmente importante al escribir imágenes en el HTML que no se pueden utilizar en todos los estilos.
   * La excepción a esta regla es cuando los recursos caros, por ejemplo las imágenes, se exponen de forma predeterminada, ya que las imágenes de evento ocultas por CSS se recuperarán innecesariamente en este caso.

      * Los componentes de imagen modernos suelen utilizar JavaScript para seleccionar y cargar la imagen más adecuada para el caso de uso (ventanilla).

### Prácticas recomendadas de CSS {#css-best-practices}

>[!NOTE]
>
>El sistema de estilos crea una pequeña divergencia técnica con respecto a [BEM](https://en.bem.info/), en el sentido de que `BLOCK` y no `BLOCK--MODIFIER` se aplican al mismo elemento, tal como lo especifica [BEM](https://en.bem.info/).
>
>En su lugar, debido a restricciones de producto, el `BLOCK--MODIFIER` se aplica al elemento principal del `BLOCK` elemento.
>
>Todos los demás inquilinos de [BEM](https://en.bem.info/) deben estar alineados.

* Utilice preprocesadores como [LESS](https://lesscss.org/) (compatible con AEM de forma nativa) o [SCSS](https://sass-lang.com/) (requiere un sistema de compilación personalizado) para permitir una definición CSS clara y la reutilización.

* Mantener uniforme el peso/especificidad del selector; Esto ayuda a evitar y resolver conflictos en cascada de CSS difíciles de identificar.
* Organice cada estilo en un archivo independiente.
   * Estos archivos se pueden combinar con LESS/SCSS `@imports` o si se requiere CSS sin procesar, mediante la inclusión de archivos de la biblioteca de clientes HTML o sistemas personalizados de generación de recursos front-end.
* Evite mezclar muchos estilos complejos.
   * Cuantos más estilos se puedan aplicar a un componente a la vez, buena será la variedad de permutaciones. Esto puede resultar difícil de mantener/QA/asegurar la alineación de la marca.
* Utilice siempre clases CSS (siguiendo la notación BEM) para definir reglas CSS.
   * Si la selección de elementos sin clases CSS (es decir, elementos vacíos) es absolutamente necesaria, muévalos más arriba en la definición CSS para dejar claro que tienen una especificidad menor que cualquier conflicto con elementos de ese tipo que no tengan clases CSS seleccionables.
* Evite aplicar estilo a la cuadrícula `BLOCK--MODIFIER` directamente, ya que se adjunta a la cuadrícula adaptable. Cambiar la visualización de este elemento puede afectar al procesamiento y la funcionalidad de la cuadrícula adaptable, por lo que solo el estilo de este nivel cuando la intención es cambiar el comportamiento de la cuadrícula adaptable.
* Aplique el ámbito de estilo mediante `BLOCK--MODIFIER`. El estilo `BLOCK__ELEMENT--MODIFIERS` se puede usar en el componente, pero como `BLOCK` representa el componente y el componente es el estilo, el estilo se &quot;define&quot; y se establece en el ámbito mediante `BLOCK--MODIFIER`.

La estructura del selector CSS de ejemplo debe ser la siguiente:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Selector de primer nivel</p> <p>BLOQUE: MODIFICADOR</p> </td> 
   <td valign="bottom"><p>Selector de segundo nivel</p> <p>BLOQUEO</p> </td> 
   <td valign="bottom"><p>Selector de tercer nivel</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Selector de CSS efectivo</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-lista: oscuro</span></td> 
   <td valign="middle"><span class="code">.cmp-lista</span></td> 
   <td valign="middle"><span class="code">.cmp-lista_elemento</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-lista: oscuro</span></p> <p><span class="code"> .cmp-lista</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-lista__item { </span></strong></p> <p><strong> color: azul;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> color: rojo;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

En el caso de los componentes anidados, la profundidad del selector de CSS para estos elementos de componente anidados superará el selector de tercer nivel. Repita el mismo patrón para el componente anidado, pero con el ámbito del componente principal `BLOCK`. O, en otras palabras, inicio el componente anidado `BLOCK` en el tercer nivel y el componente anidado `ELEMENT` estará en el cuarto nivel de selector.

### Prácticas recomendadas de JavaScript {#javascript-best-practices}

Las optimizaciones definidas en esta sección se refieren a &quot;style-JavaScript&quot; o JavaScript diseñado específicamente para manipular el componente con fines estilísticos en lugar de funcionales.

* Style-JavaScript debe utilizarse con prudencia y es un caso de uso minoritario.
* Style-JavaScript debe utilizarse principalmente para manipular el DOM del componente para admitir el estilo mediante CSS.
* Vuelva a evaluar el uso de Javascript si los componentes aparecerán muchas veces en una página y comprenda el costo de cómputo y redibujo.
* Vuelva a evaluar el uso de Javascript si extrae nuevos datos/contenido de forma asíncrona (mediante AJAX) cuando el componente pueda aparecer muchas veces en una página.
* Gestionar las experiencias de publicación y creación.
* Vuelva a utilizar style-Javascript cuando sea posible.
   * Por ejemplo, si varios estilos de un componente requieren que su imagen se mueva a una imagen de fondo, el estilo de JavaScript se puede implementar una vez y adjuntar a varios `BLOCK--MODIFIERs`.
* Separe el estilo JavaScript de JavaScript funcional cuando sea posible.
* Evalúe el coste de JavaScript en comparación con la posibilidad de manifestar estos cambios DOM en el HTML directamente a través de HTL.
   * Cuando un componente que utiliza estilo-JavaScript requiere una modificación en el servidor, evalúe si la manipulación de JavaScript puede introducirse en este momento y cuáles son los efectos/ramificaciones en el rendimiento y la compatibilidad del componente.

#### Consideraciones de rendimiento {#performance-considerations}

* Style-JavaScript debe mantenerse ligero y delgado.
* Para evitar parpadeos y redibujamientos innecesarios, oculte inicialmente el componente mediante `BLOCK--MODIFIER BLOCK`y muéstrelo cuando se hayan completado todas las manipulaciones DOM en JavaScript.
* El rendimiento de las manipulaciones de estilo-JavaScript es similar a los complementos básicos de jQuery que se adjuntan y modifican elementos en DOMReady.
* Asegúrese de que las solicitudes estén comprimidas y de que CSS y JavaScript estén minimizados.

## Recursos adicionales {#additional-resources}

* [Documentación del sistema de estilos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creación de bibliotecas de AEM cliente](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sitio web de documentación de BEM (Modificador de elementos de bloque)](https://getbem.com/)
* [Sitio web de documentación de LESS](https://lesscss.org/)
* [Sitio Web de jQuery](https://jquery.com/)
