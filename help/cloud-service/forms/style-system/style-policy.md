---
title: Uso del sistema de estilos en AEM Forms
description: Definir la política de estilo para el componente Botón
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 3%

---

# Defina el estilo en la directiva para el componente

* AEM Inicie sesión en la instancia local de Cloud Ready y navegue hasta Herramientas. | General | Plantillas | nombre del proyecto.

* Seleccione y abra la plantilla **En blanco con componentes principales** en el modo de edición.
* Haga clic en el icono de directiva del componente Botón para abrir el editor de directivas.

* ![botón-directiva](assets/button-policy.png)

Defina la política como se muestra a continuación

![button-policy-details](assets/styling-policy.png)

Hemos definido 2 estilos/variaciones llamadas Marketing y Corporate. Estas variaciones están asociadas con las clases CSS correspondientes.**Asegúrese de que no haya espacio antes y después de los nombres de clase CSS**.
Guarde los cambios.

| Estilo | Clase de CSS |
|-----------|------------------------------------|
| Marketing | cmp-adaptiveform-button-marketing |
| Corporativo | cmp-adaptiveform-button-corporate |

Estas clases CSS se definirán en el archivo scss del componente (_button.scss).

## Siguientes pasos

[Definir clases CSS](./create-variations.md)
