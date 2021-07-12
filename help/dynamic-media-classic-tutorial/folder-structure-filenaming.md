---
title: Determine la estructura de carpetas y la convención de nombres de archivos
description: La asignación de nombres a archivos es quizás la decisión más importante que debe tomar al implementar Dynamic Media Classic. La estructura de carpetas también es importante. Aprenda por qué es tan importante y posible adoptar enfoques para la estructura de carpetas y los nombres de archivos.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
topic: Administración de contenido
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 0%

---


# Determine la estructura de carpetas y la convención de nombres de archivos {#folder-structure-filenaming}

Antes de entrar y empezar a cargar todo el contenido, es aconsejable tener en cuenta la estructura de carpetas que utilizará y, en particular, la convención de nomenclatura de archivos. Es probable que le ahorre tiempo y tenga que rehacer tareas más tarde. Es mejor coordinar estas decisiones entre todos los grupos.

## Jerarquía de carpetas y convención de nomenclatura de archivos

La asignación de nombres a archivos es generalmente la decisión más importante que toma en cuanto a la implementación de Dynamic Media Classic. Sin embargo, para entender por qué es importante, hablemos primero de la estructura de carpetas.

### Jerarquía de carpetas

La jerarquía de carpetas es importante para usted y para su empresa únicamente con fines organizativos: las direcciones URL de Dynamic Media Classic solo hacen referencia al nombre del recurso, no a la carpeta o la ruta. Independientemente de dónde cargue un archivo, la dirección URL será la misma. Esto es muy diferente a cómo la mayoría de la gente organiza sus imágenes y contenido para la web, pero con Dynamic Media Classic no marca ninguna diferencia.

Otra consideración importante es el número de recursos o carpetas que se van a almacenar en cada carpeta. Si hay muchos recursos almacenados en una carpeta, el rendimiento se degradará al ver los recursos en Dynamic Media Classic. No almacene miles de recursos en una carpeta. En su lugar, desarrolle una jerarquía organizativa con menos de 500 activos o carpetas dentro de una rama determinada de la jerarquía. Este no es un requisito estricto, pero ayudará a mantener tiempos de respuesta aceptables al ver o buscar recursos. De hecho, se recomienda crear jerarquías amplias y superficiales en lugar de estrechas y profundas.

La forma más sencilla de crear las carpetas es cargar toda la estructura de carpetas mediante FTP y activar la opción **Incluir subcarpetas**. Esta opción hace que Dynamic Media Classic vuelva a crear la estructura de carpetas en el sitio FTP de Dynamic Media Classic.

Queremos que tenga en cuenta la estructura de carpetas antes de empezar a cargar todos los archivos, ya que es mucho más fácil organizar y administrar los archivos y carpetas localmente en el equipo que en Dynamic Media Classic. Por ejemplo, solo puede arrastrar y soltar archivos, pero no carpetas enteras, dentro de Dynamic Media Classic.

### Estrategias de carpeta

Para la estrategia de carpetas, considere lo que tiene sentido para su organización. Estos son algunos casos comunes de asignación de nombres a carpetas:

- sitio web espejo o desglose de producto. Por ejemplo, si vende ropa, puede tener carpetas para Hombres, Mujeres y Accesorios y subcarpetas para Camisas y Zapatos.
- SKU o estrategia basada en el ID de producto. Por ejemplo, con los minoristas que tienen miles de artículos, puede que sea recomendable utilizar números de SKU o ID de producto como nombres de carpeta.
- Estrategia de marca. Por ejemplo, los fabricantes que tengan varias marcas pueden elegir sus nombres de marca como carpetas de nivel superior.

## Convención de nomenclatura de archivos

La forma en que elija nombrar sus archivos es quizás la decisión temprana más importante que tomará con respecto a Dynamic Media Classic. Esto se debe a que todos los recursos de Dynamic Media Classic deben tener nombres únicos, independientemente de dónde se almacenen en la cuenta.

Todas las direcciones URL y transacciones en Dynamic Media Classic están impulsadas por un ID de recurso, que es el identificador único de un recurso en la base de datos. Al cargar un archivo, el ID del recurso se crea tomando el nombre de archivo y eliminando la extensión. Por ejemplo, _896649.jpg_ obtiene el _ID 896649_ del recurso.

Reglas relativas a los ID de recurso:

