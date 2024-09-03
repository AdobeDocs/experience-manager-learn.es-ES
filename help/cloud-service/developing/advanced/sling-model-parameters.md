---
title: Parametrizar modelos Sling de HTL
description: AEM Obtenga información sobre cómo pasar parámetros de HTL a un modelo Sling en la.
version: Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
exl-id: 5d852617-720a-4a00-aecd-26d0ab77d9b3
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Parametrizar modelos Sling de HTL

Adobe Experience Manager AEM () ofrece un marco sólido para crear aplicaciones web dinámicas y adaptables. Una de sus potentes funciones es la capacidad de parametrizar los modelos Sling, lo que mejora su flexibilidad y reutilización. Este tutorial le guiará a través de la creación de un modelo Sling parametrizado y su uso en HTL (lenguaje de plantilla de HTML) para procesar contenido dinámico.

## Script HTL

En este ejemplo, el script HTL envía dos parámetros al modelo Sling `ParameterizedModel`. El modelo manipula estos parámetros en su método `getValue()` y devuelve el resultado para su visualización.

Este ejemplo pasa dos parámetros de cadena, pero puede pasar cualquier tipo de valor u objeto al modelo Sling, siempre y cuando el tipo de campo [Modelo Sling, anotado con `@RequestAttribute`](#sling-model-implementation), coincida con el tipo de objeto o valor pasado desde HTL.

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **Parametrización:** La instrucción `data-sly-use` crea una instancia de `ParameterizedModel` con `myParameterOne` y `myParameterTwo`.
- **Procesamiento condicional:** `data-sly-test` comprueba si el modelo está listo antes de mostrar el contenido.
- **Llamada de marcador de posición:** El `placeholderTemplate` administra casos en los que el modelo no está listo.

## Implementación del modelo Sling

Así se implementa el modelo Sling:

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **Anotación de modelo:** La anotación `@Model` designa esta clase como un modelo Sling, adaptable desde `SlingHttpServletRequest` e implementando la interfaz `ParameterizedModel`.
- **Atributos de solicitud:** La anotación `@RequestAttribute` inserta parámetros HTL en el modelo.
- **Métodos:** `getValue()` concatena los parámetros y `isReady()` comprueba que no estén en blanco.

La interfaz `ParameterizedModel` se define de la siguiente manera:

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## Salida de ejemplo

Con los parámetros `"Hello"` y `"World"`, el script HTL genera el siguiente resultado:

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

AEM Esto muestra cómo se puede influir en los modelos Sling parametrizados en los parámetros de entrada proporcionados a través de HTL.
