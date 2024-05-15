---
title: Capacidad de autocompletar en AEM Forms
description: Permite a los usuarios buscar y seleccionar rápidamente valores de una lista previamente rellenada a medida que escriben, aprovechando la búsqueda y el filtrado.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
duration: 47
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# Implementar Completar automáticamente

AEM Implemente la capacidad de completado automático en los formularios de mediante la función de completado automático de jquery.
El ejemplo incluido con este artículo utiliza una variedad de fuentes de datos (matriz estática, matriz dinámica rellenada a partir de una respuesta de API de REST) para rellenar las sugerencias a medida que el usuario comienza a escribir en el campo de texto.

El código utilizado para realizar la capacidad de autocompletar está asociado al evento initialize del campo.

## Sugerencia de dirección

![sugerencias de país](assets/auto-complete2.png)



El siguiente es el código utilizado para proporcionar sugerencias de direcciones de calles

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





## Sugerencias con emojis

![sugerencias de país](assets/auto-complete3.png)

El siguiente código se utilizó para mostrar los emoji en la lista de sugerencias

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

El [se puede descargar un formulario de ejemplo](assets/auto-complete-form.zip) desde aquí. Asegúrese de proporcionar su propio nombre de usuario/clave de API mediante el editor de código para que el código realice llamadas REST correctamente.

>[!NOTE]
>
> Para que funcione el completado automático, asegúrese de que el formulario utiliza la siguiente biblioteca de cliente **cq.jquery.ui**. AEM Esta biblioteca de cliente viene con el código de acceso de la aplicación
