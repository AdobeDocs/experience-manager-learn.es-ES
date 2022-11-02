---
title: Capacidad de autocompletar en AEM Forms
description: Permite a los usuarios buscar y seleccionar rápidamente de una lista previamente rellenada de valores a medida que escriben, aprovechando la búsqueda y el filtrado.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Implementación de autocompletar

Implemente la capacidad de autocompletar en AEM formularios utilizando la función de autocompletar de jquery.
El ejemplo incluido en este artículo utiliza una variedad de fuentes de datos (matriz estática, matriz dinámica rellenada desde una respuesta de API de REST) para rellenar las sugerencias a medida que el usuario comienza a escribir en el campo de texto.

El código utilizado para realizar la función de autocompletar está asociado con el suceso initialize del campo .


## Proporcionar sugerencias para el nombre del país

![sugerencias de país](assets/auto-complete1.png)

## Proporcionar una sugerencia para la dirección

![sugerencias de país](assets/auto-complete2.png)

El siguiente es el código utilizado para proporcionar sugerencias de direcciones de calle

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```

## Sugerencias con emoji

![sugerencias de país](assets/auto-complete3.png)

El siguiente código se utilizó para mostrar emoji en la lista de sugerencias

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

La variable [formulario de ejemplo se puede descargar](assets/auto-complete-form.zip) desde aquí. Asegúrese de proporcionar su propio nombre de usuario/clave de API mediante el editor de código para que el código realice llamadas de REST correctas.

>[!NOTE]
>
> Para que funcione la autocompleción, asegúrese de que el formulario utiliza la siguiente biblioteca de cliente **cq.jquery.ui**. Esta biblioteca de cliente incluye AEM.
