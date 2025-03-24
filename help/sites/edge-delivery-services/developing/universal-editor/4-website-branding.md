---
title: Agregar marca del sitio web
description: Defina CSS global, variables CSS y fuentes web para un sitio de Edge Delivery Services.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# Agregar marca del sitio web

Para empezar, configure la marca general, actualice los estilos globales, defina las variables CSS y añada fuentes web. Estos elementos básicos garantizan que el sitio web sea coherente y se pueda mantener, y que se aplique de forma coherente en todo el sitio.

## Crear un problema de GitHub

Para mantener todo organizado, utilice GitHub para hacer un seguimiento del trabajo. En primer lugar, cree una incidencia de GitHub para este cuerpo de trabajo:

1. Vaya al repositorio de GitHub (consulte el capítulo [Crear un proyecto de código](./1-new-code-project.md) para obtener más información).
2. Haz clic en la ficha **Problemas** y luego en **Nuevo problema**.
3. Escribe un **título** y **descripción** para que se haga el trabajo.
4. Haga clic en **Enviar nuevo problema**.

El problema de GitHub se usa más adelante al [crear una solicitud de extracción](#merge-code-changes).

![GitHub crea un nuevo problema](./assets/4-website-branding/github-issues.png)

## Crear una rama de trabajo

Para mantener la organización y garantizar la calidad del código, cree una nueva rama para cada cuerpo de trabajo. Esta práctica evita que el nuevo código afecte negativamente al rendimiento y garantiza que los cambios no estén activos antes de completarse.

Para este capítulo, que se centra en los estilos básicos del sitio web, cree una rama denominada `wknd-styles`.

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## CSS global

Edge Delivery Services usa un archivo CSS global, ubicado en `styles/styles.css`, para configurar los estilos comunes de todo el sitio web. El `styles.css` controla aspectos como los colores, las fuentes y el espaciado, asegurándose de que todo se ve uniforme en el sitio.

CSS global debe ser agnóstico con construcciones de nivel inferior como bloques, centrándose en la apariencia general del sitio y tratamientos visuales compartidos.

Tenga en cuenta que los estilos CSS globales se pueden anular cuando sea necesario.

### Variables CSS

Las [variables CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) son una buena manera de almacenar configuraciones de diseño como colores, fuentes y tamaños. Mediante variables, puede cambiar estos elementos en un lugar y hacer que se actualicen en todo el sitio.

Para empezar a personalizar las variables CSS, siga estos pasos:

1. Abra el archivo `styles/styles.css` en el editor de código.
2. Busque la declaración `:root`, donde se almacenan las variables CSS globales.
3. Modifique las variables de color y fuente para que coincidan con la marca WKND.

A continuación se muestra un ejemplo:


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

Explore las demás variables de la sección `:root` y revise la configuración predeterminada.

A medida que desarrolla un sitio web y se encuentra repitiendo los mismos valores CSS, considere la posibilidad de crear nuevas variables para que los estilos sean más fáciles de administrar. Algunos ejemplos de otras propiedades CSS que pueden beneficiarse de las variables CSS son: `border-radius`, `padding`, `margin` y `box-shadow`.

### Elementos desnudos

Los elementos vacíos se diseñan directamente a través de su nombre de elemento en lugar de utilizar una clase CSS. Por ejemplo, en lugar de aplicar estilo a una clase CSS `.page-heading`, los estilos se aplican al elemento `h1` mediante `h1 { ... }`.

En el archivo `styles/styles.css`, se aplica un conjunto de estilos base a los elementos HTML vacíos. Los sitios web de Edge Delivery Services dan prioridad al uso de elementos vacíos porque se alinean con la semántica nativa de HTML del servicio de Edge Delivery.

Para alinearnos con la marca WKND, vamos a aplicar estilo a algunos elementos vacíos en `styles.css`:

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

Estos estilos garantizan que los elementos de `h2`, a menos que se anulen, tengan un estilo coherente con la marca WKND, lo que ayuda a crear una jerarquía visual clara. El subrayado amarillo parcial debajo de cada `h2` agrega un toque distintivo a los encabezados.

### Elementos deducidos

En Edge Delivery Services, el código `scripts.js` y `aem.js` del proyecto mejoran automáticamente elementos específicos de HTML desnudos en función de su contexto en HTML.

Por ejemplo, los elementos de anclaje (`<a>`) creados en su propia línea, en lugar de estar alineados con el texto adyacente, se infieren como botones basados en este contexto. Estos anclajes se encapsulan automáticamente con un contenedor `div` con clase CSS `button-container` y el elemento de anclaje tiene agregada una clase CSS `button`.

Por ejemplo, cuando un vínculo se crea en su propia línea, Edge Delivery Services JavaScript actualiza su DOM de la siguiente manera:

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

Estos botones se pueden personalizar para que coincidan con la marca WKND, que dicta que los botones aparezcan como rectángulos amarillos con texto negro.

A continuación se muestra un ejemplo de cómo aplicar estilo a los &quot;botones deducidos&quot; de `styles.css`:

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

Este CSS define los estilos de botón base e incluye tratamientos específicos de WKND, como texto en mayúsculas, fondo amarillo y texto negro. Las propiedades `background-color` y `color` utilizan variables CSS que permiten que el estilo del botón permanezca alineado con los colores de la marca. Este enfoque garantiza que los botones tengan un estilo coherente en todo el sitio y que, al mismo tiempo, sigan siendo flexibles.

## Fuentes web

Los proyectos de Edge Delivery Services optimizan el uso de fuentes web para mantener un alto rendimiento y minimizar el impacto en las puntuaciones de Lighthouse. Este método garantiza una representación rápida sin poner en peligro la identidad visual del sitio. A continuación se explica cómo implementar las fuentes web de forma eficaz para obtener un rendimiento óptimo.

### Caras de fuente

Agregar fuentes web personalizadas mediante declaraciones CSS `@font-face` en el archivo `styles/fonts.css`. Agregar `@font-faces` a `fonts.css` garantiza que las fuentes web se carguen en el momento óptimo y ayudan a mantener las puntuaciones de Lighthouse.

1. Abra `styles/fonts.css`.
2. Agregue las siguientes declaraciones `@font-face` para incluir las fuentes de marca WKND: `Asar` y `Source Sans Pro`.

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

Las fuentes utilizadas en este tutorial provienen de Google Fonts, pero pueden obtenerse de cualquier proveedor de fuentes, incluido [Adobe Fonts](https://fonts.adobe.com/).

+++Uso de archivos de fuentes web locales

Como alternativa, los archivos de fuentes web se copiarán en el proyecto de la carpeta `/fonts` y se hará referencia a ellos en las declaraciones `@font-face`.

Este tutorial utiliza las fuentes web remotas y alojadas para que sea más fácil seguirlas.

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

Por último, actualice las variables CSS `styles/styles.css` para que utilicen las nuevas fuentes:

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

`roboto-fallback` y `roboto-condensed-fallback` son fuentes de reserva que se actualizaron en la sección [Fuentes de reserva](#fallback-fonts) para alinearlas y admitir las fuentes web `Asar` y `Source Sans Pro` personalizadas.

### Fuentes de reserva

Las fuentes web suelen afectar al rendimiento debido a su tamaño, lo que aumenta potencialmente las puntuaciones del desplazamiento acumulativo de diseño (CLS) y reduce las puntuaciones generales de Lighthouse. Para garantizar la visualización instantánea del texto mientras se cargan las fuentes web, los proyectos de Edge Delivery Services utilizan fuentes de reserva nativas del explorador. Este método ayuda a mantener una experiencia de usuario suave mientras se aplica la fuente deseada.

Para seleccionar la mejor fuente de reserva, use la [extensión Helix Font Fallback Chrome](https://www.aem.live/developer/font-fallback) de Adobe, que determina la fuente más parecida que deben usar los exploradores antes de que se cargue la fuente personalizada. Las declaraciones de fuentes de reserva resultantes deben agregarse al archivo `styles/styles.css` para mejorar el rendimiento y garantizar una experiencia perfecta para los usuarios.

![Extensión de Chrome de reserva de fuente Helix](./assets/4-website-branding/font-fallback-chrome-plugin.png){align=center}

Para usar la extensión [Helix Font Fallback Chrome](https://www.aem.live/developer/font-fallback), asegúrate de que la página web tenga fuentes web aplicadas en las mismas variaciones usadas en el sitio web de Edge Delivery Services. Este tutorial muestra la extensión en [wknd.site](http://wknd.site/us/en.html). Al desarrollar un sitio web, aplique la extensión al sitio en el que se está trabajando en lugar de a [wknd.site](http://wknd.site/us/en.html).

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

Agregue los nombres de las familias de fuentes de reserva a las variables CSS de las fuentes en `styles/styles.css` después de los nombres de las familias de fuentes &quot;reales&quot;.

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## Previsualización de desarrollo

A medida que agrega CSS, el entorno de desarrollo local de la CLI de AEM vuelve a cargar automáticamente los cambios, lo que facilita y agiliza la visualización de cómo el CSS afecta al bloque.

![Vista previa de desarrollo de WKND brand CSS](./assets/4-website-branding/preview.png)


## Descargar archivos CSS finales

Puede descargar los archivos CSS actualizados desde los vínculos siguientes:

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## Vincular los archivos CSS

Asegúrate de [pelar con frecuencia](./3-local-development-environment.md#linting) los cambios de tu código para asegurarte de que estén limpios y sean consistentes. La vinculación regular ayuda a detectar los problemas de forma temprana y reduce el tiempo de desarrollo general. Recuerde que no puede combinar su trabajo con la rama principal hasta que se resuelvan todos los problemas de vinculación.

```bash
$ npm run lint:css
```

## Combinar cambios de código

Combine los cambios en la rama `main` de GitHub para generar el trabajo futuro con estas actualizaciones.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

Una vez insertados los cambios en la rama `wknd-styles`, cree una solicitud de extracción en GitHub para fusionarlos en la rama `main`.

1. Vaya al repositorio de GitHub desde el capítulo [Crear un nuevo proyecto](./1-new-code-project.md).
2. Haga clic en la ficha **Solicitudes de extracción** y seleccione **Nueva solicitud de extracción**.
3. Establezca `wknd-styles` como la rama de origen y `main` como la rama de destino.
4. Revise los cambios y haga clic en **Crear solicitud de extracción**.
5. En los detalles de la solicitud de extracción, **agregue lo siguiente**:

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1` hace referencia al problema de GitHub creado anteriormente.
   * Las direcciones URL de prueba indican a AEM Code Sync qué ramas utilizar para la validación y comparación. La dirección URL &quot;Después&quot; utiliza la rama de trabajo `wknd-styles` para comprobar cómo afectan los cambios de código al rendimiento del sitio web.

6. Haga clic en **Crear solicitud de extracción**.
7. Espere a que la [aplicación AEM Code Sync de GitHub](./1-new-code-project.md) **complete las comprobaciones de calidad**. Si fallan, resuelva los errores y vuelva a ejecutar las comprobaciones.
8. Una vez superadas las comprobaciones, **combine la solicitud de extracción** en `main`.

Con los cambios combinados en `main`, no se consideran implementados en producción y el nuevo desarrollo puede continuar en función de estas actualizaciones.
