---
title: Conjuntos de medios mixtos, imágenes, muestras y giros
description: Una de las capacidades más útiles y potentes de Dynamic Media Classic es su compatibilidad con la creación de conjuntos de medios enriquecidos, como imágenes, muestras, giros y conjuntos de medios mixtos. Descubra qué es cada conjunto de medios enriquecidos y cómo crear cada tipo en Dynamic Media Classic. A continuación, obtenga más información sobre los ajustes preestablecidos de conjuntos de lotes, que automatizan el proceso de creación de conjuntos de medios enriquecidos al cargarlos.
sub-product: dynamic-media
feature: Dynamic Media Classic, Conjuntos de imágenes, Conjuntos de medios mixtos, Conjuntos de giros
topic: Administración de contenido
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1446'
ht-degree: 1%

---


# Conjuntos de medios mixtos, imágenes, muestras y giros {#media-sets}

Al ir más allá de las imágenes únicas para el tamaño y el zoom dinámicos, las colecciones de conjuntos de Dynamic Media Classic permiten disfrutar de una mejor experiencia en línea. En esta sección del tutorial se explica cómo crear los siguientes conjuntos de medios enriquecidos en Dynamic Media Classic:

- Conjunto de imágenes
- Conjunto de muestras
- Conjunto de giros
- Conjunto de medios mixtos

También explicará cómo utilizar los ajustes preestablecidos de conjuntos de lotes para automatizar la creación de conjuntos mediante una carga.

## Todo Lo Que Siempre Desea Saber Sobre Los Conjuntos

Junto al tamaño dinámico básico y el zoom, los conjuntos son probablemente el subproducto de Dynamic Media Classic más utilizado. Los conjuntos son esencialmente recursos &quot;virtuales&quot; que no contienen imágenes reales, pero que consisten en un conjunto de relaciones con otras imágenes y/o vídeos. El principal atractivo de los juegos es que son miniaplicaciones que están listas &quot;fuera del estante&quot;. Con esto significamos que cada visualizador de conjunto contiene su propia lógica e interfaz, de modo que todo lo que tiene que hacer es llamarles en el sitio. Además, solo requieren que realice el seguimiento de un único ID de recurso por conjunto, en lugar de tener que administrar usted mismo todos los recursos y relaciones de los miembros.

Cuando crea un conjunto, ese conjunto se administra como un recurso independiente que debe marcarse para publicarse y publicarse antes de que se pueda servir desde una dirección URL. Todos los recursos de sus miembros también deben publicarse.

### Tipos de conjuntos

Obtenga información sobre los cuatro tipos de conjuntos que puede crear en Dynamic Media Classic: Conjuntos de medios mixtos, imágenes, muestras y giros.

## Conjunto de imágenes

Este es el tipo de conjunto más común. Normalmente lo usará para vistas alternativas del mismo elemento. Consta de varias imágenes que se cargan en el visor haciendo clic en la miniatura asociada de esa imagen.

![image](assets/media-sets/image-set-1.jpg)

_Ejemplo de conjunto de imágenes_

La dirección URL del conjunto de imágenes anterior podría aparecer como:

![image](assets/media-sets/image-set-url-1.png)

