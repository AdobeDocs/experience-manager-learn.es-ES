---
title: Uso del sistema de estilos en AEM Forms
description: Definir la política de estilo para el componente Botón
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 52205a93-d03c-430c-a707-b351ab333939
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 3%

---

# Defina el estilo en la directiva para el componente

* Inicie sesión en la instancia local de AEM lista para la nube y navegue hasta Herramientas | General | Plantillas | nombre del proyecto.

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