- No hay dos recursos que tengan el mismo nombre en Dynamic Media Classic, independientemente de la carpeta en la que se encuentren los recursos.
- Los nombres distinguen entre mayúsculas y minúsculas. Por ejemplo, chair.jpg, chair.jpg y CHAIR.jpg crearían tres ID de recurso diferentes.
- Como práctica recomendada, los ID de recurso no deben contener espacios o símbolos en blanco. El uso de espacios y símbolos dificulta la implementación, ya que tendrá que codificar estos caracteres con la dirección URL. Por ejemplo, un espacio &quot;&quot; se convierte en &quot;%20&quot;.

La convención de nombres es básicamente la forma en que se integra con Dynamic Media Classic. Normalmente, no integra los sistemas de back-office en Dynamic Media Classic porque es un sistema cerrado. Es un socio pasivo, esperando instrucciones en forma de URL.

La mayoría de los usuarios basan su convención de nombres en torno a su SKU o ID de producto internos para que cuando se llama a una página web con información sobre ese SKU, la página pueda buscar automáticamente una imagen que tenga un nombre similar. Si no hay conexión entre el nombre de archivo y el SKU o ID, el sistema de back-office deberá realizar un seguimiento manual de cada nombre de archivo y una persona deberá mantener esas asociaciones, en resumen, mucho trabajo tanto para los equipos de TI como de contenido.

### Estrategias de nomenclatura de archivos

La estrategia de asignación de nombres debe ser flexible para futuras expansiones, por lo que puede evitar tener que cambiar el nombre después de lanzarse. Estas son algunas estrategias típicas de asignación de nombres:

**No hay imágenes alternativas.** En esta situación, solo tiene una imagen por producto y ninguna vista alternativa o de color. Asignaría un nombre estricto a cada imagen según su SKU o número de ID de producto único. Cuando se carga la página, la plantilla de página llama al ID del recurso con el mismo número de SKU.

| SKU o PID | Nombre de archivo | ID del recurso |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Este es un sistema muy simple, y bueno si tienes necesidades modestas. Sin embargo, no es muy flexible. Solo porque no tengan imágenes alternativas hoy no significa que no tendrán esas imágenes mañana. El siguiente escenario ofrece más flexibilidad.

**Uso de la imagen, vistas alternativas, versiones de color, muestras.** Esta estrategia permite vistas alternativas y/o de color, si las tiene. En lugar de asignar un nombre a la imagen únicamente después del SKU, agregue un modificador como &quot;_1&quot; y &quot;_2&quot; para vistas alternativas y un código de color de &quot;_RED&quot; o &quot;_BLU&quot; para vistas de color. Si tiene imágenes de color y vistas alternativas para el mismo producto, tal vez agregaría &quot;_RED_1&quot; y &quot;_RED_2&quot; para la primera y la segunda vista de color rojo. A las muestras se les asignaría el nombre SKU, el código de color y la extensión &quot;_SW&quot;.

| SKU o PID | Categoría | Nombre de archivo | ID del recurso |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Vistas Alt | A123_1.tif AA123_2.tif AA123_3.tif | A123_1 A123_2 A123_3 |
|  | Vistas de colores | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Muestras | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Conjunto de imágenes o conjunto de muestras |  | A123 o A123_SET | — |

Cuando se trata de conjuntos de colecciones, como conjuntos de imágenes y conjuntos de muestras, el propio conjunto debe tener también un nombre único. Por lo tanto, en este caso, se podría dar al conjunto el SKU base como su nombre o el SKU con la extensión &quot;_SET&quot;.

### Convención de nomenclatura y automatización

Una última palabra sobre la importancia de la convención de nombres. Si desea utilizar conjuntos (como conjuntos de imágenes o conjuntos de muestras), una convención de nombres predecible le permitirá automatizar su creación. Cualquier método con secuencias de comandos, como un ajuste preestablecido de conjunto de lotes, del que aprenda en una sección independiente de este tutorial, se puede excluir de una convención de nomenclatura.

El método alternativo es crear manualmente los conjuntos. Aunque la creación manual de conjuntos de imágenes para 200 imágenes puede no ser un gran trabajo, imaginen si tiene más de 100.000 imágenes. Es entonces cuando la automatización de la creación de conjuntos se vuelve crucial.
