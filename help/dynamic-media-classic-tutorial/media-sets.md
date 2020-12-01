---
title: Conjuntos de medios mixtos, de imágenes, muestras, giros y muestras
description: Una de las capacidades más útiles y poderosas de Dynamic Media Classic es su compatibilidad con la creación de conjuntos de medios enriquecidos como imágenes, muestras, giros y conjuntos de medios mixtos. Descubra qué es cada conjunto de medios enriquecidos y cómo crear cada tipo en Dynamic Media Classic. A continuación, obtenga más información sobre los ajustes preestablecidos de conjunto por lotes, que automatizan el proceso de creación de conjuntos de medios enriquecidos tras la carga.
sub-product: Dynamic-media
feature: sets
doc-type: tutorial
topics: sets, development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: e7a02900b0582fe9b329e5f9bd568f3c54d8d63d
workflow-type: tm+mt
source-wordcount: '1456'
ht-degree: 1%

---


# Conjuntos de medios mixtos, de imágenes, muestras, giros {#media-sets}

Las colecciones de conjuntos de Dynamic Media Classic, que van más allá de las imágenes únicas para aplicar un tamaño y un zoom dinámicos, permiten disfrutar de una experiencia en línea más rica. En esta sección del tutorial se explica cómo crear los siguientes conjuntos de medios enriquecidos en Dynamic Media Classic:

- Conjunto de imágenes
- Conjunto de muestras
- Conjunto de giros
- Conjunto de medios mixtos

También explicará cómo utilizar los ajustes preestablecidos de conjunto por lotes para automatizar la creación de conjuntos mediante una carga.

## Todo Lo Que Siempre Deseaba Saber Sobre Los Conjuntos

Junto al tamaño y zoom dinámicos básicos, los conjuntos son probablemente el subproducto de Dynamic Media Classic más utilizado. Los conjuntos son esencialmente recursos &quot;virtuales&quot; que no contienen imágenes reales, pero que consisten en un conjunto de relaciones con otras imágenes o vídeos. El principal atractivo de los juegos es que son miniaplicaciones listas para &quot;abrir y cerrar&quot;. Con esto queremos decir que cada visor de conjunto contiene su propia lógica e interfaz para que todo lo que tenga que hacer sea llamarlos al sitio. Además, solo requieren que realice un seguimiento de un único ID de recurso por conjunto, en lugar de tener que administrar usted mismo todos los recursos y relaciones de los miembros.

Cuando se crea un conjunto, ese conjunto se administra como un recurso independiente que debe marcarse para la publicación y publicarse antes de que se pueda servir desde una dirección URL. Todos los recursos de sus miembros también deben publicarse.

### Tipos de conjuntos

Conozca los cuatro tipos de conjuntos que puede crear en Dynamic Media Classic: Conjuntos de medios mixtos, imágenes, muestras, giros y muestras.

## Conjunto de imágenes

Este es el tipo de conjunto más común. Normalmente lo usará para vistas alternativas del mismo elemento. Consta de varias imágenes que se cargan en el visor haciendo clic en la miniatura asociada de esa imagen.

![image](assets/media-sets/image-set-1.jpg)

_Ejemplo de un conjunto de imágenes_

La dirección URL del conjunto de imágenes anterior podría aparecer como:

![image](assets/media-sets/image-set-url-1.png)

