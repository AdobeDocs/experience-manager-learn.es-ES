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
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# Definición de la regla

En la propiedad Tags creamos 2 nuevas [reglas](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html?lang=es) (**Error de validación de campo y FormSubmit**).

![formulario adaptable](assets/rules.png)


## Error de validación de campo

La regla **Error de validación de campo** se activa cada vez que hay un error de validación en el campo del formulario adaptable. Por ejemplo, en nuestro formulario si el número de teléfono o el correo electrónico no tienen el formato esperado, se muestra un mensaje de error de validación.

La regla de error de validación de campo se configura al establecer el evento en _&#x200B;**Adobe Experience Manager Forms-Error**&#x200B;_, como se muestra en la captura de pantalla



![solicitante-estado-residencia](assets/field_validation_error_rule.png)

Adobe Analytics - Set Variables está configurado de la siguiente manera

![establecer acción](assets/field_validation_action_rule.png)

## Regla de envío de formulario

La regla de envío de formulario se activa cada vez que se envía correctamente un formulario adaptable.

La regla de envío de formulario se ha configurado con el evento _&#x200B;**Adobe Experience Manager Forms - Submit**&#x200B;_

![form-submit-rule](assets/form-submit-rule.png)

En la regla de envío de formulario, el valor del elemento de datos _&#x200B;**ApplicantsStateOfResidence**&#x200B;_ se asigna a prop5 y el valor del elemento de datos FormTitle se asigna a prop8.

Las variables Adobe Analytics - Set se configuran de la siguiente manera.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Cuando esté listo para probar el código de etiquetas,[publique los cambios realizados en las etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html?lang=es) mediante el flujo de publicación.

## Pasos siguientes

[Prueba de la solución](./test.md)