---
title: Algunos consejos y trucos útiles para la interfaz de usuario
description: Documento para mostrar algunos consejos útiles sobre la interfaz de usuario
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
exl-id: 13b9cd28-2797-4da9-a300-218e208cd21b
last-substantial-update: 2019-07-07T00:00:00Z
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# Alternar visibilidad del campo de contraseña

Un caso de uso común es permitir que los rellenadores de formulario cambien a la visibilidad del texto introducido en el campo de contraseña.
Para aplicar este ejemplo de uso, he usado el icono de ojo de la [Biblioteca de fuentes impresionantes](https://fontawesome.com/). El CSS requerido y eye.svg se incluyen en la biblioteca de cliente creada para esta demostración.



## Código de muestra

El formulario adaptable tiene un campo de tipo PasswordBox denominado **ssnField**.

El siguiente código se ejecuta cuando se carga el formulario

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

El siguiente CSS se utilizó para colocar el icono **eye** dentro del campo de contraseña

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## Implementar el ejemplo de contraseña de alternancia

* Descargar la [biblioteca de cliente](assets/simple-ui-tips.zip)
* Descargar [formulario de ejemplo](assets/simple-ui-tricks-form.zip)
* Importe la biblioteca de cliente mediante la [IU del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar el formulario de ejemplo con [Forms y el documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)
