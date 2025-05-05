---
title: Conjuntos de imágenes, muestras, giros y medios mixtos
description: Una de las capacidades más útiles y potentes de Dynamic Media Classic es su compatibilidad para crear conjuntos de medios enriquecidos como conjuntos de imágenes, muestras, giros y medios mixtos. Descubra qué es cada conjunto de medios enriquecidos y cómo crear cada tipo en Dynamic Media Classic. A continuación, obtenga más información sobre los ajustes preestablecidos de conjuntos de lotes, que automatizan el proceso de creación de conjuntos de medios enriquecidos al cargar.
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
duration: 280
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1359'
ht-degree: 1%

---

# Conjuntos de imágenes, muestras, giros y medios mixtos {#media-sets}

Más allá de las imágenes únicas para un tamaño y zoom dinámico, las colecciones de conjuntos de Dynamic Media Classic permiten una mejor experiencia en línea. En esta sección del tutorial se explica cómo crear los siguientes conjuntos de medios enriquecidos en Dynamic Media Classic:

- Conjunto de imágenes
- Conjunto de muestras
- Conjunto de giros
- Conjunto de medios mixtos

También se explica cómo utilizar los ajustes preestablecidos de conjunto por lotes para automatizar la creación de conjuntos mediante una carga.

## Todo Lo Que Siempre Querías Saber Sobre Conjuntos

Junto al tamaño dinámico básico y el zoom, los conjuntos son probablemente el subproducto de Dynamic Media Classic más utilizado. Los conjuntos son recursos esencialmente &quot;virtuales&quot; que no contienen imágenes reales, pero que consisten en un conjunto de relaciones con otras imágenes o vídeos. El principal atractivo de los sets es que son miniaplicaciones que están listas para usarse. Con esto queremos decir que cada visor de conjunto contiene su propia lógica e interfaz para que todo lo que tenga que hacer sea llamarles en el sitio. Además, solo es necesario realizar el seguimiento de un único ID de recurso por conjunto, en lugar de tener que administrar todos los recursos y relaciones de los miembros por su cuenta.

Al crear un conjunto, este se administra como un recurso independiente que debe marcarse para publicarse antes de poder servirlo desde una dirección URL. Todos los recursos de sus miembros también deben publicarse.

### Tipos de conjuntos

Vamos a conocer los cuatro tipos de conjuntos que puede crear en Dynamic Media Classic: Conjuntos de imágenes, muestras, giros y medios mixtos.

## Conjunto de imágenes

Este es el tipo de conjunto más común. Normalmente se utiliza para vistas alternativas del mismo elemento. Consiste en varias imágenes que se cargan en el visor haciendo clic en la miniatura asociada de esa imagen.

![imagen](assets/media-sets/image-set-1.jpg)

_Ejemplo de un conjunto de imágenes_

La URL del conjunto de imágenes anterior puede aparecer de la siguiente manera:

![imagen](assets/media-sets/image-set-url-1.png)

