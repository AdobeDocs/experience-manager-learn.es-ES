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
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Prueba de la solución

Obtenga una vista previa y envíe el formulario con varias combinaciones de valores de formulario. Espere entre varios y 30 minutos para ver los datos en los informes de Adobe Analytics. Los conjuntos de datos a props aparecen en los informes antes que los conjuntos de datos a eVars.

## Grupo de informes

Los datos del formulario capturados en Adobe Analytics se presentan en formato de anillo

**Envíos por Estado**

![aplicantsbystate](assets/donut.png)

Errores de validación de campo

![field-validation-error](assets/donut-field-validation.png)

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

Añada el [Extensión de AEP Debugger](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) en el explorador (debe iniciar sesión) para obtener más información sobre depuración

![platform-debugger](assets/platform-debugger.png)
