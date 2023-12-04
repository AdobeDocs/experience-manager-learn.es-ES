---
title: AEM Uso de pruebas automatizadas con Forms adaptable de
description: Pruebas automatizadas de Forms adaptable con el SDK de Calvin
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 142
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# AEM Uso de pruebas automatizadas con Forms adaptable de {#using-automated-tests-with-aem-adaptive-forms}

Pruebas automatizadas de Forms adaptable con el SDK de Calvin

El SDK de Calvin es una API de utilidad para que los desarrolladores de Forms adaptables prueben Forms adaptable. Calvin SDK se basa en el [Marco de pruebas de Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=es). El SDK de Calvin está disponible a partir de AEM Forms 6.3.

En este tutorial, creará lo siguiente:

* Grupo de pruebas
* El grupo de pruebas contendrá uno o más casos de prueba
* Los casos de prueba contendrán una o más acciones

## Introducción {#getting-started}

[Descargar e instalar los recursos mediante el administrador de paquetes](assets/testingadaptiveformsusingcalvinsdk1.zip)El paquete contiene secuencias de comandos de ejemplo y varios Forms adaptables. Estos Forms adaptables se crean con la versión 6.3 de AEM Forms. Se recomienda crear formularios nuevos específicos de la versión de AEM Forms si está probando esto en AEM Forms 6.4 o superior. Los scripts de ejemplo muestran varias API del SDK de Calvin disponibles para probar el Forms adaptable. AEM Los pasos generales para probar el Forms adaptable de la aplicación son:

* Desplácese hasta el formulario que necesite probar
* Establecer el valor del campo
* Enviar el formulario adaptable
* Buscar mensajes de error

Los scripts de ejemplo del paquete muestran todas las acciones anteriores.
Vamos a explorar el código de `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

El código anterior crea un nuevo grupo de pruebas.

* El nombre de TestSuite en este caso es &#39; `Mortgage Form Test` &#39;.
* AEM Se proporciona la ruta absoluta en la que se encuentra el archivo js, que contiene el grupo de pruebas, en el que se indica la ruta de acceso absoluta.
* El parámetro register cuando se establece en &#39; `true` &quot;, hace que el grupo de pruebas esté disponible en la interfaz de usuario de prueba.

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
>Si está probando esta capacidad en AEM Forms 6.4 o superior, cree un nuevo formulario adaptable y utilícelo para realizar las pruebas. No se recomienda utilizar el formulario adaptable que se proporciona con el paquete.

Se pueden agregar casos de prueba al grupo de pruebas para ejecutarlos en un formulario adaptable.

* Para añadir un caso de prueba al grupo de pruebas, utilice el `addTestCase` método del objeto TestSuite.
* El `addTestCase` toma un objeto TestCase como parámetro.
* Para crear TestCase, utilice el `hobs.TestCase(..)` método.
* Nota: El primer parámetro es el nombre del caso de prueba que aparecerá en la interfaz de usuario.
* Una vez que haya creado un caso de prueba, puede agregar acciones a dicho caso.
* Acciones que incluyen `navigateTo`, `asserts.isTrue` se puede añadir como acciones al caso de prueba.

## Ejecución de pruebas automatizadas {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)Expanda el grupo de pruebas y ejecute las pruebas. Si todo se ejecuta correctamente, verá el siguiente resultado.

![calvinsdk](assets/calvinimage.png)

## Pruebe los grupos de pruebas de ejemplo {#try-out-the-sample-test-suites}

Como parte del paquete de muestra, hay tres grupos de pruebas adicionales. Puede probarlos incluyendo los archivos adecuados en el archivo js.txt de la biblioteca de cliente como se muestra a continuación:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
