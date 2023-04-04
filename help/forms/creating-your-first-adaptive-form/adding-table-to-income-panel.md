---
title: Adición de componentes al panel Ingresos
description: Añadiremos una tabla al panel Ingresos . Configure las filas de tabla y utilice el editor de reglas para calcular el total general.
feature: Adaptive Forms
version: 6.4,6.5
thumbnail: 22198.jpg
kt: 4211
topic: Development
role: Developer
level: Beginner
exl-id: e7674c46-259f-4dbd-96db-c40369534911
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Adición de componentes al panel Ingresos {#adding-components-to-income-panel}

Añadiremos una tabla al panel Ingresos . Configure las filas de tabla y utilice el editor de reglas para calcular el total general.

**Agregar y configurar componente de tabla**

>[!VIDEO](https://video.tv.adobe.com/v/22198?quality=12&learn=on)



## Hacer dinámica la tabla de ingresos {#make-the-income-table-dynamic}

**Asegúrese de que está en el modo de edición. El botón de edición se encuentra en la parte superior derecha del explorador.**

* De forma predeterminada, cuando se inserta una tabla en el formulario adaptable, la tabla no es dinámica, lo que significa que no se pueden agregar nuevas filas a la tabla durante la ejecución.

* Actualice el explorador.

* Seleccione Fila1 en la jerarquía de contenido.

* Haga clic en el icono de la llave inglesa para abrir la hoja de propiedades.

* Defina Mínimo y Máximo en 1 y 5 en Repetir configuración y guarde los cambios haciendo clic en el icono de marca de verificación azul. Esto significa que la tabla puede tener un máximo de 5 filas. Para tener un número indefinido de filas, establezca el recuento máximo en -1.

## Crear regla para calcular el total general {#create-rule-to-calculate-grand-total}


>[!VIDEO](https://video.tv.adobe.com/v/22197?quality=12&learn=on)
