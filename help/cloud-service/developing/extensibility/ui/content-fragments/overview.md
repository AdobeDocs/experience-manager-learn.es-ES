---
title: AEM Extensiones de fragmentos de contenido
description: Obtenga información sobre cómo crear e implementar extensiones de fragmentos de contenido de AEM as a Cloud Service
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
jira: KT-11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
duration: 126
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# AEM Extensibilidad de fragmentos de contenido

AEM La interfaz de usuario de fragmentos de contenido es una potente interfaz de usuario ampliable para administrar la creación, administración y edición de fragmentos de contenido. Hay varios puntos de extensión disponibles para personalizar la interfaz de usuario según sus necesidades. Hay diferentes puntos de extensión disponibles en función de la interfaz de usuario que amplíe.

## Puntos de extensión de la consola Fragmentos de contenido

AEM La Consola de fragmento de contenido en (Adobe Experience Manager) es una interfaz de usuario que proporciona una ubicación centralizada para administrar y organizar fragmentos de contenido. Ofrece un completo conjunto de herramientas y funciones para crear, editar, publicar y rastrear fragmentos de contenido, lo que permite a los usuarios administrar de forma eficaz el contenido estructurado en varios canales y puntos de contacto.

![Consola Fragmentos de contenido](./assets/overview/cfc.png)

AEM [Consola de fragmentos de contenido de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=es) es la interfaz de usuario ampliable para enumerar y administrar fragmentos de contenido. AEM [Se han creado extensiones de la consola de fragmentos de contenido de](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) mediante la plantilla de App Builder `@adobe/aem-cf-admin-ui-ext-tpl`.

Los siguientes puntos de extensión de la consola Fragmentos de contenido están disponibles:

<div class="columns is-multiline">
      <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action bar">
        <div class="card" style="height: 100%">
          <div class="card-image">
            <figure class="image is-16by9">
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Barra de acciones" tabindex="-1" target="_blank" rel="referrer">
                <img class="is-bordered-r-small" src="./assets/overview/cfc-action-bar.png" alt="Barra de acciones">
              </a>
            </figure>
          </div>
          <div class="card-content is-padded-small">
            <div class="content">
              <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Barra de acciones" target="_blank" rel="referrer">Barra de acciones</a></p>
              <p class="is-size-6">Personalizar acciones para cuando se seleccionan uno o más fragmentos de contenido.</p>
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver los documentos</span>
              </a>
            </div>
          </div>
        </div>
      </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Grid columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Columnas de cuadrícula" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-grid-columns.png" alt="Columnas de cuadrícula">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Columnas de cuadrícula" target="_blank" rel="referrer">Columnas de cuadrícula</a></p>
          <p class="is-size-6">Personalice los datos que aparecen en la lista Fragmentos de contenido.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver los documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menú del encabezado" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-header-menu.png" alt="Menú del encabezado">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menú del encabezado" target="_blank" rel="referrer">Menú del encabezado</a></p>
          <p class="is-size-6">Personalice las acciones para cuando no haya fragmentos de contenido seleccionados.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver los documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## Puntos de extensión del Editor de fragmentos de contenido

AEM El Editor de fragmentos de contenido en (Adobe Experience Manager) es un componente de interfaz de usuario que permite a los usuarios crear, editar y administrar fragmentos de contenido. Proporciona un entorno visualmente intuitivo y fácil de usar para trabajar con contenido estructurado, lo que permite a los usuarios definir y organizar elementos de contenido, aplicar plantillas, administrar variaciones y previsualizar cómo aparece el contenido en diferentes canales. El editor de fragmentos de contenido optimiza el proceso de creación de contenido reutilizable y modular que se puede distribuir y publicar fácilmente en varias experiencias digitales.

![Editor de fragmentos de contenido](./assets/overview/cfe.png)

AEM El Editor de fragmentos de contenido es la interfaz de usuario ampliable para editar fragmentos de contenido. AEM [Se han creado extensiones del Editor de fragmentos de contenido de la](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) con la plantilla de App Builder `@adobe/aem-cf-editor-ui-ext-tpl`.

Los siguientes puntos de extensión del Editor de fragmentos de contenido están disponibles:

<div class="columns is-multiline">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
      <div class="card" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" title="Menú del encabezado" tabindex="-1" target="_blank" rel="referrer">
              <img class="is-bordered-r-small" src="./assets/overview/cfe-header-menu.png" alt="Menú del encabezado">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="Menú del encabezado" target="_blank" rel="referrer">Menú del encabezado</a></p>
            <p class="is-size-6">Personalice las acciones en el menú de encabezado del Editor de fragmentos de contenido.</p>
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver los documentos</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra de herramientas Editor de texto enriquecido" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="Barra de herramientas Editor de texto enriquecido">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra de herramientas Editor de texto enriquecido"  target="_blank" rel="referrer">Barra de herramientas Editor de texto enriquecido</a></p>
          <p class="is-size-6">Agregue un botón personalizado al Editor de texto enriquecido (RTE) del Editor de fragmentos de contenido.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver los documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor widgets">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widgets del editor de texto enriquecido" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="Widgets del editor de texto enriquecido">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widgets del editor de texto enriquecido" target="_blank" rel="referrer">Widgets del editor de texto enriquecido</a></p>
          <p class="is-size-6">Personalizar acciones en RTE enlazadas a pulsaciones de teclas.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver los documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor badges">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="Distintivos del Editor de texto enriquecido" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="Distintivos del Editor de texto enriquecido">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="Distintivos del Editor de texto enriquecido" target="_blank" rel="referrer">Distintivos del Editor de texto enriquecido</a></p>
          <p class="is-size-6">Personalice bloques de estilo no editables dentro de RTE.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver los documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## Ejemplos de extensiones

AEM Le damos la bienvenida a una colección de ejemplos de código de extensibilidad de la interfaz de usuario de. Este recurso está diseñado para proporcionarle demostraciones prácticas y perspectivas sobre la ampliación de la interfaz de usuario de Adobe Experience Manager AEM (). AEM Tanto si es un desarrollador que busca mejorar la funcionalidad de la aplicación, estos ejemplos de código sirven como una valiosa referencia.

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="Actualización de propiedades por lotes" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="Actualización de propiedades por lotes">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="Actualización de propiedades por lotes">Actualización de propiedades de fragmentos de contenido en lotes</a></p>
          <p class="is-size-6">Una extensión de la barra de acciones de la consola Fragmento de contenido con acción modal y Adobe I/O Runtime.</p>
          <a href="./examples/console-bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver el ejemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card" style="height: 100%">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./examples/console-image-generation-and-image-upload.md" title="AEM Generación y carga de imágenes basadas en OpenAI a la extensión de la" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="AEM Generación y carga de imágenes basadas en OpenAI a la extensión de la">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="AEM Generación y carga de imágenes basadas en OpenAI a la extensión de la">Generación de imágenes de OpenAPI</a></p>
                    <p class="is-size-6">AEM Explore un ejemplo de extensión de barra de acciones que genera una imagen mediante OpenAI, la carga en la propiedad de imagen y la actualiza en el fragmento de contenido seleccionado.</p>
                    <a href="./examples/console-image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver el ejemplo</span>
                    </a>
                </div>
            </div>
        </div>
    </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/custom-grid-columns.md" title="Columnas personalizadas" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/custom-grid-columns/card.png" alt="Columnas personalizadas">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/custom-grid-columns.md" title="Columnas personalizadas">Columnas personalizadas</a></p>
          <p class="is-size-6">Agregue una columna personalizada a la consola Fragmento de contenido.</p>
          <a href="./examples/custom-grid-columns.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver el ejemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Export to XML">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-export-to-xml.md" title="Exportar a XML" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="Exportar a XML">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="Exportar a XML">Exportar a XML</a></p>
          <p class="is-size-6">Exporte un fragmento de contenido como XML desde el Editor de fragmentos de contenido.</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver el ejemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar button">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-toolbar.md" title="Botón de barra de herramientas del Editor de texto enriquecido" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="Botón de barra de herramientas del Editor de texto enriquecido">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Botón de barra de herramientas del Editor de texto enriquecido">Botón de barra de herramientas del Editor de texto enriquecido</a></p>
          <p class="is-size-6">Agregue botones personalizados de la barra de herramientas a los campos RTE en el Editor de fragmentos de contenido.</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver el ejemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Widget">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-widget.md" title="Widget del editor de texto enriquecido" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-widget-card.png" alt="Widget del editor de texto enriquecido">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Widget del editor de texto enriquecido">Widget del editor de texto enriquecido</a></p>
          <p class="is-size-6">Agregue widgets al editor de texto enriquecido en el editor de fragmentos de contenido.</p>
          <a href="./examples/editor-rte-widget.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver el ejemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Badge">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-badges.md" title="Insignia del editor de texto enriquecido" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="Insignia del editor de texto enriquecido">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="Insignia del editor de texto enriquecido">Insignia del editor de texto enriquecido</a></p>
          <p class="is-size-6">Agregue insignias al Editor de texto enriquecido en el Editor de fragmentos de contenido.</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver el ejemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom fields">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-custom-field.md" title="Campos personalizados" tabindex="-1">
            <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3427585?format=jpeg" alt="Campos personalizados">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-custom-field.md" title="Campos personalizados">Campos personalizados</a></p>
          <p class="is-size-6">Cree campos personalizados de fragmento de contenido.</p>
          <a href="./examples/editor-custom-field.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver el ejemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div> 
</div>
