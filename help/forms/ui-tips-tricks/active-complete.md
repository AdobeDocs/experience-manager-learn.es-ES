---
title: Aplicar estilo a las fichas de navegación de la izquierda con iconos
description: Agregar iconos para indicar las pestañas activas y completadas
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
duration: 68
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 24%

---

# Agregar iconos para indicar las pestañas activas y completadas

Cuando tenga un formulario adaptable con navegación de pestaña izquierda, es posible que desee mostrar iconos para indicar el estado de la pestaña. Por ejemplo, desea mostrar un icono para indicar la pestaña activa y un icono para indicar la pestaña completada, como se muestra en la captura de pantalla siguiente.

![espaciado de barra de herramientas](assets/active-completed.png)

## Creación de un formulario adaptable

Para crear el formulario de ejemplo se utilizó un formulario adaptable simple basado en la plantilla Básico y la temática Lienzo 3.0.
Los [iconos utilizados en este artículo](assets/icons.zip) se pueden descargar desde aquí.


## Establecer el estilo del estado predeterminado

Abra el formulario en modo de edición
Asegúrese de que está en la capa de estilo y seleccione cualquier pestaña (por ejemplo, pestaña General).
Se encuentra en el estado predeterminado cuando abre el editor de estilos para la pestaña, como se muestra en la captura de pantalla siguiente
![ficha de navegación](assets/navigation-tab.png)

Establezca las propiedades CSS para el estado predeterminado como se muestra a continuación

| Categoría | Nombre de la propiedad | Valor de propiedad |
|:---|:---|:---|
| Dimensiones y posición | Anchura | 50 px |
| Texto | Grosor de fuente | Negrita |
| Texto | Color | #FFF |
| Texto | Altura de la línea | 3 |
| Texto | Alineación de texto | Izquierda |
| Fondo | Color | #056dae |

Guarde los cambios

## Aplicar estilo al estado activo

Asegúrese de que está en estado Activo y aplique estilo a las siguientes propiedades CSS

| Categoría | Nombre de la propiedad | Valor de propiedad |
|:---|:---|:---|
| Dimensiones y posición | Anchura | 50 px |
| Texto | Grosor de fuente | Negrita |
| Texto | Color | #FFF |
| Texto | Altura de la línea | 3 |
| Texto | Alineación de texto | Izquierda |
| Fondo | Color | #056dae |

Aplicar un estilo a la imagen de fondo como se muestra en la captura de pantalla siguiente

Guarde los cambios.



![estado activo](assets/active-state.png)

## Aplicar estilo al estado visitado

Asegúrese de que está en el estado visitado y aplique estilo a las siguientes propiedades

| Categoría | Nombre de la propiedad | Valor de propiedad |
|:---|:---|:---|
| Dimensiones y posición | Anchura | 50 px |
| Texto | Grosor de fuente | Negrita |
| Texto | Color | #FFF |
| Texto | Altura de la línea | 3 |
| Texto | Alineación de texto | Izquierda |
| Fondo | Color | #056dae |

Aplicar un estilo a la imagen de fondo como se muestra en la captura de pantalla siguiente


![estado visitado](assets/visited-state.png)

Guarde los cambios

Previsualice el formulario y pruebe que los iconos funcionan según lo esperado.
