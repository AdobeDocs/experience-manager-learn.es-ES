---
title: Determinar la estructura de carpetas y la convención de nombres de archivos
description: La nominación de archivos es quizás la decisión más importante que tomará al implementar Dynamic Media Classic. La estructura de carpetas también es importante. Conozca por qué es tan importante y posible acercarse a la estructura de carpetas y a los nombres de archivos.
sub-product: Dynamic-media
feature: null
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
translation-type: tm+mt
source-git-commit: e7a02900b0582fe9b329e5f9bd568f3c54d8d63d
workflow-type: tm+mt
source-wordcount: '1211'
ht-degree: 0%

---


# Determinar la estructura de carpetas y la convención de nombres de archivos {#folder-structure-filenaming}

Antes de saltar y de cargar en inicio todo el contenido, es aconsejable tener en cuenta la estructura de carpetas que utilizará y, en particular, la convención de nomenclatura de archivos. Probablemente le ahorrará tiempo y tendrá que rehacer tareas más tarde. Es mejor coordinar estas decisiones en todos los grupos.

## Jerarquía de carpetas y convención de nombres de archivos

La nominación de archivos es generalmente la decisión más importante que toma con respecto a la implementación de Dynamic Media Classic. Sin embargo, para comprender por qué es importante, hablemos primero de la estructura de carpetas.

### Jerarquía de carpetas

La jerarquía de carpetas es importante para usted y su compañía únicamente para fines organizativos — las direcciones URL de Dynamic Media Classic solo hacen referencia al nombre del recurso, no a la carpeta o ruta. Independientemente de dónde cargue un archivo, la dirección URL será la misma. Esto es muy diferente a cómo la mayoría de la gente organiza sus imágenes y contenido para la web, pero con Dynamic Media Classic no hay diferencia.

Otra consideración importante es el número de recursos o carpetas que se almacenarán en cada carpeta. Si hay muchos recursos almacenados en una carpeta, el rendimiento se degradará al visualizar recursos en Dynamic Media Classic. No almacene miles de recursos en una carpeta. En su lugar, desarrolle una jerarquía organizativa con menos de 500 recursos o carpetas dentro de una rama determinada de la jerarquía. Este no es un requisito estricto, pero ayudará a mantener tiempos de respuesta aceptables al ver o buscar recursos. De hecho, la recomendación es crear jerarquías amplias y superficiales en lugar de estrechas y profundas.

La forma más sencilla de crear las carpetas es cargar toda la estructura de carpetas mediante FTP y activar la opción **Incluir subcarpetas**. Esta opción hace que Dynamic Media Classic vuelva a crear la estructura de carpetas en el sitio FTP en Dynamic Media Classic.

Queremos que tenga en cuenta la estructura de inicios antes de cargar todos los archivos, ya que es mucho más fácil organizar y administrar los archivos y las carpetas localmente en el equipo que dentro de Dynamic Media Classic. Por ejemplo, solo puede arrastrar y soltar archivos, pero no carpetas enteras, dentro de Dynamic Media Classic.

### Estrategias de carpetas

Para la estrategia de carpetas, considere lo que tiene sentido para su organización. A continuación se presentan algunos casos de nombres de carpetas comunes:

- Reflejar el desglose del producto o del sitio Web. Por ejemplo, si vende ropa, puede tener carpetas para hombres, mujeres y accesorios, y subcarpetas para camisetas y zapatos.
- Estrategia basada en SKU o ID de producto. Por ejemplo, con los minoristas que tienen miles de artículos, puede que sea recomendable utilizar números de SKU o ID de producto como nombres de carpeta.
- Estrategia de marca. Por ejemplo: los fabricantes que tienen varias marcas pueden elegir sus nombres de marca como carpetas de nivel superior.

## Convención de nombres de archivo

El modo en que elija nombrar los archivos es quizás la decisión más importante que tomará con respecto a Dynamic Media Classic. Esto se debe a que todos los recursos de Dynamic Media Classic deben tener nombres únicos, independientemente de dónde estén almacenados en la cuenta.

Todas las direcciones URL y transacciones de Dynamic Media Classic están dirigidas por un ID de recurso, que es un identificador único de recurso en la base de datos. Al cargar un archivo, el ID de recurso se crea tomando el nombre de archivo y eliminando la extensión. Por ejemplo, _896649.jpg_ obtiene el _ID de recurso 896649_.

