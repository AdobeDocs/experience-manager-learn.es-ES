---
title: Ajustes preestablecidos de imagen
description: Los ajustes preestablecidos de imagen de Dynamic Media Classic contienen todos los ajustes necesarios para crear una imagen con un tamaño, formato, calidad y enfoque específicos. Los ajustes preestablecidos de imagen son un componente clave del tamaño dinámico. Al consultar una URL en Dynamic Media Classic, podrá ver fácilmente si se está utilizando un ajuste preestablecido de imagen. Obtenga información sobre los ajustes preestablecidos de imagen, por qué son tan útiles y cómo crearlos.
sub-product: Dynamic-media
feature: image-presets
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '705'
ht-degree: 1%

---


# Ajustes preestablecidos de imagen {#image-presets}

Un ajuste preestablecido de imagen es esencialmente una fórmula que contiene todos los ajustes necesarios para crear una imagen con un tamaño, formato, calidad y enfoque específicos. Los ajustes preestablecidos de imagen son un componente clave del tamaño dinámico.

Si observa las direcciones URL de casi cualquier cliente de Dynamic Media Classic, probablemente verá un ajuste preestablecido de imagen en uso. Busque $name$ al final de la dirección URL (con cualquier palabra o palabra sustituida por nombre).

Los ajustes preestablecidos de imagen abrevian la URL, por lo que en lugar de escribir varias instrucciones de servicio de imágenes por solicitud, puede escribir un solo ajuste preestablecido de imagen. Por ejemplo, estas dos direcciones URL producen la misma imagen JPEG de 300 x 300 con enfoque, pero la segunda utiliza un ajuste preestablecido de imagen:

![image](assets/image-presets/image-preset-2.png)

El verdadero valor de los ajustes preestablecidos de imagen es que cualquier administrador de Compañías puede actualizar la definición de ese ajuste preestablecido de imagen y afectar a todas las imágenes que utilicen ese formato: sin cambiar ningún código web. Verá los resultados de cualquier cambio en un ajuste preestablecido de imagen después de que se borre la caché de la URL.

>[!IMPORTANT]
>
>Al cambiar el tamaño de una imagen, la proporción de aspecto, la proporción del ancho de la imagen con respecto a su altura, siempre debe mantenerse proporcional para que la imagen no se distorsione.

Un ajuste preestablecido de imagen tiene un signo de dólar ($) en ambos lados del nombre y sigue el signo de interrogación (?) separador.

>[!TIP]
>
>Cree un ajuste preestablecido de imagen por tamaño de imagen único en el sitio. Por ejemplo, si necesita una imagen de 350 X 350 para la página de detalles del producto, una imagen de 120 X 120 para las páginas de búsqueda y búsqueda y una imagen de 90 X 90 para un artículo de venta cruzada/destacado, necesitará tres ajustes preestablecidos de imagen, ya sea que tenga 500 imágenes o 500.000.

- Obtenga más información sobre los [ajustes preestablecidos de imagen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Obtenga información sobre cómo [crear un ajuste preestablecido de imagen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Ajustes preestablecidos de imagen y enfoque

Los ajustes preestablecidos de imagen suelen cambiar el tamaño de una imagen y cada vez que se cambia el tamaño de una imagen a partir de su tamaño original, se debe agregar enfoque. Esto se debe a que el cambio de tamaño provoca que muchos píxeles se combinen y se fusionen en un espacio más pequeño, lo que hace que la imagen parezca suave y borrosa. El enfoque aumenta el contraste de los bordes y las áreas de alto contraste de una imagen.

Esperamos que las imágenes de alta resolución que cargue en Dynamic Media Classic no necesiten enfoque cuando se vean a tamaño completo. al acercar. Sin embargo, en cualquier tamaño más pequeño, normalmente es deseable un cierto enfoque.

>[!TIP]
>
>Siempre enfocar al cambiar el tamaño de las imágenes. Esto significa que tendrá que añadir enfoque a cada ajuste preestablecido de imagen (y ajuste preestablecido de visor, que analizaremos más adelante).
>
>Si tus imágenes no se ven bien, lo más probable es que se deba a que necesitan enfoque o, para empezar, la calidad era mala.

Cuánto enfoque agregar es totalmente subjetivo. A algunas personas les gustan las imágenes más suaves, mientras que a otras les gustan mucho. Es fácil mejorar una imagen mediante la ejecución de una combinación de filtros de enfoque en una imagen. Sin embargo, también es fácil sobrepasar y enfocar una imagen en exceso.

El siguiente gráfico muestra tres niveles de enfoque. De derecha a izquierda no hay enfoque, sólo la cantidad correcta y demasiado.

![image](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic permite tres tipos de enfoque: Enfoque simple, Modo de remuestreo y Máscara de enfoque.

Obtenga más información sobre [Opciones de enfoque de Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Recursos adicionales

[Guía](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf) de ajustes preestablecidos de imagen. Configuración que se utilizará para optimizar la calidad de imagen y la velocidad de carga.

[La Imagen Es Todo Parte 2: Nunca es solo un desenfoque. Calidad frente a velocidad](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Publicación de blog sobre el uso de ajustes preestablecidos de imagen para la distribución de imágenes de alta calidad y de carga rápida.

[La Imagen Es Todo Seminario Web](https://dynamicmediaseries2019.enterprise.adobeevents.com/). Vínculos a grabaciones de los tres seminarios web de la serie _Image Is Everything_. [En el seminario web 2 se ](https://seminars.adobeconnect.com/p6lqaotpjnd3) analizan los ajustes preestablecidos de imagen.
