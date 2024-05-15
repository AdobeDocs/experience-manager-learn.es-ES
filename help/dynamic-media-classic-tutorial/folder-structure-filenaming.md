---
title: Determine la estructura de carpetas y la convención de nombres de archivos
description: La asignación de nombres de archivo es quizás la decisión más importante que tomará al implementar Dynamic Media Classic. La estructura de carpetas es igualmente importante. Aprenda por qué es tan importante y posibles enfoques para la estructura de carpetas y los nombres de archivo.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
duration: 253
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1202'
ht-degree: 0%

---

# Determine la estructura de carpetas y la convención de nombres de archivos {#folder-structure-filenaming}

Antes de saltar y empezar a cargar todo el contenido, es aconsejable tener en cuenta la estructura de carpetas que se utilizará y, en particular, la convención de nombres de archivos. Es probable que le ahorre tiempo y tenga que rehacer las tareas más adelante. Es mejor coordinar estas decisiones en todos los grupos.

## Jerarquía de carpetas y convención de nombres de archivos

La nomenclatura de archivos es generalmente la decisión más importante que toma con respecto a la implementación de Dynamic Media Classic. Sin embargo, para comprender por qué es importante, hablemos primero sobre la estructura de carpetas.

### Jerarquía de carpetas

La jerarquía de carpetas es importante para usted y para su compañía únicamente a efectos organizativos: las direcciones URL de Dynamic Media Classic solo hacen referencia al nombre del recurso, no a la carpeta o ruta. Independientemente de dónde cargue un archivo, la dirección URL es la misma. Esto es bastante diferente a cómo la mayoría de la gente organiza sus imágenes y contenido para la web, pero con Dynamic Media Classic no hace ninguna diferencia.

Otra consideración importante es el número de recursos o carpetas que se almacenarán en cada carpeta. Si hay muchos recursos almacenados en una carpeta, el rendimiento se degrada al ver los recursos en Dynamic Media Classic. No almacene miles de recursos en una carpeta. En su lugar, desarrolle una jerarquía organizativa con menos de 500 recursos o carpetas dentro de una rama determinada de la jerarquía. Este no es un requisito estricto, pero ayuda a mantener tiempos de respuesta aceptables al ver o buscar recursos. De hecho, la recomendación es crear jerarquías anchas y superficiales en lugar de estrechas y profundas.

La forma más sencilla de crear las carpetas es cargar toda la estructura de carpetas mediante FTP y activar la opción **Incluir subcarpetas**. Esta opción hace que Dynamic Media Classic vuelva a crear la estructura de carpetas en el sitio FTP de Dynamic Media Classic.

Queremos que tenga en cuenta la estructura de carpetas antes de empezar a cargar todos los archivos, ya que es mucho más fácil organizar y administrar los archivos y carpetas localmente en el equipo que dentro de Dynamic Media Classic. Por ejemplo, solo puede arrastrar y soltar archivos, pero no carpetas completas, dentro de Dynamic Media Classic.

### Estrategias de carpeta

Para la estrategia de carpetas, considere lo que tiene sentido para su organización. Estos son algunos escenarios comunes de nomenclatura de carpetas:

- Sitio web espejo o desglose de productos. Por ejemplo, si vende ropa, puede tener carpetas para hombres, mujeres y accesorios, y subcarpetas para camisas y zapatos.
- Estrategia basada en SKU o ID del producto. Por ejemplo, en el caso de los comercios que tienen miles de artículos, puede ser recomendable usar números de SKU o ID de productos como nombres de carpetas.
- Estrategia de marca. Por ejemplo, los fabricantes que tienen varias marcas pueden elegir sus nombres de marca como carpetas de nivel superior.

## Convención de nomenclatura de archivos

La forma en que elija asignar un nombre a los archivos es quizás la decisión anticipada más importante que tomará con respecto a Dynamic Media Classic. Esto se debe a que todos los recursos de Dynamic Media Classic deben tener nombres únicos, independientemente de dónde se almacenen en la cuenta.

Todas las direcciones URL y transacciones en Dynamic Media Classic están gobernadas por un ID de recurso, que es el identificador único de un recurso en la base de datos. Al cargar un archivo, el ID de recurso se crea tomando el nombre del archivo y eliminando la extensión. Por ejemplo, _896649.jpg_ obtiene el recurso _896649 de ID_.