- Obtenga más información sobre los conjuntos de imágenes con el [Inicio rápido a los conjuntos de imágenes](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- Obtenga información sobre cómo [crear un conjunto de imágenes](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set).

### Conjunto de muestras

Este tipo de conjunto generalmente se utiliza para mostrar vistas de color del mismo elemento. Consta de pares de imágenes y muestras de color.

La diferencia principal entre una muestra y un conjunto de imágenes es que los conjuntos de muestras utilizan una imagen diferente como muestra en la que se puede hacer clic, mientras que los conjuntos de imágenes utilizan una versión en miniatura en miniatura en miniatura de la imagen original.

Los conjuntos de muestras no colorean las imágenes (idea errónea común). Las imágenes simplemente se intercambian, exactamente como en un conjunto de imágenes. Las miniimágenes de muestra se podrían haber creado con Photoshop, se podría haber fotografiado cada color por separado o se podría haber utilizado la herramienta Recortar en Dynamic Media Classic para crear una muestra de una de las imágenes de color.

![image](assets/media-sets/image-set-2.jpg)

_Ejemplo de un conjunto de muestras_

La dirección URL del conjunto de muestras anterior podría aparecer como:

![image](assets/media-sets/image-set_url.png)

- Obtenga más información sobre los conjuntos de muestras con el [Inicio rápido a los conjuntos de muestras](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- Obtenga información sobre cómo [crear un conjunto de muestras](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set).

### Conjunto de giros

Este conjunto suele utilizarse para mostrar una vista de 360 grados de un elemento. Al igual que los conjuntos de muestras, los conjuntos de giros no utilizan magia 3D. el verdadero trabajo es crear muchas fotos de una imagen desde todos lados. El visor simplemente le permite cambiar entre las imágenes, como una animación con movimiento de detención.

Los conjuntos de giros pueden girar en una dirección a lo largo de un solo eje o bien, si se crean alternativamente como un conjunto de giros 2D — girar en varios ejes. Por ejemplo, se puede girar un coche mientras todas las ruedas están sobre el suelo y luego se puede &quot;voltear&quot; hacia arriba y rotar también sobre sus ruedas traseras. Para un conjunto de giros 2D configurado correctamente, el número de imágenes por fila para cada eje debe ser el mismo. En otras palabras, si está girando en dos ejes, necesita el doble de imágenes que un giro de un solo ángulo.

![image](assets/media-sets/image-set-3.png)

_Ejemplo de un conjunto de giros_

La dirección URL del conjunto de giros anterior podría aparecer como:

![image](assets/media-sets/spin-set.png)

- Obtenga más información sobre los conjuntos de giros con el [Inicio rápido en los conjuntos de giros](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html).
- Descubra cómo [Crear un conjunto de giros](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set).

## Conjunto de medios mixtos

Este es un conjunto de combinaciones. Le permite combinar cualquiera de los conjuntos anteriores, así como añadir vídeo, en un único visor. En este flujo de trabajo, se crea primero cualquiera de los conjuntos de componentes y, a continuación, se ensamblan en un conjunto de medios mixtos.

![image](assets/media-sets/image-set-4.png)

_Ejemplo de un conjunto de medios mixtos_

La dirección URL del conjunto de medios mixtos anterior podría aparecer como:

![image](assets/media-sets/image-set-url-1.png)

- Obtenga más información sobre los conjuntos de medios mixtos con el [Inicio rápido a los conjuntos de medios mixtos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html).

- Obtenga información sobre cómo [crear un conjunto de medios mixtos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set).

Para mostrar una imagen para zoom, un conjunto o un vídeo en el sitio web, debe llamarla en un &quot;visor&quot; de Dynamic Media Classic. Dynamic Media Classic incluye visores para recursos de medios enriquecidos como conjuntos de muestras, conjuntos de giros, vídeos y muchos otros.

Obtenga más información sobre [Visores para AEM Assets y Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html).

## Valores preestablecidos de conjunto por lotes

Hasta ahora hemos estado discutiendo cómo crear conjuntos manualmente con la función de generación de Dynamic Media Classic. Sin embargo, es posible automatizar la creación de conjuntos de imágenes y conjuntos de giros mediante un ajuste preestablecido de conjunto de lotes siempre y cuando se disponga de una convención de nombres estandarizada.

Cada ajuste preestablecido tiene un nombre exclusivo y un conjunto de instrucciones independiente que define cómo construir el conjunto con imágenes que coinciden con las convenciones de nombres definidas. En el ajuste preestablecido, primero se definen las convenciones de nombre de los recursos que se desea agrupar en un conjunto. A continuación, se puede crear un ajuste preestablecido de conjunto por lotes para hacer referencia a estas imágenes.

Si bien es posible crear el ajuste preestablecido usted mismo (se encuentra en **Ajustes > Ajustes de aplicación > Valores preestablecidos de conjunto por lotes** ), como práctica recomendada debe configurar el equipo de consultoría o el servicio de asistencia técnica. He aquí por qué:

- Los ajustes preestablecidos de conjunto por lotes pueden ser complejos de configurar — son propulsados por expresiones regulares, y a menos que usted sea un desarrollador, esta sintaxis puede ser desconocida o confusa.
- Una vez creadas, se activan de forma predeterminada. No hay ninguna función &quot;deshacer&quot;. Si inicio cargar miles de imágenes y el ajuste preestablecido no está configurado correctamente, puede que termine con cientos o miles de conjuntos dañados que debe buscar y eliminar manualmente.

Antes se sugirió una convención de nombres sencilla que sería muy fácil de incorporar en un ajuste preestablecido de conjunto de lotes. Sin embargo, como los ajustes preestablecidos son muy flexibles, pueden manejar estrategias de nombres complejas. En resumen, las imágenes que pertenecen a un conjunto deberían estar unidas por algún nombre común — a menudo es el número de SKU o la ID del producto. En Dynamic Media Classic, puede indicarle una convención de nombres predeterminada para que todas las imágenes se utilicen en un ajuste preestablecido o puede crear varios ajustes preestablecidos, cada uno con distintas reglas de nombres.

Los ajustes preestablecidos de conjunto de lotes solo se aplican durante la carga; no se pueden ejecutar después de cargar las imágenes. Por lo tanto, es importante planificar la convención de nombres y crear un ajuste preestablecido antes de realizar el inicio de carga de todas las imágenes.

Una vez creados los ajustes preestablecidos, el administrador de Compañía puede elegir si están activos o inactivos. Activo significa que aparecerán en la página de carga en **Opciones de trabajo**, mientras que los ajustes preestablecidos inactivos permanecerán ocultos.

Obtenga información sobre cómo [crear un ajuste preestablecido de conjunto por lotes](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset).

### Uso de ajustes preestablecidos de conjunto por lotes al cargar

A continuación se muestra cómo se utilizan los ajustes preestablecidos de conjunto por lotes al cargarlos una vez creados:

1. Haga clic en **Cargar** y elija **Desde el escritorio** o **Por medio de FTP**.
2. Haga clic en **Opciones de trabajo**.
3. Abra la opción **Valores preestablecidos de conjunto por lotes** y marque o desmarque el ajuste preestablecido para utilizarlo con la carga.
4. Una vez finalizada la carga, busque los conjuntos terminados en la carpeta.

Obtenga más información sobre [Valores preestablecidos de conjunto por lotes](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
