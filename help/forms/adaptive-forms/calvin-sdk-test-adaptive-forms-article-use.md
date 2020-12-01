---
title: 'Uso de pruebas automatizadas con AEM Forms adaptable '
seo-title: 'Uso de pruebas automatizadas con AEM Forms adaptable '
description: Pruebas automatizadas de Forms adaptable con el SDK de Calvin
seo-description: Pruebas automatizadas de Forms adaptable con el SDK de Calvin
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%

---


# Uso de pruebas automatizadas con AEM Forms adaptable {#using-automated-tests-with-aem-adaptive-forms}

Pruebas automatizadas de Forms adaptable con el SDK de Calvin

Calvin SDK es una API de utilidad para desarrolladores de Forms adaptable para probar Forms adaptable. El SDK de Calvin está construido sobre el [marco de pruebas de Hobbes.js](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html). El SDK de Calvin está disponible con AEM Forms 6.3 y versiones posteriores.

En este tutorial, creará lo siguiente:

* Test Suite
* Test Suite contendrá uno o más casos de prueba
* Los casos de prueba contendrán una o varias acciones

## Introducción {#getting-started}

[Descargar e instalar los recursos mediante ](assets/testingadaptiveformsusingcalvinsdk1.zip)el Administrador de paquetesEl paquete contiene secuencias de comandos de ejemplo y varios Forms adaptable.Estos Forms adaptables se crean con la versión AEM Forms 6.3. Se recomienda crear nuevos formularios específicos de su versión de AEM Forms si está probando esto en AEM Forms 6.4 o posterior. Las secuencias de comandos de ejemplo muestran varias API de Calvin SDK disponibles para probar Forms adaptable. Los pasos generales para probar AEM Forms adaptable son:

* Vaya al formulario que debe probarse
* Definir el valor del campo
* Enviar el formulario adaptable
* Buscar mensajes de error

Los scripts de ejemplo del paquete muestran todas las acciones anteriores.
Analicemos el código de `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

El código anterior crea un nuevo grupo de pruebas.

* El nombre de TestSuite en este caso es &#39; `Mortgage Form Test` &#39;.
* Se proporciona la ruta absoluta en AEM al archivo js que contiene el grupo de pruebas.
* El parámetro register cuando se establece en &#39; `true` &#39;, hace que Test Suite esté disponible en la interfaz de usuario de la prueba.

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
>Si va a probar esta capacidad en AEM Forms 6.4 o superior, cree un nuevo formulario adaptable y úselo para realizar la prueba.No se recomienda utilizar el formulario adaptable proporcionado con el paquete.

Se pueden agregar casos de prueba al grupo de pruebas para que se ejecuten en un formulario adaptable.

* Para agregar un caso de prueba al grupo de pruebas, utilice el método `addTestCase` del objeto TestSuite.
* El método `addTestCase` toma un objeto TestCase como parámetro.
* Para crear TestCase, utilice el método `hobs.TestCase(..)`.
* Nota: El primer parámetro es el nombre del caso de prueba que aparecerá en la interfaz de usuario.
* Una vez creado un caso de prueba, puede agregar acciones al caso de prueba.
* Las acciones que incluyen `navigateTo`, `asserts.isTrue` se pueden agregar como acciones al caso de prueba.

## Ejecución de las pruebas automatizadas {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)OpenthetestsuiteExpanda el grupo de pruebas y ejecute las pruebas. Si todo se ejecuta correctamente, verá el siguiente resultado.

![calvinsdk](assets/calvinimage.png)

## Pruebe los grupos de pruebas de ejemplo {#try-out-the-sample-test-suites}

Como parte del paquete de muestra, hay tres grupos de pruebas adicionales. Puede probarlos incluyendo los archivos correspondientes en el archivo js.txt de la biblioteca de clientes, como se muestra a continuación:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
