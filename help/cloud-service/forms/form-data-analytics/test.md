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
source-wordcount: '180'
ht-degree: 1%

---

# Probar la solución

Obtenga una vista previa del formulario y envíelo utilizando varias combinaciones de valores de formulario. Espere entre varios y 30 minutos para ver los datos en los informes de Adobe Analytics. Los datos configurados en props aparecen en los informes antes de que los datos configurados en eVars.

## Grupo de informes

Los datos del formulario capturados en Adobe Analytics se presentan en formato de anillo

**Presentaciones por Estado**

![aplicantsbystate](assets/donut.png)

Errores de validación del campo

![field-validation-error](assets/donut-field-validation.png)

## Depuración

Asegúrese de que el formulario adaptable utiliza el mismo contenedor de configuración que contiene la configuración de Launch de Adobe.

Para confirmar que el formulario envía datos a Adobe Analytics, haga lo siguiente

* Abra las herramientas para desarrolladores en el explorador.
* Introduzca el texto siguiente en el panel Consola.

```javascript
_satellite.setDebug(true)
```

Interactúe con el formulario mientras mantiene abierta la ventana de la consola. Debería ver algo como esto

![console-debug](assets/debug.png)

## Usar Adobe Experience Platform Debugger

Agregue la variable [Extensión del depurador de AEP](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) al explorador (es necesario que inicie sesión) para obtener más información de depuración

![platform-debugger](assets/platform-debugger.png)





