---
title: Uso de pruebas automatizadas con AEM Adaptive Forms
description: Pruebas automatizadas de Forms adaptable con Calvin SDK
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 101
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# Uso de pruebas automatizadas con AEM Adaptive Forms {#using-automated-tests-with-aem-adaptive-forms}

Pruebas automatizadas de Forms adaptable con Calvin SDK

Calvin SDK es una API de utilidad para que los desarrolladores de Forms adaptables prueben Forms adaptable. Calvin SDK se basa en el [marco de pruebas Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=es). Calvin SDK está disponible con AEM Forms 6.3 en adelante.

En este tutorial, creará lo siguiente:

* Grupo de pruebas
* El grupo de pruebas contendrá uno o más casos de prueba
* Los casos de prueba contendrán una o más acciones

## Introducción {#getting-started}

[Descargue e instale Assets mediante el administrador de paquetes](assets/testingadaptiveformsusingcalvinsdk1.zip)El paquete contiene secuencias de comandos de ejemplo y varios Forms adaptables. Estos Forms adaptables se han creado con la versión 6.3 de AEM Forms. Se recomienda crear formularios nuevos específicos de la versión de AEM Forms si está probando esto en AEM Forms 6.4 o superior. Los scripts de ejemplo muestran varias API de SDK de Calvin disponibles para probar el Forms adaptable. Los pasos generales para probar AEM Adaptive Forms son:

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
* Se proporciona la ruta absoluta en AEM al archivo js que contiene el grupo de pruebas.
* El parámetro register cuando se establece en &#39; `true` &#39;, hace que el grupo de pruebas esté disponible en la interfaz de usuario de prueba.

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

* Para agregar un caso de prueba al grupo de pruebas, utilice el método `addTestCase` del objeto TestSuite.
* El método `addTestCase` toma un objeto TestCase como parámetro.
* Para crear TestCase, utilice el método `hobs.TestCase(..)`.
* Nota: El primer parámetro es el nombre del caso de prueba que aparecerá en la interfaz de usuario.
* Una vez que haya creado un caso de prueba, puede agregar acciones a dicho caso.
* Las acciones, incluidas `navigateTo`, `asserts.isTrue`, se pueden agregar como acciones al caso de prueba.

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
