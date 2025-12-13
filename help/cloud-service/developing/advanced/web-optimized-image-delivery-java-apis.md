---
title: Entrega de imágenes optimizadas para la web Java& trade; API
description: Aprenda a utilizar las API de Java&trade; de entrega de imágenes optimizadas para la web de AEM as a Cloud Service para desarrollar experiencias web de alto rendimiento.
version: Experience Manager as a Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
exl-id: c6bb9d6d-aef0-42d5-a189-f904bbbd7694
duration: 352
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 1%

---

# API de Java™ para la entrega de imágenes optimizadas para la web

Aprenda a utilizar las API de Java™ de entrega de imágenes optimizadas para la web de AEM as a Cloud Service para desarrollar experiencias web de alto rendimiento.

AEM as a Cloud Service admite [entrega de imágenes optimizadas para la web](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=es), lo cual genera automáticamente representaciones web de imágenes optimizadas de los recursos. La entrega de imágenes optimizadas para la web se puede utilizar con tres enfoques principales:

1. [Usar componentes de AEM Core WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
2. Crear un componente personalizado que [extienda el componente de imagen de AEM Core WCM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=es#tackling-the-image-problem)
3. Cree un componente personalizado que utilice la API Java™ de AssetDelivery para generar direcciones URL de imagen optimizadas para la web.

Este artículo explora el uso de las API de Java™ de imagen optimizada para la web en un componente personalizado, de una manera que permite que las API basadas en código funcionen tanto en AEM as a Cloud Service como en AEM SDK.

## API de Java™

La [API AssetDelivery](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) es un servicio OSGi que genera direcciones URL de entrega optimizadas para la web para los recursos de imagen. `AssetDelivery.getDeliveryURL(...)` opciones permitidas están [documentadas aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=es#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

El servicio OSGi `AssetDelivery` solo se cumple cuando se ejecuta en AEM as a Cloud Service. En AEM SDK, las referencias al servicio OSGi `AssetDelivery` devuelven `null`. Es mejor utilizar de forma condicional la URL optimizada para la web cuando se ejecuta en AEM as a Cloud Service y utilizar una URL de imagen de reserva en el SDK de AEM. Normalmente, la representación web del recurso es una reserva suficiente.


### Uso de API en el servicio OSGi

Marque la referencia `AssetDelivery` como opcional en los servicios OSGi personalizados para que el servicio OSGi personalizado siga estando disponible en AEM SDK.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### Uso de API en el modelo Sling

Marque la referencia `AssetDelivery` como opcional en los modelos Sling personalizados, de modo que el modelo Sling personalizado permanezca disponible en AEM SDK.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### Uso condicional de API

Devolver condicionalmente la URL de imagen optimizada para la web o la URL de reserva en función de la disponibilidad del servicio OSGi `AssetDelivery`. El uso condicional permite que el código funcione al ejecutarlo en AEM SDK.

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## Código de ejemplo

El siguiente código crea un componente de ejemplo que muestra una lista de recursos de imagen mediante direcciones URL de imagen optimizadas para la web.

Cuando el código se ejecuta en AEM as a Cloud Service, las representaciones de imágenes web optimizadas para la web se utilizan en el componente personalizado.

![Imágenes optimizadas para la web en AEM as a Cloud Service](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEM as a Cloud Service admite la API AssetDelivery, por lo que se utiliza la representación web optimizada_

Cuando el código se ejecuta en AEM SDK, se utilizan las representaciones web estáticas menos óptimas, lo que permite que el componente funcione durante el desarrollo local.

![Imágenes de reserva optimizadas para la web en AEM SDK](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_AEM SDK no admite la API AssetDelivery, por lo que se utiliza la representación web estática de reserva (PNG o JPEG)_

La implementación se divide en tres partes lógicas:

1. El servicio OSGi `WebOptimizedImage` actúa como un &quot;proxy inteligente&quot; para el servicio OSGi `AssetDelivery` proporcionado por AEM que puede controlar la ejecución tanto en AEM as a Cloud Service como en AEM SDK.
2. El modelo Sling `ExampleWebOptimizedImages` proporciona lógica empresarial para recopilar la lista de recursos de imagen y sus direcciones URL optimizadas para la web que se van a mostrar.
3. El componente AEM `example-web-optimized-images` implementa HTL para mostrar la lista de imágenes optimizadas para la web.

El código de ejemplo siguiente se puede copiar en la base de código y actualizar según sea necesario.

### Servicio OSGi

El servicio OSGi `WebOptimizedImage` está dividido en una interfaz pública a la que se puede dirigir (`WebOptimizedImage`) y una implementación interna (`WebOptimizedImageImpl`). `WebOptimizedImageImpl` devuelve una URL de imagen optimizada para la web al ejecutarse en AEM as a Cloud Service y una URL de representación web estática en AEM SDK, lo que permite que el componente siga funcionando en AEM SDK.

#### Interfaz

La interfaz define el contrato de servicio OSGi con el que pueden interactuar otros códigos, como los modelos Sling.

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### Implementación

La implementación del servicio OSGi incluye una referencia opcional al servicio OSGi `AssetDelivery` de AEM y una lógica de reserva para seleccionar una URL de imagen adecuada cuando `AssetDelivery` es `null` en AEM SDK. La lógica de reserva se puede actualizar según los requisitos.

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### Modelo Sling

El modelo Sling de `ExampleWebOptimizedImages` se divide en una interfaz pública a la que se puede dirigir (`ExampleWebOptimizedImages`) y una implementación interna (`ExampleWebOptimizedImagesImpl`);

El modelo Sling `ExampleWebOptimizedImagesImpl` recopila la lista de recursos de imagen que se van a mostrar e invoca el servicio OSGi personalizado `WebOptimizedImage` para obtener la dirección URL de la imagen optimizada para la web. Dado que este modelo Sling representa un componente AEM, tiene los métodos habituales como `isEmpty()`, `getId()` y `getData()`; sin embargo, estos métodos no son directamente relevantes para utilizar imágenes optimizadas para la web.

#### Interfaz

La interfaz define el contrato del modelo Sling con el que puede interactuar otro código, como HTL.

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### Implementación

El modelo Sling utiliza el servicio OSGi personalizado `WebOptimizeImage` para recopilar las direcciones URL de imagen optimizadas para la web de los recursos de imagen que muestra su componente.

En este ejemplo, se utiliza una consulta simple para recopilar recursos de imagen.

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### Componente de AEM

Un componente de AEM está enlazado al tipo de recurso Sling de la implementación del modelo Sling `WebOptimizedImagesImpl` y es responsable de mostrar la lista de imágenes.



El componente recibe una lista de `Img` objetos a través de `getImages()` que incluyen las imágenes WEBP optimizadas para la web al ejecutarse en AEM as a Cloud Service El componente recibe una lista de `Img` objetos a través de `getImages()` que incluyen imágenes web PNG/JPEG estáticas al ejecutarse en AEM SDK.

#### HTL

HTL usa el modelo Sling `WebOptimizedImages` y procesa la lista de objetos `Img` devueltos por `getImages()`.

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```
