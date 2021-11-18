---
title: Algunos consejos y trucos útiles en la interfaz de usuario
description: Documento para mostrar algunas sugerencias útiles de la interfaz de usuario
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 4%

---

# Alternar visibilidad del campo de contraseña

Un caso de uso común es permitir que los usuarios que rellenen el formulario puedan acceder al texto introducido en el campo de contraseña.
Para lograr este caso de uso, he utilizado el icono de ojo del [Biblioteca Awesome de fuentes](https://fontawesome.com/). El CSS requerido y el eye.svg se incluyen en la biblioteca de cliente creada para esta demostración.



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

Se ha utilizado el CSS siguiente para colocar el **ojo** dentro del campo de contraseña

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

## Implementar el ejemplo de contraseña de alternador

* Descargue el [biblioteca cliente](assets/simple-ui-tips.zip)
* Descargue el [formulario de ejemplo](assets/simple-ui-tricks-form.zip)
* Importe la biblioteca del cliente utilizando la variable [interfaz de usuario del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe el formulario de ejemplo utilizando la variable [Forms y documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


