---
title: AEM Extensiones de la consola Fragmentos de contenido
description: AEM Obtenga información sobre cómo crear e implementar extensiones de consola de fragmentos de contenido as a Cloud Service
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
exl-id: 093c8d83-2402-4feb-8a56-267243d229dd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 4%

---

# AEM Extensión de la consola Fragmentos de contenido

[AEM Consola Fragmentos de contenido de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=es) extensiones se pueden añadir mediante dos puntos de extensión: un botón en la variable [Consola de fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=es) menú de encabezado o barra de acciones. Las extensiones están escritas en JavaScript que se ejecutan como aplicaciones de App Builder y pueden implementar una interfaz de usuario web personalizada y acciones de Adobe I/O Runtime sin servidor para realizar un trabajo más intensivo y de larga duración.

![AEM Extensión de la consola Fragmentos de contenido](./assets/overview/example.png){align="center"}

| Tipo de extensión | Descripción | Parámetro(s) |
| :--- | :--- | :--- |
| Menú del encabezado | Agrega un botón al encabezado que se muestra cuando __zero__ Los fragmentos de contenido están seleccionados. | Ninguna. |
| Barra de acciones | Agrega un botón a la barra de acciones que se muestra cuando __una o más__ Los fragmentos de contenido están seleccionados. | Una matriz de rutas de los fragmentos de contenido seleccionados. |

AEM Una sola extensión de la consola Fragmentos de contenido de un solo elemento puede incluir cero o un menú Encabezado, y cero o un tipo de extensión de barra de acciones. AEM Si se requieren varios tipos de extensión del mismo tipo, se deben crear varias extensiones de la consola Fragmentos de contenido de la.

AEM Extensiones de la consola Fragmentos de contenido, requieren un [Proyecto de la consola Adobe Developer](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) y un [Aplicación App Builder](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) uso del `@adobe/aem-cf-admin-ui-ext-tpl` plantilla, asociada al proyecto de la consola de Adobe Developer.

Seleccione entre las siguientes funcionalidades al generar la aplicación App Builder, en función de lo que haga la extensión. Se puede utilizar cualquier combinación de opciones en una extensión.

|  | Añadir botón a [Menú del encabezado](./header-menu.md) | Añadir botón a [Barra de acciones](./action-bar.md) | Mostrar [Modal](./modal.md) | Añadir [controlador del lado del servidor](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Disponible cuando los fragmentos de contenido no están seleccionados | ✔ |  |  |  |
| Disponible cuando se seleccionan uno o más fragmentos de contenido |  | ✔ |  |  |
| Recopila entradas personalizadas del usuario |  |  | ✔️ |  |
| Muestra comentarios personalizados al usuario |  |  | ✔️ |  |
| AEM Invoca solicitudes HTTP a los recursos de la |  |  |  | ✔ |
| Invoca solicitudes HTTP a servicios de Adobe/terceros |  |  |  | ✔ |


## Documentación de Adobe Developer

Adobe Developer AEM contiene detalles del desarrollador sobre las extensiones de la consola de fragmentos de contenido de la. Consulte la [Contenido de Adobe Developer para obtener más información técnica](https://developer.adobe.com/uix/docs/).

## Desarrollo de una extensión

AEM AEM Siga los pasos descritos a continuación para aprender a generar, desarrollar e implementar una extensión de la Consola de fragmento de contenido de para su uso en as a Cloud Service.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="Crear proyecto de Adobe Developer" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Crear proyecto de Adobe Developer">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Crear un proyecto</p>
                    <p class="is-size-6">Cree un proyecto de la consola de Adobe Developer que defina su acceso a otros servicios de y administre sus implementaciones.</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
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
                    <a href="./app-initialization.md" title="Generación de una aplicación de extensión" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Inicialización de una aplicación de extensión">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Inicializar una aplicación de extensión</p>
                    <p class="is-size-6">AEM Inicialice una aplicación de App Builder de la extensión de la consola Fragmento de contenido de la aplicación que defina dónde aparece la extensión y el trabajo que realiza.</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Inicialización de una aplicación de extensión</span>
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
                    <p class="headline is-size-5 has-text-weight-bold">3. Registro de la extensión</p>
                    <p class="is-size-6">AEM Registre la extensión en la consola Fragmento de contenido de la como un menú de encabezado o un tipo de extensión de barra de acciones.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Registro de la extensión</span>
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
                    <a href="./header-menu.md" title="Menú del encabezado" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menú del encabezado">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Menú de encabezado</p>
                    <p class="is-size-6">AEM Obtenga información sobre cómo crear una extensión de menú de encabezado de la consola Fragmento de contenido de.</p>
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
                    <p class="is-size-6">AEM Obtenga información sobre cómo crear una extensión de barra de acciones de la consola Fragmento de contenido de.</p>
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
                    <p class="is-size-6">Añada un modal personalizado a la extensión de que se puede utilizar para crear experiencias personalizadas para los usuarios. Los modelos a menudo recopilan información de los usuarios y muestran los resultados de una operación.</p>
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
                    <p class="is-size-6">Añada una acción de Adobe I/O Runtime AEM sin servidor que la extensión pueda invocar para interactuar con fragmentos de contenido y para realizar operaciones comerciales personalizadas.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Añadir una acción de Adobe I/O Runtime</span>
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
                    <p class="is-size-6">Pruebe las extensiones durante el desarrollo y comparta las extensiones completadas con probadores de control de calidad o UAT mediante una dirección URL especial.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Prueba de la extensión</span>
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
                    <p class="is-size-6">Implemente la extensión en el Adobe I/O AEM para que esté disponible para los usuarios de la. Las extensiones también se pueden actualizar y eliminar.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Implementación en producción</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Extensiones de ejemplo

AEM Ejemplo de extensiones de la consola Fragmentos de contenido.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Extensión de actualización de propiedades masiva" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Extensión de actualización de propiedades masiva">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Extensión de actualización de propiedades masiva</p>
                    <p class="is-size-6">Explore una extensión de barra de acciones de ejemplo que actualiza de forma masiva una propiedad en los fragmentos de contenido seleccionados.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exploración de la extensión de ejemplo</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Image Generartion update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="AEM Generación y carga de imágenes basadas en OpenAI a la extensión de la" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="AEM Generación y carga de imágenes basadas en OpenAI a la extensión de la">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">AEM Generación y carga de imágenes basadas en OpenAI a la extensión de la</p>
                    <p class="is-size-6">AEM Explore un ejemplo de extensión de barra de acciones que genera una imagen mediante OpenAI, la carga en la propiedad de imagen y la actualiza en el fragmento de contenido seleccionado.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exploración de la extensión de ejemplo</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