- Obtenga más información sobre los conjuntos de imágenes con [Inicio rápido para los conjuntos de imágenes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html?lang=es).
- Aprenda a [crear un conjunto de imágenes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html?lang=es#creating-an-image-set).

### Conjunto de muestras

Este tipo de conjunto se utiliza generalmente para mostrar vistas de color del mismo elemento. Consiste en pares de imágenes y muestras de color.

La principal diferencia entre una muestra y un conjunto de imágenes es que los conjuntos de muestras utilizan una imagen diferente como muestra en la que se puede hacer clic, mientras que los conjuntos de imágenes utilizan una versión en miniatura en miniatura de la imagen original en la que se puede hacer clic.

Los conjuntos de muestras no colorean las imágenes (una idea errónea común). Las imágenes simplemente se están intercambiando, exactamente como en un conjunto de imágenes. Las minimuestras podrían haberse creado con Photoshop, cada color podría haberse fotografiado por separado o la herramienta Recortar de Dynamic Media Classic podría haberse utilizado para crear una muestra de una de las imágenes en color.

![imagen](assets/media-sets/image-set-2.jpg)

_Ejemplo de conjunto de muestras_

La URL del conjunto de muestras anterior podría aparecer como:

![imagen](assets/media-sets/image-set_url.png)

- Obtenga más información sobre los conjuntos de muestras con [Inicio rápido para los conjuntos de muestras](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html?lang=es).
- Aprenda a [crear un conjunto de muestras](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html?lang=es#creating-a-swatch-set).

### Conjunto de giros

Este conjunto se utiliza generalmente para mostrar una vista de 360 grados de un elemento. Al igual que los conjuntos de muestras, los conjuntos de giros no utilizan magia 3D; el verdadero trabajo consiste en crear muchas fotos de una imagen desde todos los lados. El visor simplemente le permite alternar entre las imágenes como una animación stop-motion.

Los conjuntos de giros pueden girar en una dirección a lo largo de un solo eje o, si se crean alternativamente como un conjunto de giros 2D, girar en varios ejes. Por ejemplo, un coche puede girarse mientras todas las ruedas están en el suelo, y luego se puede &quot;voltear&quot; hacia arriba y girar también en sus ruedas traseras. Para un conjunto de giros 2D configurado correctamente, el número de imágenes por fila para cada eje debe ser el mismo. En otras palabras, si está girando en dos ejes, necesita el doble de imágenes que un solo giro de ángulo.

![imagen](assets/media-sets/image-set-3.png)

_Ejemplo de un conjunto de giros_

La dirección URL del conjunto de giros anterior puede aparecer de la siguiente manera:

![imagen](assets/media-sets/spin-set.png)

- Obtenga más información sobre los conjuntos de giros con [Inicio rápido para los conjuntos de giros](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html?lang=es).
- Aprenda a [crear un conjunto de giros](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html?lang=es#creating-a-spin-set).

## Conjunto de medios mixtos

Este es un conjunto combinado. Permite combinar cualquiera de los conjuntos anteriores, así como agregar vídeo, en un solo visor. En este flujo de trabajo, cree primero cualquiera de los conjuntos de componentes y, a continuación, agrúpelos en un conjunto de medios mixtos.

![imagen](assets/media-sets/image-set-4.png)

_Ejemplo de un conjunto de medios mixtos_

La URL del conjunto de medios mixtos anterior podría aparecer como:

![imagen](assets/media-sets/image-set-url-1.png)

- Obtenga más información sobre los conjuntos de medios mixtos con [Inicio rápido para los conjuntos de medios mixtos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html?lang=es).

- Aprenda a [crear un conjunto de medios mixtos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html?lang=es#creating-a-mixed-media-set).

Para mostrar una imagen para zoom, un conjunto o un vídeo en el sitio web, lo llama en un &quot;visualizador&quot; de Dynamic Media Classic. Dynamic Media Classic incluye visores para recursos de medios enriquecidos como conjuntos de muestras, conjuntos de giros, vídeo y muchos otros.

Más información sobre [Visores para AEM Assets y Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=es).

## Valores preestablecidos de conjunto por lotes

Hasta ahora hemos estado hablando de cómo construir conjuntos manualmente usando la función Build de Dynamic Media Classic. Sin embargo, es posible automatizar la creación de conjuntos de imágenes y conjuntos de giros mediante un ajuste preestablecido de conjunto por lotes, siempre y cuando tenga una convención de nombres estandarizada.

Cada ajuste preestablecido es un conjunto de instrucciones independientes con un nombre único que define cómo construir el conjunto mediante imágenes que coinciden con las convenciones de nomenclatura definidas. En el ajuste preestablecido, primero debe definir las convenciones de nomenclatura de los recursos que desea agrupar en un conjunto. A continuación, se puede crear un ajuste preestablecido de conjunto por lotes para hacer referencia a estas imágenes.

Aunque es posible crear el ajuste preestablecido usted mismo (se encuentran en **Configuración > Configuración de aplicación > Ajustes preestablecidos de conjunto de lotes** ), se recomienda que pida a su equipo de consultoría o a su equipo de soporte técnico que lo configuren. He aquí la razón:

- La configuración de los ajustes preestablecidos de conjuntos de lotes puede ser compleja: utilizan expresiones regulares y, a menos que sea un desarrollador, esta sintaxis puede resultar desconocida o confusa.
- Una vez creados, se activan de forma predeterminada. No hay función &quot;deshacer&quot;. Si empieza a cargar miles de imágenes y el ajuste preestablecido no está configurado correctamente, puede acabar teniendo cientos o miles de conjuntos rotos que debe buscar y eliminar manualmente.

Anteriormente se sugirió una convención de nombres sencilla que sería muy fácil de incorporar en un ajuste preestablecido de conjunto de lotes. Sin embargo, como los ajustes preestablecidos son muy flexibles, pueden gestionar estrategias de nomenclatura complejas. En resumen, las imágenes que pertenecen a un conjunto deben estar unidas por algún nombre común; a menudo es el número SKU o el ID del producto. En Dynamic Media Classic, puede definir una convención de nombres predeterminada para todas las imágenes que se van a utilizar en un ajuste preestablecido o puede crear varios ajustes preestablecidos, cada uno con reglas de nombres diferentes.

Los ajustes preestablecidos de conjunto de lotes solo se aplican durante la carga; no se pueden ejecutar después de cargar las imágenes. Por lo tanto, es importante planificar la convención de nombres y crear un ajuste preestablecido antes de empezar a cargar todas las imágenes.

Una vez creados los ajustes preestablecidos, el administrador de la empresa puede elegir si están activos o inactivos. Activo significa que aparecerán en la página de carga en **Opciones de trabajo**, mientras que los ajustes preestablecidos inactivos permanecerán ocultos.

Aprenda a [Crear un ajuste preestablecido de conjunto de lotes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=es#creating-a-batch-set-preset).

### Uso de ajustes preestablecidos de conjunto por lotes al cargar

Así se utilizan los ajustes preestablecidos de conjunto de lotes en la carga después de crearlos:

1. Haga clic en **Cargar** y elija **Desde el escritorio** o **Mediante FTP**.
2. Haga clic en **Opciones de trabajo**.
3. Abra la opción **Ajustes preestablecidos por lotes** y marque o desmarque el ajuste preestablecido para utilizarlo con la carga.
4. Una vez completada la carga, busque en la carpeta los conjuntos finalizados.

Más información sobre [Ajustes preestablecidos por lotes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=es#batch-set-presets).
