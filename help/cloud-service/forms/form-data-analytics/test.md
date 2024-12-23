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
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# Prueba de la solución

Obtenga una vista previa y envíe el formulario con varias combinaciones de valores de formulario. Espere entre varios y 30 minutos para ver los datos en los informes de Adobe Analytics. Los conjuntos de datos a props aparecen en los informes antes que los conjuntos de datos a eVars.

## Grupo de informes

Los datos del formulario capturados en Adobe Analytics se presentan en formato de anillo

**Envíos por estado**

![aplicantsbystate](assets/donut.png)

Errores de validación de campo

![error de validación de campo](assets/donut-field-validation.png)

## Depuración

Asegúrese de que el formulario adaptable utiliza el mismo contenedor de configuración que contiene la configuración de Launch de Adobe.

Para confirmar que el formulario está enviando datos a Adobe Analytics, haga lo siguiente

* Abra las herramientas para desarrolladores en el explorador.
* Introduzca el siguiente texto en el panel de la consola.

```javascript
_satellite.setDebug(true)
```

Interactúe con el formulario mientras mantiene abierta la ventana de la consola. Deberías ver algo como esto

![console-debug](assets/debug.png)

## Usar Adobe Experience Platform Debugger

Agregue la [extensión del depurador de AEP](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) a su explorador (debe iniciar sesión) para obtener más información sobre depuración

![depurador de plataforma](assets/platform-debugger.png)

## Felicitaciones

Se ha integrado correctamente AEM Forms as a Cloud Service con Adobe Analytics para informar sobre los campos de datos de formulario.