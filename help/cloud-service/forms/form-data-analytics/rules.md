---
title: Informar sobre los campos de datos de formulario enviados mediante Adobe Analytics
description: Integrar AEM Forms CS con Adobe Analytics para informar sobre campos de datos de formulario
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 55
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# Definición de la regla

En la propiedad Etiquetas hemos creado 2 nuevas [reglas](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**Error de validación de campo y envío de formulario**).

![formulario adaptable](assets/rules.png)


## Error de validación de campo

El **Error de validación de campo** La regla se activa cada vez que se produce un error de validación en el campo del formulario adaptable. Por ejemplo, en nuestro formulario si el número de teléfono o el correo electrónico no tienen el formato esperado, se muestra un mensaje de error de validación.

La regla de error de validación de campo se configura estableciendo el evento en _**Adobe Experience Manager Forms-Error**_ como se muestra en la captura de pantalla



![solicitante-estado-residencia](assets/field_validation_error_rule.png)

Adobe Analytics - Set Variables está configurado de la siguiente manera

![acción set](assets/field_validation_action_rule.png)

## Regla de envío de formulario

La regla de envío de formulario se activa cada vez que se envía correctamente un formulario adaptable.

La regla de envío de formulario se configura con la variable _**Adobe Experience Manager Forms - Enviar**_ evento

![form-submit-rule](assets/form-submit-rule.png)

En la regla de envío de formulario, el valor del elemento de datos _**ApplicationsStateOfResidence**_ está asignado a prop5 y el valor del elemento de datos FormTitle está asignado a prop8.

Las variables Adobe Analytics - Set se configuran de la siguiente manera.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Cuando esté listo para probar el código de etiquetas,[publicar los cambios realizados en las etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) uso del flujo de publicación.

## Pasos siguientes

[Prueba de la solución](./test.md)