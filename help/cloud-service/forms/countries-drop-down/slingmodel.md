---
title: Crear un modelo Sling para el componente
description: Crear un modelo Sling para el componente
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: f4a18f02-61a2-4fa3-bfbb-41bf696cd2a8
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Crear un modelo Sling para el componente

Un modelo Sling en AEM es un marco basado en Java que se utiliza para simplificar el desarrollo de la lógica back-end para los componentes. Permite a los desarrolladores asignar datos de recursos de AEM (nodos JCR) a objetos Java mediante anotaciones, lo que proporciona una forma limpia y eficaz de gestionar datos dinámicos para componentes.
Esta clase, PaísesDropDownImpl, es una implementación de la interfaz PaísesDropDown en un proyecto de AEM (Adobe Experience Manager). Activa un componente desplegable en el que los usuarios pueden seleccionar un país en función de su continente seleccionado. Los datos desplegables se cargan dinámicamente desde un archivo JSON almacenado en AEM DAM (Digital Asset Manager).

**Campos de la clase**

* **multiSelect**: indica si la lista desplegable permite selecciones múltiples.
Se inserta desde las propiedades del componente utilizando @ValueMapValue con un valor predeterminado de false.
* **solicitud**: representa la solicitud HTTP actual. Útil para acceder a información específica del contexto.
* **continente**: Almacena el continente seleccionado para el menú desplegable (por ejemplo, &quot;asia&quot;, &quot;europa&quot;).
Se inserta desde el cuadro de diálogo de propiedades del componente, con un valor predeterminado de &quot;asia&quot; si no se proporciona ninguno.
* **resourceResolver**:Se usa para acceder y manipular recursos en el repositorio de AEM.
* **jsonData**: un objeto JSONbject que almacena los datos analizados del archivo JSON correspondiente al continente seleccionado.

**Métodos de la clase**

* **getContinent()** Un método sencillo para devolver el valor del campo continente.
Registra el valor que se devuelve con fines de depuración.
* **init()** Método de ciclo de vida anotado con @PostConstruct, ejecutado después de construir la clase y de insertar las dependencias. Construye dinámicamente la ruta de acceso al archivo JSON en función del valor de continente.
Obtiene el archivo JSON del DAM de AEM mediante resourceResolver.
Adapta el archivo a un recurso, lee su contenido y lo analiza en un objeto JSON.
Registra cualquier error o advertencia durante este proceso.
* **getEnums()**: recupera todas las claves (códigos de país) de los datos JSON analizados.
Ordena las claves alfabéticamente y las devuelve en forma de matriz.
Registra el número de códigos de país que se devuelven.
* **getEnumNames()** extrae todos los nombres de países de los datos JSON analizados.
Ordena los nombres alfabéticamente y los devuelve en forma de matriz.
Registra el número total de países y cada nombre de país recuperado.
* **isMultiSelect()** Devuelve el valor del campo multiSelect para indicar si la lista desplegable permite selecciones múltiples.



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## Siguientes pasos

[Generar, implementar y probar](./build.md)
