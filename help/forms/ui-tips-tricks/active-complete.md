---
title: Estilo de las pestañas de navegación izquierda con iconos
description: Agregar iconos para indicar las pestañas activas y completadas
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9359
source-git-commit: bbb2c352e8a4297496f248bbbc86252ac7118999
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 15%

---

# Agregar iconos para indicar las pestañas activas y completadas

Cuando tiene un formulario adaptable con la navegación de tabulación izquierda, puede que desee mostrar los iconos para indicar el estado de la pestaña. Por ejemplo, desea mostrar un icono para indicar la pestaña activa y el icono para indicar la pestaña completada, tal y como se muestra en la captura de pantalla siguiente.

![espaciado de la barra de herramientas](assets/active-completed.png)

## Crear un formulario adaptable

Para crear el formulario de ejemplo se utilizó un formulario adaptable sencillo basado en la plantilla Básico y el tema Lienzo 3.0.
La variable [iconos utilizados en este artículo](assets/icons.zip) se puede descargar desde aquí.


## Estilo del estado predeterminado

Abra el formulario en modo de edición Asegúrese de que está en la capa de estilo y seleccione cualquier pestaña (por ejemplo, la ficha General ).
Se encuentra en el estado predeterminado al abrir el editor de estilos para la pestaña como se muestra en la captura de pantalla siguiente
![navigation-tab](assets/navigation-tab.png)

Establezca las propiedades CSS para el estado predeterminado como se muestra a continuación | Categoría | Nombre de propiedad | Valor de propiedad | |:—|:—|:—| | Dimension y posición | Anchura | 50px | | Texto | Grosor de fuente | Negrita | | Texto | Color | #FFF | |Texto | Altura de línea | 3 | |Texto | Alineación de texto | Izquierda | |Fondo| Color | #056dae |

Guarde los cambios

## Estilo del estado activo

Asegúrese de que está en el estado Activo y aplique estilo a las siguientes propiedades CSS

| Categoría | Nombre de propiedad | Valor de propiedad |
|:---|:---|:---|
| Dimension y posición | Anchura | 50px |
| Texto | Grosor de fuente | Negrita |
| Texto | Color | #FFF |
| Texto | Altura de la línea | 3 |
| Texto | Alineación de texto | Izquierda |
| Fondo | Color | #056dae |

Establezca el estilo de la imagen de fondo como se muestra en la captura de pantalla siguiente.

Guarde los cambios.



![active-state](assets/active-state.png)

## Estilo del estado visitado

Asegúrese de que está en el estado visitado y aplique estilo a las siguientes propiedades

| Categoría | Nombre de propiedad | Valor de propiedad |
|:---|:---|:---|
| Dimension y posición | Anchura | 50px |
| Texto | Grosor de fuente | Negrita |
| Texto | Color | #FFF |
| Texto | Altura de la línea | 3 |
| Texto | Alineación de texto | Izquierda |
| Fondo | Color | #056dae |

Establezca el estilo de la imagen de fondo como se muestra en la captura de pantalla siguiente.


![estados visitados](assets/visited-state.png)

Guarde los cambios

Obtenga una vista previa del formulario y pruebe que los iconos funcionan según lo esperado.
