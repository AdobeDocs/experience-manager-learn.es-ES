---
title: Informar sobre los campos de datos de formulario enviados mediante Adobe Analytics
description: Integración de AEM Forms CS con Adobe Analytics para informar sobre campos de datos de formulario
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# Crear elementos de datos adecuados

En la propiedad Tags hemos agregado dos nuevos elementos de datos (ApplicationsStateOfResidence y validationError).

![formulario adaptable](assets/data_elements.png)

## ApplicationStateOfResidence

La variable **ApplicationStateOfResidence** el elemento de datos se configuró seleccionando **Principal** en la lista desplegable de extensiones y **Código personalizado** para el tipo de elemento de datos como se muestra en la captura de pantalla siguiente
![solicitante-estado-residencia](assets/applicantstateofresidence.png)

El siguiente código personalizado se utilizó para capturar el valor de la variable **_state_** campo de formulario adaptable.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

La variable **Error de validación** el elemento de datos se configuró seleccionando **Principal** en la lista desplegable de extensiones y **Código personalizado** para el tipo de elemento de datos como se muestra en la captura de pantalla siguiente

![validation-error](assets/validation-error.png)

Se escribió el siguiente código personalizado para establecer el valor del elemento de datos validationError.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");
_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)
if(tel.isValid == false)
{  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if(email.isValid == false)
{  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```
