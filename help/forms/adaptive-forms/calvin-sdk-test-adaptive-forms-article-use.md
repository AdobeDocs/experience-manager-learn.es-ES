---
title: 'Uso de pruebas automatizadas con AEM Forms adaptable '
description: Pruebas automatizadas de Forms adaptable mediante el SDK de Calvin
feature: Formularios adaptables
doc-type: article
activity: develop
version: 6.3,6.4,6.5
topic: Desarrollo
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 1%

---


# Uso de pruebas automatizadas con AEM Forms adaptable {#using-automated-tests-with-aem-adaptive-forms}

Pruebas automatizadas de Forms adaptable mediante el SDK de Calvin

El SDK de Calvin es una API de utilidad para que los desarrolladores de Forms adaptables prueben el Forms adaptable. El SDK de Calvin se basa en el [marco de pruebas Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html). El SDK de Calvin está disponible a partir de AEM Forms 6.3.

En este tutorial, creará lo siguiente:

* Grupo de pruebas
* El grupo de pruebas contendrá uno o más casos de prueba
* Los casos de prueba contendrán una o más acciones

## Introducción {#getting-started}

[Descargue e instale los recursos mediante el ](assets/testingadaptiveformsusingcalvinsdk1.zip)Administrador de paquetesEl paquete contiene secuencias de comandos de ejemplo y varios Forms adaptables.Estos Forms adaptables se crean con la versión 6.3 de AEM Forms. Se recomienda crear nuevos formularios específicos de su versión de AEM Forms si está probando esto en AEM Forms 6.4 o superior. Los scripts de ejemplo muestran varias API de SDK de Calvin disponibles para probar Forms adaptable. Los pasos generales para probar AEM Adaptive Forms son:

* Vaya al formulario que debe probarse
* Definir el valor del campo
* Enviar el formulario adaptable
* Buscar mensajes de error

Los scripts de ejemplo del paquete muestran todas las acciones anteriores.
Exploremos el código de `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

El código anterior crea un nuevo grupo de pruebas.

* El nombre de TestSuite en este caso es &#39; `Mortgage Form Test` &#39;.
* Se proporciona la ruta absoluta en AEM al archivo js que contiene el grupo de pruebas.
* El parámetro register cuando se establece en &#39; `true` &#39; &#39;, hace que Test Suite esté disponible en la interfaz de usuario de prueba.

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>Si está probando esta capacidad en AEM Forms 6.4 o superior, cree un nuevo formulario adaptable y utilícelo para realizar las pruebas.No se recomienda utilizar el formulario adaptable proporcionado con el paquete.

Se pueden agregar casos de prueba al grupo de pruebas para ejecutarlos en un formulario adaptable.

* Para agregar un caso de prueba al grupo de pruebas, utilice el método `addTestCase` del objeto TestSuite.
* El método `addTestCase` toma un objeto TestCase como parámetro.
* Para crear TestCase, utilice el método `hobs.TestCase(..)`.
* Nota: El primer parámetro es el nombre del caso de prueba que aparecerá en la interfaz de usuario.
* Una vez creado un caso de prueba, puede agregar acciones al caso de prueba.
* Se pueden agregar acciones, incluidas `navigateTo` y `asserts.isTrue`, como acciones al caso de prueba.

## Ejecución de las pruebas automatizadas {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)Abrir el conjunto de pruebasExpanda el grupo de pruebas y ejecute las pruebas. Si todo se ejecuta correctamente, verá el siguiente resultado.

![calvinsdk](assets/calvinimage.png)

## Pruebe los grupos de pruebas de ejemplo {#try-out-the-sample-test-suites}

Como parte del paquete de muestra, hay tres grupos de pruebas adicionales. Puede probarlos incluyendo los archivos adecuados en el archivo js.txt de la biblioteca de clientes como se muestra a continuación:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