Reglas relativas a los ID de recurso:

- Dos recursos no pueden tener el mismo nombre en Dynamic Media Classic, independientemente de la carpeta en la que se encuentren.
- Los nombres distinguen entre mayúsculas y minúsculas. Por ejemplo, Chair.jpg, chair.jpg y CHAIR.jpg crearían tres ID de recurso diferentes.
- Como práctica recomendada, los ID de recurso no deben contener espacios ni símbolos en blanco. El uso de espacios y símbolos dificulta la implementación, ya que tendrá que codificar estos caracteres mediante URL. Por ejemplo, un espacio &quot;&quot; se convierte en &quot;%20&quot;.&quot;

La convención de nombres es esencialmente la forma en que se integra con Dynamic Media Classic. Normalmente, no integra los sistemas de back-office en Dynamic Media Classic porque es un sistema cerrado. Es un socio pasivo que espera instrucciones en forma de URL.

La mayoría de los usuarios basan su convención de nombres en torno a su SKU interno o ID de producto, de modo que cuando se abre una página web con información sobre ese SKU, la página puede buscar automáticamente una imagen con un nombre similar. Si no hay conexión entre el nombre del archivo y el SKU o ID, su sistema de back-office deberá realizar un seguimiento manual de cada nombre de archivo y una persona tendrá que mantener esas asociaciones, en resumen, mucho trabajo tanto para los equipos de TI como para los de contenido.

### Estrategias de nomenclatura de archivos

La estrategia de nomenclatura debe ser flexible para futuras expansiones, de modo que pueda evitar tener que cambiar el nombre después de iniciar. Estas son algunas estrategias de nomenclatura típicas:

**No hay imágenes alternativas.** En esta situación, solo tiene una imagen por producto y no hay vistas alternativas o de color. Asigne un nombre estricto a cada imagen según su SKU único o número de ID de producto. Cuando se carga la página, la plantilla de página llama al ID del recurso con el mismo número de SKU.

| SKU/PID | Nombre de archivo | ID de recurso |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Este es un sistema muy simple, y bueno si tiene necesidades modestas. Sin embargo, no es muy flexible. El hecho de que no tenga imágenes alternativas hoy no significa que no las tendrá mañana. El siguiente escenario ofrece más flexibilidad.

**Uso de la imagen, vistas alternativas, versiones de color y muestras.** Esta estrategia permite vistas alternativas y/ o vistas de color, si las tiene. En lugar de asignar un nombre a la imagen solo después del SKU, se añade un modificador como &quot;_1&quot; y &quot;_2&quot; para vistas alternativas, y un código de color de &quot;_RED&quot; o &quot;_BLU&quot; para vistas de color. Si tiene imágenes en color y vistas alternativas para el mismo producto, quizás agregaría &quot;_RED_1&quot; y &quot;_RED_2&quot; para la primera y la segunda vista en color rojo. Las muestras recibirán el nombre del SKU, el código de color y la extensión &quot;_SW&quot;.

| SKU/PID | Categoría | Nombre de archivo | ID de recurso |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Vistas Alt | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|         | Vistas en color | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|         | Muestras | AA123_BLU_SW.tif | AA123_BLU_SW |
|         | Conjunto de imágenes o conjunto de muestras |                                             | AA123 o AA123_SET | -- |

Al tratar con colecciones de conjuntos, como conjuntos de imágenes y conjuntos de muestras, el propio conjunto también debe tener un nombre único. Por lo tanto, en este caso, el conjunto podría recibir el SKU base como su nombre o el SKU con la extensión &quot;_SET&quot;.

### Convención de nomenclatura y automatización

Una última palabra sobre la importancia de la convención de nombres. Si se desea utilizar conjuntos (como conjuntos de imágenes o conjuntos de muestras), una convención de nombres predecible permite automatizar su creación. Cualquier método con script, como un ajuste preestablecido de conjunto por lotes, que aprenderá en una sección independiente de este tutorial, se puede derivar de una convención de nombres.

El método alternativo es crear manualmente los conjuntos. Aunque la creación manual de conjuntos de imágenes para 200 imágenes puede no ser un gran trabajo, imagine que tiene más de 100 000 imágenes. Es entonces cuando la automatización de la creación de conjuntos se vuelve crucial.
