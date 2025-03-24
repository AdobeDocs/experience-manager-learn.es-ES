---
title: Informar sobre los campos de datos de formulario enviados mediante Adobe Analytics
description: Integrar AEM Forms CS con Adobe Analytics para informar sobre campos de datos de formulario
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 42
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---

# Creación de elementos de datos

En la propiedad Tags se han agregado dos nuevos elementos de datos (ApplicantsStateOfResidence y validationError).

![formulario adaptable](assets/data_elements.png)

## ApplicantStateOfResidence

El elemento de datos **ApplicantStateOfResidence** se configuró seleccionando **Core** en la lista desplegable de extensión y **Custom Code** para el tipo de elemento de datos como se muestra en la captura de pantalla siguiente
![solicitante-estado-residencia](assets/applicantstateofresidence.png)

El siguiente código personalizado se usó para capturar el valor del campo de formulario adaptable **_state_**.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

El elemento de datos **ValidationError** se configuró seleccionando **Core** en la lista desplegable de extensión y **Custom Code** para el tipo de elemento de datos como se muestra en la captura de pantalla siguiente

![error de validación](assets/validation-error.png)

El siguiente código personalizado se escribió para establecer el valor del elemento de datos `validationError`.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## Pasos siguientes

[Crear reglas](./rules.md)