Reglas relativas a los ID de recursos:

- No hay dos recursos que tengan el mismo nombre en Dynamic Media Classic, independientemente de la carpeta en la que se encuentren los recursos.
- Los nombres distinguen entre mayúsculas y minúsculas. Por ejemplo, silla.jpg, silla.jpg y CHAIR.jpg crearían tres ID de recurso diferentes.
- La práctica recomendada es que los ID de recurso no contengan espacios ni símbolos en blanco. El uso de espacios y símbolos dificulta la implementación, ya que tendrá que codificar estos caracteres mediante URL. Por ejemplo, un espacio &quot; &quot; se convierte en &quot;%20&quot;.

La convención de nombres es básicamente la forma en que se integra con Dynamic Media Classic. Normalmente, los sistemas de back-office no se integran en Dynamic Media Classic porque se trata de un sistema cerrado. Es un socio pasivo, esperando instrucciones en forma de direcciones URL.

La mayoría de los usuarios basa su convención de nombres en sus SKU internos o ID de productos para que cuando se llama a una página web con información sobre ese SKU, la página pueda buscar automáticamente una imagen con un nombre similar. Si no hay conexión entre el nombre del archivo y el SKU o ID, el sistema de back-office deberá realizar un seguimiento manual de cada nombre de archivo y una persona deberá mantener dichas asociaciones. en resumen, mucho trabajo para los equipos de TI y de contenido.

### Estrategias de nominación de archivos

La estrategia de asignación de nombres debe ser flexible para futuras ampliaciones, por lo que puede evitar tener que cambiar el nombre después de iniciar. Estas son algunas estrategias típicas de nomenclatura:

**No hay imágenes alternativas.** En este escenario, solo tiene una imagen por producto y ninguna vista alternativa o de color. Asignaría un nombre exclusivo a cada imagen según su SKU o número de ID de producto único. Cuando se carga la página, la plantilla de página llama al ID de recurso con el mismo número de SKU.

| SKU/PID | Nombre de archivo | ID del recurso |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Este es un sistema muy simple, y bueno si tienes necesidades modestas. Sin embargo, no es muy flexible. Sólo porque no tengas imágenes alternativas hoy no significa que no tengas esas imágenes mañana. El siguiente escenario oferta más flexibilidad.

**Mediante la imagen, vistas alternativas, versiones coloreadas, muestras.** Esta estrategia permite vistas alternativas y/o vistas coloreadas, si las tiene. En lugar de asignar un nombre a la imagen únicamente después del SKU, se agrega un modificador como &quot;_1&quot; y &quot;_2&quot; para vistas alternativas y un código de color de &quot;_RED&quot; o &quot;_BLU&quot; para vistas de color. Si tiene imágenes de color y vistas alternativas para el mismo producto, quizás agregue &quot;_RED_1&quot; y &quot;_RED_2&quot; para la primera y la segunda vista de color rojo. Las muestras recibirán un nombre con el SKU, el código de color y la extensión &quot;_SW&quot;.

| SKU/PID | Categoría | Nombre de archivo | ID del recurso |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Vistas Alt | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Vistas de color | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Muestras | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Conjunto de imágenes o conjunto de muestras |  | AA123 o AA123_SET | -- |

Cuando se trata de conjuntos de colecciones, como conjuntos de imágenes y conjuntos de muestras, el conjunto mismo también debe tener un nombre único. En este caso, se podría dar al conjunto el SKU base como su nombre o el SKU con la extensión &quot;_SET&quot;.

### Convención de nombres y automatización

Una última palabra sobre la importancia de la convención de nombres. Si desea utilizar conjuntos (como conjuntos de imágenes o conjuntos de muestras), una convención de nombre predecible le permitirá automatizar su creación. Cualquier método con secuencias de comandos, como un ajuste preestablecido de conjunto por lotes, del que aprenderá en una sección separada de este tutorial, se puede excluir de una convención de nombre.

El método alternativo es crear manualmente los conjuntos. Si bien la creación manual de conjuntos de imágenes para 200 imágenes puede no ser un gran trabajo, imagine que tiene más de 100.000 imágenes. Esto es cuando la automatización de la creación establecida resulta crucial.