- Obtenga más información sobre los conjuntos de imágenes con [Quick Start to Image Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- Obtenga información sobre cómo [Crear un conjunto de imágenes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set).

### Conjunto de muestras

Este tipo de conjunto se utiliza generalmente para mostrar vistas de color del mismo elemento. Consta de pares de imágenes y muestras de color.

La diferencia principal entre una muestra y un conjunto de imágenes es que los conjuntos de muestras utilizan una imagen diferente como muestra en la que se puede hacer clic, mientras que los conjuntos de imágenes utilizan una versión en miniatura en miniatura de la imagen original en la que se puede hacer clic.

Los conjuntos de muestras no colorean las imágenes (idea errónea común). Las imágenes simplemente se están intercambiando, exactamente como en un conjunto de imágenes. Las imágenes de minimuestras podrían haberse creado utilizando Photoshop, cada color podría haberse fotografiado por separado o la herramienta Recortar en Dynamic Media Classic podría haberse utilizado para hacer una muestra de una de las imágenes coloreadas.

![image](assets/media-sets/image-set-2.jpg)

_Ejemplo de conjunto de muestras_

La dirección URL del conjunto de muestras anterior podría aparecer como:

![image](assets/media-sets/image-set_url.png)

- Obtenga más información sobre los conjuntos de muestras con los [conjuntos de muestras de inicio rápido](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- Obtenga información sobre cómo [Crear un conjunto de muestras](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set).

### Conjunto de giros

Este conjunto suele utilizarse para mostrar una vista de 360 grados de un elemento. Al igual que los conjuntos de muestras, los conjuntos de giros no utilizan magia 3D; el trabajo real es crear muchas fotos de una imagen desde todos lados. El visor simplemente le permite cambiar entre las imágenes como una animación de movimiento de parada.

Los conjuntos de giros pueden girar en una dirección a lo largo de un solo eje o si se crean alternativamente como un conjunto de giros 2D — girar en varios ejes. Por ejemplo, se puede girar un coche mientras todas las ruedas están sobre el suelo, y luego se puede &quot;girar&quot; hacia arriba y girar también sobre sus ruedas traseras. Para un conjunto de giros 2D configurado correctamente, el número de imágenes por fila para cada eje debe ser el mismo. En otras palabras, si está girando en dos ejes, necesita el doble de imágenes que un giro de un solo ángulo.

![image](assets/media-sets/image-set-3.png)

_Ejemplo de conjunto de giros_

La dirección URL del conjunto de giros anterior podría aparecer como:

![image](assets/media-sets/spin-set.png)

- Obtenga más información sobre los conjuntos de giros con los [Conjuntos de giros](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html) de inicio rápido.
- Obtenga información sobre cómo [Crear un conjunto de giros](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set).

## Conjunto de medios mixtos

Este es un conjunto de combinaciones. Le permite combinar cualquiera de los conjuntos anteriores, así como añadir vídeo, en un solo visor. En este flujo de trabajo, cree primero cualquiera de los conjuntos de componentes y, a continuación, agréguelos juntos en un conjunto de medios mixtos.

![image](assets/media-sets/image-set-4.png)

_Ejemplo de conjunto de medios mixtos_

La dirección URL del conjunto de medios mixtos anterior podría aparecer como:

![image](assets/media-sets/image-set-url-1.png)

- Obtenga más información sobre los conjuntos de medios mixtos con los [Conjuntos de medios mixtos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html) de inicio rápido.

- Obtenga información sobre cómo [Crear un conjunto de medios mixtos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set).

Para mostrar una imagen para zoom, un conjunto o un vídeo en su sitio web, la llama en un &quot;visor&quot; de Dynamic Media Classic. Dynamic Media Classic incluye visores para recursos de medios enriquecidos, como conjuntos de muestras, conjuntos de giros, vídeos y muchos otros.

Obtenga más información sobre [Visores para AEM Assets y Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html).

## Valores preestablecidos de conjunto por lotes

Hasta ahora hemos estado discutiendo cómo crear conjuntos manualmente con la función de compilación de Dynamic Media Classic. Sin embargo, es posible automatizar la creación de conjuntos de imágenes y conjuntos de giros mediante un ajuste preestablecido de conjunto de lotes siempre que tenga una convención de nombres estandarizada.

Cada ajuste preestablecido es un conjunto de instrucciones con un nombre único e independiente que define cómo construir el conjunto con imágenes que coinciden con las convenciones de nomenclatura definidas. En el ajuste preestablecido, primero se definen las convenciones de nomenclatura de los recursos que se desean agrupar en un conjunto. A continuación, se puede crear un ajuste preestablecido de conjunto de lotes para hacer referencia a estas imágenes.

Aunque es posible crear el ajuste preestablecido usted mismo (se encuentran en **Configuración > Configuración de la aplicación > Ajustes preestablecidos de conjuntos de lotes** ), como práctica recomendada debe configurar su equipo de consultoría o soporte técnico para usted. He aquí por qué:

- Los ajustes preestablecidos de conjuntos de lotes pueden ser complejos de configurar, funcionan con expresiones regulares y, a menos que sea un desarrollador, esta sintaxis puede resultar desconocida o confusa.
- Una vez creadas, se activan de forma predeterminada. No hay función &quot;deshacer&quot;. Si comienza a cargar miles de imágenes y el ajuste preestablecido está configurado incorrectamente, puede que termine con cientos o miles de conjuntos rotos que debe buscar y eliminar manualmente.

Anteriormente se sugirió una convención de nombres sencilla que sería muy fácil incorporar en un ajuste preestablecido de conjunto de lotes. Sin embargo, como los ajustes preestablecidos son muy flexibles, pueden gestionar estrategias de nomenclatura complejas. En resumen, las imágenes que pertenecen a un conjunto deben estar unidas por algún nombre común, a menudo es el número de SKU o el ID de producto. En Dynamic Media Classic, puede indicarle una convención de nombres predeterminada para que todas las imágenes se utilicen para un ajuste preestablecido, o bien puede crear varios ajustes preestablecidos, cada uno con distintas reglas de nombres.

Los ajustes preestablecidos de conjuntos de lotes solo se aplican al cargarlos; no se pueden ejecutar después de cargar las imágenes. Por lo tanto, es importante planificar la convención de nomenclatura y obtener un ajuste preestablecido antes de comenzar a cargar todas las imágenes.

Una vez creados los ajustes preestablecidos, el administrador de la empresa puede elegir si están activos o inactivos. Activo significa que aparecerán en la página de carga en **Opciones de trabajo**, mientras que los ajustes preestablecidos inactivos permanecerán ocultos.

Obtenga información sobre cómo [Crear un ajuste preestablecido de conjunto de lotes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset).

### Uso de ajustes preestablecidos de conjuntos de lotes al cargar

A continuación se indica cómo se utilizan los ajustes preestablecidos de conjuntos de lotes en la carga después de crearlos:

1. Haga clic en **Upload** y elija **From Desktop** o **Via FTP**.
2. Haga clic en **Opciones de trabajo**.
3. Abra la opción **Valores preestablecidos de conjunto de lotes** y marque o desmarque el ajuste preestablecido para utilizarlo con la carga.
4. Una vez finalizada la carga, busque en la carpeta los conjuntos finalizados.

Obtenga más información sobre [Ajustes preestablecidos de conjuntos de lotes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
