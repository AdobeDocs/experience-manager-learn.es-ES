---
title: Informar sobre los campos de datos de formulario enviados mediante Adobe Analytics
description: Integrar AEM Forms CS con Adobe Analytics para informar sobre campos de datos de formulario
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# Crear los elementos de datos adecuados

En la propiedad Tags se han agregado dos nuevos elementos de datos (ApplicantsStateOfResidence y validationError).

![formulario adaptable](assets/data_elements.png)

## ApplicantStateOfResidence

El **ApplicantStateOfResidence** El elemento de datos se configuró seleccionando **Núcleo** en la lista desplegable extensión y **Código personalizado** para el Tipo de elemento de datos como se muestra en la captura de pantalla siguiente
![solicitante-estado-residencia](assets/applicantstateofresidence.png)

El siguiente código personalizado se utilizó para capturar el valor de **_state_** campo de formulario adaptable.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

El **ErrorDeValidación** El elemento de datos se configuró seleccionando **Núcleo** en la lista desplegable extensión y **Código personalizado** para el Tipo de elemento de datos como se muestra en la captura de pantalla siguiente

![error de validación](assets/validation-error.png)

El siguiente código personalizado se escribió para establecer el valor del elemento de datos validationError.

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
