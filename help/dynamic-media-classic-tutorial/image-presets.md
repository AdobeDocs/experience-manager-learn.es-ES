---
title: Ajustes preestablecidos de imagen
description: Los ajustes preestablecidos de imagen en Dynamic Media Classic contienen todos los ajustes necesarios para crear una imagen con un tamaño, formato, calidad y enfoque específicos. Los ajustes preestablecidos de imagen son un componente clave del tamaño dinámico. Si observa una dirección URL en Dynamic Media Classic, puede ver fácilmente si se está utilizando un ajuste preestablecido de imagen. Obtenga información sobre los ajustes preestablecidos de imagen, por qué son tan útiles y cómo crearlos.
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
duration: 157
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 1%

---

# Ajustes preestablecidos de imagen {#image-presets}

Un ajuste preestablecido de imagen es esencialmente una fórmula que contiene todos los ajustes necesarios para crear una imagen con un tamaño, formato, calidad y enfoque específicos. Los ajustes preestablecidos de imagen son un componente clave del tamaño dinámico.

Si observa las direcciones URL de casi cualquier cliente de Dynamic Media Classic, probablemente verá un ajuste preestablecido de imagen en uso. Busque $name$ al final de la dirección URL (con cualquier palabra o palabras sustituidas por nombre).

Los ajustes preestablecidos de imagen acortan la dirección URL, por lo que, en lugar de escribir varias instrucciones de servicio de imágenes por solicitud, puede escribir un solo ajuste preestablecido de imagen. Por ejemplo, estas dos direcciones URL producen la misma imagen de JPEG de 300 x 300 con enfoque, pero la segunda utiliza un ajuste preestablecido de imagen:

![imagen](assets/image-presets/image-preset-2.png)

El valor verdadero de los ajustes preestablecidos de imagen es que cualquier administrador de la empresa puede actualizar la definición de ese ajuste preestablecido de imagen y afectar a cada imagen que utilice ese formato, sin cambiar ningún código web. Verá los resultados de cualquier cambio en un ajuste preestablecido de imagen después de que se borre la caché de la URL.

>[!IMPORTANT]
>
>Al cambiar el tamaño de una imagen, la proporción de aspecto, la proporción de anchura de la imagen respecto a su altura, siempre debe mantenerse proporcional para que la imagen no se distorsione.

Un ajuste preestablecido de imagen tiene un signo de dólar ($) a ambos lados del nombre y sigue al signo de interrogación (?) separador.

>[!TIP]
>
>Cree un ajuste preestablecido de imagen por cada tamaño de imagen único en el sitio. Por ejemplo, si necesita una imagen de 350 x 350 para la página de detalles del producto, una imagen de 120 x 120 para las páginas de exploración/búsqueda y una imagen de 90 x 90 para un artículo de venta cruzada/con funciones, necesitará tres ajustes preestablecidos de imagen, tanto si tiene 500 imágenes como 500 000.

- Más información sobre [Ajustes preestablecidos de imagen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Obtenga información sobre cómo [Crear un ajuste preestablecido de imagen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Ajustes preestablecidos y enfoque de imagen

Los ajustes preestablecidos de imagen suelen cambiar el tamaño de una imagen y, cada vez que cambia el tamaño de una imagen respecto a su tamaño original, debe agregar enfoque. Esto se debe a que el cambio de tamaño hace que muchos píxeles se fusionen y se fusionen en un espacio más pequeño, lo que hace que la imagen parezca suave y borrosa. El enfoque aumenta el contraste de los bordes y las áreas de alto contraste de una imagen.

Esperamos que las imágenes de alta resolución que cargue en Dynamic Media Classic no necesiten ningún enfoque cuando se vean a tamaño completo (cuando se amplíen). Sin embargo, en cualquier tamaño más pequeño, suele ser deseable un cierto enfoque.

>[!TIP]
>
>Enfoque siempre al cambiar el tamaño de las imágenes. Esto significa que tendrá que agregar enfoque a cada ajuste preestablecido de imagen (y ajuste preestablecido de visualizador, que analizaremos más adelante).
>
>Si sus imágenes no se ven bien, lo más probable es que sea porque necesitan enfoque o tal vez la calidad era mala para empezar.

Cuánto enfoque añadir es totalmente subjetivo. A algunas personas les gustan las imágenes más suaves, mientras que a otras les gustan muy nítidas. Es fácil mejorar una imagen ejecutando una combinación de filtros de enfoque en una imagen. Sin embargo, también es fácil exagerar y enfocar una imagen en exceso.

El siguiente gráfico muestra tres niveles de enfoque. De derecha a izquierda no tiene enfoque, solo la cantidad correcta, y demasiado.

![imagen](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic permite tres tipos de enfoque: enfoque simple, modo de remuestreo y máscara de enfoque.

Más información sobre [Opciones de enfoque de Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Recursos adicionales

[Guía de ajustes preestablecidos de imagen](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Configuración que se utilizará para optimizar la calidad de imagen y la velocidad de carga.

[La imagen es todo, parte 2: Nunca es solo un desenfoque: calidad frente a velocidad](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Una publicación de blog que habla sobre el uso de ajustes preestablecidos de imagen para ofrecer imágenes de alta calidad y carga rápida.
