---
title: Aplicar estilo a las fichas de navegación de la izquierda con iconos
description: Agregar iconos para indicar las pestañas activas y completadas
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 17%

---

# Agregar iconos para indicar las pestañas activas y completadas

Cuando tenga un formulario adaptable con navegación de pestaña izquierda, es posible que desee mostrar iconos para indicar el estado de la pestaña. Por ejemplo, desea mostrar un icono para indicar la pestaña activa y un icono para indicar la pestaña completada, como se muestra en la captura de pantalla siguiente.

![espaciado de barra de herramientas](assets/active-completed.png)

## Crear un formulario adaptable

Para crear el formulario de ejemplo se utilizó un formulario adaptable simple basado en la plantilla Básico y la temática Lienzo 3.0.
El [iconos utilizados en este artículo](assets/icons.zip) se puede descargar desde aquí.


## Establecer el estilo del estado predeterminado

Abra el formulario en modo de edición Asegúrese de que está en la capa de estilo y seleccione cualquier pestaña (por ejemplo, la pestaña General ).
Se encuentra en el estado predeterminado cuando abre el editor de estilos para la pestaña, como se muestra en la captura de pantalla siguiente
![navigation-tab](assets/navigation-tab.png)

Establezca las propiedades CSS para el estado predeterminado como se muestra a continuación | Categoría | Nombre de propiedad | Valor de propiedad | |:—|:—|:—| | Dimension y posición | Ancho | 50 px | | Texto | Grosor de fuente| Negrita | | Texto | Color | #FFF | |Texto | Altura de línea| 3 | |Texto | Alineación de texto | Izquierda | Color de |fondo| | #056dae |

Guarde los cambios

## Aplicar estilo al estado activo

Asegúrese de que está en estado Activo y aplique estilo a las siguientes propiedades CSS

| Categoría | Nombre de la propiedad | Valor de propiedad |
|:---|:---|:---|
| Dimensiones y posición | Anchura | 50 px |
| Texto | Grosor de fuente | Negrita |
| Texto | Color | #FFF |
| Texto | Altura de la línea | 3 |
| Texto | Texto  Alinear | Izquierda |
| Fondo | Color | #056dae |

Aplicar un estilo a la imagen de fondo como se muestra en la captura de pantalla siguiente

Guarde los cambios.



![active-state](assets/active-state.png)

## Aplicar estilo al estado visitado

Asegúrese de que está en el estado visitado y aplique estilo a las siguientes propiedades

| Categoría | Nombre de la propiedad | Valor de propiedad |
|:---|:---|:---|
| Dimensiones y posición | Anchura | 50 px |
| Texto | Grosor de fuente | Negrita |
| Texto | Color | #FFF |
| Texto | Altura de la línea | 3 |
| Texto | Texto  Alinear | Izquierda |
| Fondo | Color | #056dae |

Aplicar un estilo a la imagen de fondo como se muestra en la captura de pantalla siguiente


![visit-state](assets/visited-state.png)

Guarde los cambios

Previsualice el formulario y pruebe que los iconos funcionan según lo esperado.
