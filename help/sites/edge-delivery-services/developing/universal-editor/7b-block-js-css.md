---
title: Desarrollo de un bloque con CSS y JS
description: Desarrolle un bloque con CSS y JavaScript para Edge Delivery Services, editable con el editor universal.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 41c4cfcf-0813-46b7-bca0-7c13de31a20e
source-git-commit: ecd3ce33204fa6f3f2c27ebf36e20ec26e429981
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---

# Desarrollo de un bloque con CSS y JavaScript

En el [capítulo anterior](./7b-block-js-css.md), se trató el estilo de un bloque que utiliza solo CSS. Ahora, el enfoque cambia al desarrollo de un bloque con JavaScript y CSS.

Este ejemplo muestra cómo mejorar un bloque de tres formas:

1. Añadir clases CSS personalizadas.
1. Uso de detectores de eventos para añadir movimiento.
1. Administrar términos y condiciones que se pueden incluir opcionalmente en el texto del teaser.

## Casos de uso comunes

Este enfoque es especialmente útil en los siguientes casos:

- **Administración CSS externa:** Cuando el CSS del bloque se administra fuera de los Edge Delivery Services y no se alinea con su estructura de HTML.
- **Atributos adicionales:** Cuando se requieren atributos adicionales, como [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) para accesibilidad o [microdatos](https://developer.mozilla.org/en-US/docs/Web/HTML/Microdata).
- **Mejoras de JavaScript:** Cuando se necesitan características interactivas, como detectores de eventos.

Este método se basa en la manipulación DOM de JavaScript nativa del explorador, pero requiere precaución al modificar el DOM, especialmente al mover elementos. Estos cambios pueden interrumpir la experiencia de creación del editor universal. Lo ideal es que el [modelo de contenido](./5-new-block.md#block-model) del bloque esté diseñado cuidadosamente para minimizar la necesidad de realizar amplias modificaciones en el DOM.

## Bloquear HTML

Para abordar el desarrollo de bloques, comience por revisar el DOM expuesto por los Edge Delivery Services. La estructura se mejora con JavaScript y se diseña con CSS.

>[!BEGINTABS]

>[!TAB DOM para decorar]

A continuación se muestra el DOM del bloque de teaser, que es el destino para decorar con JavaScript y CSS.

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB Cómo encontrar el DOM]

Para encontrar el DOM que decorar, abra la página con el bloque sin decorar en su entorno de desarrollo local, seleccione el bloque e inspeccione el DOM.

![Inspect bloquea DOM](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]


## Bloquear JavaScript

Para agregar la funcionalidad JavaScript a un bloque, cree un archivo JavaScript en el directorio del bloque con el mismo nombre que el bloque, por ejemplo, `/blocks/teaser/teaser.js`.

El archivo JavaScript debe exportar una función predeterminada:

```javascript
export default function decorate(block) { ... }
```

La función predeterminada toma el elemento o árbol DOM que representa el bloque en el HTML de Edge Delivery Services y contiene el JavaScript personalizado que se ejecuta cuando se procesa el bloque.

Este ejemplo de JavaScript realiza tres acciones principales:

1. Agrega un detector de eventos al botón de CTA para ampliar la imagen al pasar el ratón por encima.
1. Añade clases CSS semánticas a los elementos del bloque, lo que resulta útil al integrar sistemas de diseño CSS existentes.
1. Agrega una clase CSS especial a los párrafos que comienzan por `Terms and conditions:`.

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="Nombre de archivo del ejemplo de código siguiente."}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Adds a zoom effect to image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's' DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
   * Entry point to block's JavaScript.
   * Must be exported as default and accept a block's DOM element.
   * This function is called by the project's style.js, and passed the block's element.
   *
   * @param {HTMLElement} block represents the block's' DOM element/tree
   */
