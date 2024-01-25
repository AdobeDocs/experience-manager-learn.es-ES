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
last-substantial-update: 2019-06-09T00:00:00Z
duration: 43
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# Alternar visibilidad del campo de contraseña

Un caso de uso común es permitir que los rellenadores de formulario cambien a la visibilidad del texto introducido en el campo de contraseña.
Para llevar a cabo este caso de uso, he utilizado el icono de ojo del [Biblioteca impresionante de fuentes](https://fontawesome.com/). El CSS requerido y eye.svg se incluyen en la biblioteca de cliente creada para esta demostración.


## Código de muestra

El formulario adaptable tiene un campo de tipo PasswordBox llamado **ssnField**.

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

El siguiente CSS se utilizó para colocar la variable **ojo** dentro del campo contraseña

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

* Descargue la [biblioteca de cliente](assets/simple-ui-tips.zip)
* Descargue la [formulario de ejemplo](assets/simple-ui-tricks-form.zip)
* Importe la biblioteca de cliente mediante el [IU del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe el formulario de ejemplo con la variable [Forms y documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Previsualización del formulario](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


