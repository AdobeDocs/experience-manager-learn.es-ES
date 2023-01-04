---
title: Extensiones de la consola de fragmentos de contenido de AEM
description: Obtenga información sobre cómo crear e implementar AEM extensiones de la consola de fragmentos de contenido as a Cloud Service
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: 8b683fdcea05859151b929389f7673075c359141
workflow-type: tm+mt
source-wordcount: '774'
ht-degree: 4%

---


# Extensión de la consola Fragmentos de contenido de AEM

[Consola AEM fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=es) las extensiones se pueden agregar mediante dos puntos de extensión: un botón de la sección [La consola Fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=es) menú de encabezado o barra de acciones. Las extensiones se escriben en JavaScript que se ejecutan como aplicaciones de App Builder y pueden implementar una interfaz de usuario web personalizada y acciones de Adobe I/O Runtime sin servidor para realizar un trabajo más intensivo y de larga duración.

![Extensión de la consola Fragmentos de contenido de AEM](./assets/overview/example.png){align="center"}

| Tipo de extensión | Descripción | Parámetros |
| :--- | :--- | :--- |
| Menú Encabezado | Agrega un botón al encabezado que se muestra cuando __zero__ Los fragmentos de contenido están seleccionados. | Ninguno. |
| Barra de acciones | Agrega un botón a la barra de acciones que se muestra cuando __uno o más__ Los fragmentos de contenido están seleccionados. | Matriz de las rutas de fragmentos de contenido seleccionadas. |

Una sola extensión de la consola de fragmentos de contenido AEM puede incluir cero o un menú de encabezado y cero o un tipo de extensión de barra de acciones. Si se requieren varios tipos de extensión del mismo tipo, se deben crear varias extensiones de la consola Fragmentos de contenido AEM.

AEM las extensiones de la consola Fragmentos de contenido requieren un [Proyecto de la consola de Adobe Developer](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) y [aplicación de App Builder](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) usando la variable `@adobe/aem-cf-admin-ui-ext-tpl` plantilla, asociada al proyecto de la consola de Adobe Developer.

Seleccione entre las siguientes funciones al generar la aplicación de App Builder, en función de lo que haga la extensión. Cualquier combinación de opciones se puede utilizar en una extensión.

|  | Botón Agregar a [Menú Encabezado](./header-menu.md) | Botón Agregar a [Barra de acciones](./action-bar.md) | Show [Modal](./modal.md) | Agregar [controlador del lado del servidor](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Disponible cuando los fragmentos de contenido no están seleccionados | š |  |  |  |
| Disponible cuando se seleccionan uno o más fragmentos de contenido |  | ✔ |  |  |
| Recopila datos personalizados del usuario |  |  | ✔️ |  |
| Muestra comentarios personalizados al usuario |  |  | ✔️ |  |
| Invoca solicitudes HTTP a AEM |  |  |  | ✔ |
| Invoca solicitudes HTTP a servicios de Adobe o de terceros |  |  |  | ✔ |


## Documentación de Adobe Developer

Adobe Developer contiene detalles del desarrollador sobre AEM extensiones de la consola de fragmentos de contenido. Revise el [Contenido de Adobe Developer para obtener más información técnica](https://developer.adobe.com/uix/docs/).

## Desarrollo de una extensión

Siga los pasos descritos a continuación para aprender a generar, desarrollar e implementar una extensión de la consola de fragmentos de contenido AEM para AEM as a Cloud Service.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console" title="Crear proyecto de Adobe Developer" tabindex="-1" target="_adobe-developer-com">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Crear proyecto de Adobe Developer">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Crear un proyecto</p>
                    <p class="is-size-6">Cree un proyecto de Adobe Developer Console que defina su acceso a otros servicios de Adobe y administre sus implementaciones.</p>
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_adobe-developer-com">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Creación de un proyecto de Adobe Developer</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation/#launch-code-generation-during-project-initialization" title="Generar una aplicación de extensión" tabindex="-1" target="_adobe-developer-com">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Inicializar una aplicación de extensión">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Inicializar una aplicación de extensión</p>
                    <p class="is-size-6">Inicialice una aplicación del Generador de aplicaciones de la extensión de la consola de fragmentos de contenido de AEM que defina dónde aparece la extensión y el trabajo que realiza.</p>
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation/#launch-code-generation-during-project-initialization" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_adobe-developer-com">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Inicializar una aplicación de extensión</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="Registro de extensiones" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="Registro de extensiones">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. Registro de extensión</p>
                    <p class="is-size-6">Registre la extensión en la consola de fragmentos de contenido de AEM como un menú de encabezado o un tipo de extensión de barra de acciones.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Registrar la extensión</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="Menú Encabezado" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menú Encabezado">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Menú Encabezado</p>
                    <p class="is-size-6">Obtenga información sobre cómo crear una extensión de menú de encabezado de la Consola de fragmento de contenido AEM.</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ampliación del menú del encabezado</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="Barra de acciones" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="Barra de acciones">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. Barra de acciones</p>
                    <p class="is-size-6">Obtenga información sobre cómo crear una extensión de barra de acciones de la Consola de fragmentos de contenido AEM.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ampliación de la barra de acciones</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="Modal" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="Modal">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5. Modal</p>
                    <p class="is-size-6">Agregue un modal personalizado a la extensión que se puede usar para crear experiencias personalizadas para los usuarios. Los modelos suelen recopilar entradas de los usuarios y mostrar los resultados de una operación.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Añadir un modal</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Acción de Adobe I/O Runtime" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Acción de Adobe I/O Runtime">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Acción de Adobe I/O Runtime</p>
                    <p class="is-size-6">Agregue una acción Adobe I/O Runtime sin servidor que la extensión pueda invocar para interactuar con fragmentos de contenido y AEM para realizar operaciones comerciales personalizadas.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Agregar una acción de Adobe I/O Runtime</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="Probar" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="Probar">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7. Prueba</p>
                    <p class="is-size-6">Pruebe las extensiones durante el desarrollo y comparta las extensiones completadas en los probadores de control de calidad o de aceptación del usuario mediante una dirección URL especial.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Probar la extensión</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="Implementación de extensiones" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="Implementación de extensiones">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. Implementación de producción</p>
                    <p class="is-size-6">Implemente la extensión en Adobe I/O para que esté disponible para AEM usuarios. Las extensiones también se pueden actualizar y eliminar.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Implementar en producción</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Extensiones de ejemplo

Ejemplo AEM extensiones de la consola Fragmento de contenido.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Extensión de actualización de propiedades masivas" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Extensión de actualización de propiedades masivas">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Extensión de actualización de propiedades masivas</p>
                    <p class="is-size-6">Explore una extensión de barra de acciones de ejemplo que actualice de forma masiva una propiedad en fragmentos de contenido seleccionados.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Explorar la extensión de ejemplo</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="Generación y carga de imágenes en AEM extensión" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="Generación y carga de imágenes en AEM extensión">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Generación y carga de imágenes en AEM extensión</p>
                    <p class="is-size-6">Explore una extensión de barra de acciones de ejemplo que genere una imagen mediante OpenAI, la cargue en AEM y actualice la propiedad de imagen en el fragmento de contenido seleccionado.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Explorar la extensión de ejemplo</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