export default function decorate(block) {
  /* This JavaScript makes minor adjustments to the block's DOM */

  // Dress the DOM elements with semantic CSS classes so it's obvious what they are.
  // If needed we could also add ARIA roles and attributes, or add/remove/move DOM elements.

  // Add a class to the first picture element to target it with CSS
  block.querySelector('picture').classList.add('image-wrapper');

  // Use previously applied classes to target new elements
  block.querySelector('.image-wrapper img').classList.add('image');

  // Mark the second/last div as the content area (white, bottom aligned box w/ text and cta)
  block.querySelector(':scope > div:last-child').classList.add('content');

  // Mark the first H1-H6 as a title
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();

    // If the paragraph starts with Terms and conditions: then style it as such
    if (innerHTML?.startsWith("Terms and conditions:")) {
      /* If a paragraph starts with '*', add a special CSS class. */
      p.classList.add('terms-and-conditions');
    }
  });

  // Add event listeners to the block
  addEventListeners(block);
}
```

## Bloquear CSS

Si creó un `teaser.css` en el [capítulo anterior](./7a-block-css.md), elimínelo o renómbrelo a `teaser.css.bak`, ya que este capítulo implementa CSS diferente para el bloque de teaser.

Crear un archivo de `teaser.css` en la carpeta del bloque. Este archivo contiene el código CSS que aplica estilo al bloque. Este código CSS se dirige a los elementos del bloque y a las clases CSS semánticas específicas agregadas por JavaScript en `teaser.js`.

Los elementos vacíos se pueden seguir diseñando directamente o con las clases CSS aplicadas personalizadas. Para bloques más complejos, la aplicación de clases CSS semánticas puede ayudar a que el CSS sea más comprensible y mantenible, especialmente cuando se trabaja con equipos más grandes durante períodos de tiempo más largos.

[Al igual que antes de](./7a-block-css.md#develop-a-block-with-css), asigne el ámbito de CSS a `.block.teaser` mediante el anidamiento de [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting) para evitar conflictos con otros bloques.

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="Nombre de archivo del ejemplo de código siguiente."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;

    /* The teaser image */
    & .image-wrapper {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;
        overflow: hidden; 

        & .image {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
            transform: scale(1); 
            transition: transform 0.6s ease-in-out;
        }
    }

    /* The teaser text content */
    & .content {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;
  
        & .title {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        & .title::after {
            border-bottom: 0;
        }

        & p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
            animation: teaser-fade-in .6s;
        }

        & p.terms-and-conditions {
            font-size: var(--body-font-size-xs);
            color: var(--secondary-color);
            padding: .5rem 1rem;
            font-style: italic;
            border: solid var(--light-color);
            border-width: 0 0 0 10px;
        }

        /* Add underlines to links in the text */
        & a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        & .button-container {
            margin: 0;
            padding: 0;
        }

        & .button {   
            background-color: var(--primary-color);
            border-radius: 0;
            color: var(--dark-color);
            font-size: var(--body-font-size-xs);
            font-weight: bold;
            padding: 1em 2.5em;
            margin: 0;
            text-transform: uppercase;
        }
    }

    & .zoom {
        transform: scale(1.1);
    }
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## Agregar términos y condiciones

La implementación anterior agrega compatibilidad con párrafos con un estilo especial que comienzan con el texto `Terms and conditions:`. Para validar esta funcionalidad, en el Editor universal, actualice el contenido de texto del bloque de teaser para incluir términos y condiciones.

Siga los pasos del [autor de un bloque](./6-author-block.md) y edite el texto para incluir un párrafo de **términos y condiciones** al final:

```
WKND Adventures

Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.

Terms and conditions: By signing up, you agree to the rules for participation and booking.
```

Compruebe que el párrafo se representa con el estilo de los términos y condiciones en el entorno de desarrollo local. Recuerde, estos cambios de código no se reflejan en el editor universal hasta que se [insertan en una rama de GitHub](#preview-in-universal-editor) para la que el editor universal se ha configurado.

## Previsualización de desarrollo

A medida que se añaden CSS y JavaScript AEM, el entorno de desarrollo local de la CLI de la vuelve a cargar los cambios, lo que permite una visualización rápida y sencilla del impacto del código en el bloque. Pase el ratón sobre CTA y compruebe que la imagen del teaser se amplía y reduce.

![Vista previa de desarrollo local del teaser mediante CSS y JS](./assets/7b-block-js-css/local-development-preview.png)

## Vincular el código

Asegúrate de [pelar con frecuencia](./3-local-development-environment.md#linting) los cambios de tu código para mantenerlo limpio y consistente. La identificación regular ayuda a detectar los problemas de forma temprana, lo que reduce el tiempo de desarrollo general. Recuerde, no puede combinar su trabajo de desarrollo en la rama `main` hasta que se resuelvan todos los problemas de vinculación.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## Vista previa en el editor universal

AEM Para ver los cambios en el Editor universal de Git de la aplicación, agregue, confirme e inserte los cambios en la rama del repositorio de Git utilizada por el Editor universal. Al hacerlo, se garantiza que la implementación de bloques no interrumpa la experiencia de creación.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for teaser block"
$ git push origin teaser
```

Ahora puede obtener una vista previa de los cambios en el Editor universal al agregar el parámetro de consulta `?ref=teaser`.

![Teaser en el editor universal](./assets/7b-block-js-css/universal-editor-preview.png)
