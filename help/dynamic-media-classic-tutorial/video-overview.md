---
title: Información general del vídeo
description: Dynamic Media Classic incluye conversión automática de vídeo durante la carga, transmisión de vídeo a dispositivos de escritorio y móviles, y conjuntos de vídeos adaptables optimizados para la reproducción en función del dispositivo y el ancho de banda. Obtenga más información sobre vídeo en Dynamic Media Classic y obtenga un manual sobre conceptos y terminología de vídeo. A continuación, descubra en profundidad cómo cargar y codificar vídeo, elija ajustes preestablecidos de vídeo para cargarlo, añada o edite un ajuste preestablecido de vídeo, previsualice los vídeos en un visor de vídeo, implemente vídeos en sitios web y móviles, añada subtítulos y marcadores de capítulo a vídeo y publique visualizadores de vídeo para usuarios de escritorio y móviles.
sub-product: dynamic-media
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
doc-type: tutorial
topics: development, authoring, configuring, videos, video-profiles
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '6234'
ht-degree: 0%

---


# Información general de vídeo {#video-overview}

Dynamic Media Classic incluye conversión automática de vídeo durante la carga, transmisión de vídeo a dispositivos de escritorio y móviles, y conjuntos de vídeos adaptables optimizados para la reproducción en función del dispositivo y el ancho de banda. Una de las cosas más importantes sobre el vídeo es que el flujo de trabajo es sencillo, está diseñado para que cualquiera pueda utilizarlo, aunque no esté muy familiarizado con la tecnología de vídeo.

Al final de esta sección del tutorial, sabrá cómo:

- Cargar y codificar vídeo (transcodificar) a diferentes tamaños y formatos
- Elija entre los ajustes preestablecidos de vídeo disponibles para cargar
- Añadir o editar un ajuste preestablecido de codificación de vídeo
- Vista previa de vídeos en un visor de vídeo
- Implementación de vídeo en sitios web y móviles
- Agregar subtítulos y marcadores de capítulo a vídeo
- Personalización y publicación de visores de vídeo para usuarios de escritorio y móviles

>[!NOTE]
>
>Todas las direcciones URL de este capítulo tienen fines ilustrativos únicamente; no son vínculos en directo.

## Información general sobre el vídeo de Dynamic Media Classic

Primero vamos a tener un mejor sentido de las posibilidades de vídeo con Dynamic Media Classic.

### Funciones y capacidades

La plataforma de vídeo de Dynamic Media Classic ofrece todas las partes de la solución de vídeo: la carga, la conversión y la administración de vídeos; la capacidad de agregar subtítulos y marcadores de capítulo a un vídeo; y la capacidad de utilizar ajustes preestablecidos para una reproducción más sencilla.

Facilita la publicación de vídeos adaptables de alta calidad para su transmisión en varias pantallas, incluidos dispositivos móviles de escritorio, iOS, Android, Blackberry y Windows. Un conjunto de vídeos adaptables agrupa versiones del mismo vídeo codificadas a diferentes velocidades de bits y formatos, como 400 kbps, 800 kbps y 1000 kbps. El equipo de escritorio o dispositivo móvil detecta el ancho de banda disponible.

Además, la calidad del vídeo se cambia automáticamente si las condiciones de red cambian en el escritorio o en el dispositivo móvil. Además, si un cliente entra en el modo de pantalla completa en un escritorio, el conjunto de vídeos adaptables responde mediante una mejor resolución, lo que mejora la experiencia de visualización del cliente. El uso de conjuntos de vídeos adaptables le ofrece la mejor reproducción posible para los clientes que reproduzcan vídeo de Dynamic Media Classic en varias pantallas y dispositivos.

### Administración de vídeo

Trabajar con vídeo puede ser más complejo que trabajar con imágenes digitales fijas. Con el vídeo, usted trata con numerosos formatos y estándares y la incertidumbre de si su audiencia podrá reproducir sus clips. Dynamic Media Classic facilita trabajar con vídeo, ya que proporciona muchas herramientas potentes &quot;bajo el capó&quot;, pero elimina la complejidad de trabajar con ellas.

Dynamic Media Classic reconoce y puede trabajar con muchos formatos de origen disponibles. Sin embargo, leer el vídeo es solo una parte del esfuerzo: también debe convertir el vídeo a un formato fácil de usar en la web. Dynamic Media Classic se encarga de esto permitiéndole convertir vídeo a vídeo H.264.

Conversión del vídeo usted mismo puede ser muy complicado utilizando las muchas herramientas profesionales y entusiastas disponibles. Dynamic Media Classic lo mantiene sencillo al ofrecer ajustes preestablecidos fáciles que están optimizados para diferentes configuraciones de calidad. Sin embargo, si desea algo más personalizado, también puede crear sus propios ajustes preestablecidos.

Si tiene mucho vídeo, apreciará la capacidad de administrar todos sus recursos junto con sus imágenes y otros medios en Dynamic Media Classic. Puede organizar, catalogar y buscar sus recursos, incluidos los recursos de vídeo, con compatibilidad sólida con metadatos XMP.

### Reproducción de vídeo

De forma similar al problema de la conversión de vídeo para que sea accesible y fácil de usar, el problema es implementar e implementar vídeo en el sitio. Elegir si comprar un reproductor o crear uno propio, haciéndolo compatible con varios dispositivos y pantallas, y luego mantener los reproductores puede ser una ocupación a tiempo completo.

De nuevo, el enfoque de Dynamic Media Classic es permitirle elegir el ajuste preestablecido y el visor que se adapte a sus necesidades. Tiene muchas opciones de visor diferentes y una biblioteca de numerosos ajustes preestablecidos disponibles.

Puede entregar vídeo fácilmente a la web y a los dispositivos móviles, ya que Dynamic Media Classic admite vídeo HTML5, lo que significa que puede dirigirse a usuarios que ejecuten varios navegadores, así como a usuarios de plataformas Android e iOS. El vídeo de flujo continuo permite una reproducción fluida de contenido más largo o de alta definición, mientras que el vídeo HTML5 progresivo tiene ajustes preestablecidos optimizados para la pantalla pequeña.

Los ajustes preestablecidos de visor para vídeo se pueden configurar parcialmente en función del tipo de visor.

Al igual que todos los visualizadores, la integración se realiza a través de una sola URL de Dynamic Media Classic por visualizador o vídeo.

>[!NOTE]
>
>Como práctica recomendada, utilice los visores de vídeo HTML5 de Dynamic Media Classic. Los ajustes preestablecidos utilizados en los visores de vídeo HTML5 son reproductores de vídeo sólidos. Al combinar en un solo reproductor la capacidad de diseñar los componentes de reproducción mediante HTML5 y CSS, tener reproducción incrustada y utilizar flujo adaptable y progresivo en función de la capacidad del explorador, puede ampliar el alcance del contenido de medios enriquecidos a los usuarios de escritorio, tableta y móvil, y garantizar una experiencia de vídeo optimizada.

Una última nota sobre el vídeo de Dynamic Media Classic que puede aplicarse a algunos clientes: es posible que no todas las empresas tengan la conversión automática, el flujo continuo o los ajustes preestablecidos de vídeo habilitados para su cuenta. Si, por alguna razón, no puede acceder a las direcciones URL para la transmisión de vídeo, este puede ser el motivo. Todavía podrá cargar y publicar vídeo descargado progresivamente y tener acceso a todos los visualizadores de vídeo. Sin embargo, para aprovechar todas las funciones de vídeo de Dynamic Media Classic, debe ponerse en contacto con el administrador de su cuenta o el administrador de ventas para habilitarlas.

Obtenga más información sobre [Vídeo en Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/quick-start-video.html).

## Vídeo 101

### Conceptos básicos de vídeo y terminología

Antes de comenzar, hablemos de algunos términos con los que debería estar familiarizado para trabajar con vídeo. Estos conceptos no son específicos de Dynamic Media Classic, y si va a gestionar vídeo para un sitio web profesional, le recomendamos que siga estudiando el tema. Recomendaremos algunos recursos al final de esta sección.

- **Codificación/transcodificación.** La codificación es el proceso de aplicación de la compresión de vídeo para convertir datos de vídeo sin comprimir y sin procesar en un formato que facilita el trabajo con ellos. La transcodificación, aunque similar, hace referencia a la conversión de un método de codificación a otro.

   - Los archivos de vídeo maestro creados con software de edición de vídeo suelen ser demasiado grandes y no tienen el formato adecuado para su envío a destinos en línea. Generalmente se codifican para una reproducción rápida en el escritorio y para la edición, pero no para la entrega a través de la web.
   - Para convertir el vídeo digital al formato y las especificaciones adecuados para la reproducción en distintas pantallas, los archivos de vídeo se transcodifican a un tamaño de archivo más pequeño y eficaz, óptimo para la entrega a la web y a dispositivos móviles.

- **Compresión de vídeo.** La reducción de la cantidad de datos utilizados para representar imágenes de vídeo digital es una combinación de compresión de imágenes espaciales y compensación de movimiento temporal.

   - La mayoría de las técnicas de compresión presentan pérdidas, lo que significa que envían datos para obtener un tamaño más pequeño.
   - Por ejemplo, el vídeo DV se comprime relativamente poco y permite editar fácilmente el material de origen, aunque es demasiado grande para utilizarlo en la web o incluso para colocarlo en un DVD.

- **Formatos de archivo.** El formato es un contenedor, similar a un archivo ZIP, que determina cómo se organizan los archivos en el archivo de vídeo, pero normalmente no cómo se codifican.

   - Los formatos de archivos comunes para el vídeo de origen incluyen Windows Media (WMV), QuickTime (MOV), Microsoft AVI y MPEG, entre otros. Los formatos publicados por Dynamic Media Classic son MP4.
   - Un archivo de vídeo generalmente contiene varias pistas (una pista de vídeo (sin audio) y una o más pistas de audio (sin vídeo)) que están interrelacionadas y sincronizadas.
   - El formato de archivo de vídeo determina cómo se organizan estas diferentes pistas de datos y metadatos.

- **Códec.** Un códec de vídeo describe el algoritmo mediante el cual se codifica un vídeo mediante el uso de compresión. El audio también se codifica mediante un códec de audio.

   - Los códecs minimizan la cantidad de información necesaria para reproducir vídeo. En lugar de información sobre cada fotograma individual, solo se almacena información sobre las diferencias entre un fotograma y el siguiente.
   - Debido a que la mayoría de los vídeos cambian poco de un fotograma a otro, los códecs permiten tasas de compresión altas, lo que da como resultado tamaños de archivo más pequeños.
   - Un reproductor de vídeo descodifica el vídeo según su códec y, a continuación, muestra una serie de imágenes o fotogramas en la pantalla.
   - Los códecs de vídeo comunes incluyen H.264, On2 VP6 y H.263.

![image](assets/video-overview/bird-video.png)

- **Resolución.** Altura y anchura del vídeo en píxeles.

   - El tamaño del vídeo de origen viene determinado por la cámara y la salida del software de edición. Una cámara HD normalmente creará vídeo de alta resolución 1920 x 1080, sin embargo, para reproducirlo sin problemas en la web, reduciría la muestra (redimensionaría) a una resolución más pequeña, como 1280 x 720, 640 x 480, o más pequeña.
   - La resolución tiene un impacto directo en el tamaño del archivo, así como en el ancho de banda necesario para reproducir ese vídeo.

- **Mostrar relación de aspecto.** Proporción de la anchura de un vídeo respecto a la altura de un vídeo. Cuando la proporción de aspecto del vídeo no coincide con la proporción del reproductor, es posible que vea &quot;barras negras&quot; o espacio vacío. Dos proporciones de aspecto comunes utilizadas para mostrar el vídeo son:

   - 4:3 (1.33:1). Se utiliza para casi todo el contenido de emisión de TV de definición estándar.
   - 16:9 (1.78:1). Se utiliza para casi todos los contenidos de TV de pantalla ancha y alta definición (HDTV) y películas.

- **Velocidad de bits/velocidad de datos.** Cantidad de datos codificados para formar un solo segundo de reproducción de vídeo (en kilobits por segundo).

   - Por lo general, cuanto menor sea la velocidad de bits, más deseable será para la web porque se puede descargar más rápido. Sin embargo, también puede significar que la calidad es baja debido a la pérdida de compresión.
   - Un buen códec debería equilibrar la velocidad de bits baja con la calidad correcta.

- **Velocidad de fotogramas (fotogramas por segundo o FPS).** Número de fotogramas, o imágenes fijas, para cada segundo de vídeo. Normalmente, la televisión norteamericana (NTSC) se emite en 29,97 FPS; La televisión europea y asiática (PAL) se emite en 25 FPS; y las películas (analógicas y digitales) suelen estar en 24 (23,976) FPS.

   - Para hacer las cosas más confusas, también hay marcos progresivos e interconectados. Cada marco progresivo contiene un marco de imagen completo, mientras que los marcos entrelazados contienen cada dos filas de píxeles en un marco de imagen. A continuación, los fotogramas se reproducen muy rápidamente y parecen mezclarse. La película utiliza un método de exploración progresiva, mientras que el vídeo digital suele estar entrelazado.
   - En general, no importa si el material de archivo de origen está entrelazado o no: Dynamic Media Classic conservará el método de digitalización en el vídeo convertido.
   - Entrega progresiva/de flujo continuo. La transmisión de vídeo es la entrega de contenido en un flujo continuo que se puede reproducir a medida que llega, mientras que el vídeo descargado progresivamente se descarga como cualquier otro archivo de un servidor y se almacena en caché localmente en el explorador.

Con suerte, este manual le ayudará a comprender las distintas opciones implicadas en el uso del vídeo de Dynamic Media Classic.

## Flujo de trabajo de vídeo

Cuando se trabaja con vídeo en Dynamic Media Classic, se sigue un flujo de trabajo básico similar al trabajo con imágenes.

![image](assets/video-overview/video-overview-2.png)

1. Comience por cargar archivos de vídeo en Dynamic Media Classic. Para ello, abra el **menú Herramientas** en la parte inferior del panel de extensiones de Dynamic Media Classic y elija **Cargar a Dynamic Media Classic > Archivos en el nombre de la carpeta** o **Cargar a Dynamic Media Classic > Carpetas en el nombre de la carpeta**. &quot;Nombre de carpeta&quot; es la carpeta que esté explorando con la extensión. Los archivos de vídeo pueden ser grandes, por lo que se recomienda utilizar FTP para cargar archivos de gran tamaño. Como parte de la carga, elija uno o varios ajustes preestablecidos de vídeo para codificar los vídeos. El vídeo se puede transcodificar a vídeo MP4 durante la carga. Consulte el tema Ajustes preestablecidos de vídeo para obtener más información sobre el uso y la creación de ajustes preestablecidos de codificación. Obtenga más información sobre [Carga y codificación de vídeos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. Seleccione o seleccione y modifique un ajuste preestablecido de visualizador de vídeo y previsualice el vídeo. Puede elegir un ajuste preestablecido de visualizador creado previamente o personalizar el suyo propio. Si está segmentando usuarios móviles, no tiene que hacer nada aquí porque las plataformas móviles no requieren un visor ni un ajuste preestablecido. Obtenga más información sobre [Vista previa de vídeos en un visualizador de vídeo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) y [Adición o edición de un ajuste preestablecido de visualizador de vídeo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. Ejecute una publicación de vídeo, obtenga la URL e integre. La diferencia principal entre este paso para el flujo de trabajo de vídeo y el flujo de trabajo de imagen es que se ejecutará una publicación de vídeo especial en lugar de (o tal vez así como) la publicación estándar de servicio de imágenes. La integración del visualizador de vídeo en el escritorio funciona exactamente igual que la integración del visualizador de imágenes, pero para los dispositivos móviles es aún más simple: todo lo que necesita es la URL del propio vídeo.

### Acerca de la transcodificación

La transcodificación se definió anteriormente como el proceso de conversión de un método de codificación a otro. En el caso de Dynamic Media Classic, es el proceso de convertir el vídeo de origen de su formato actual a MP4. Esto es necesario antes de que el vídeo aparezca en el navegador de escritorio o en un dispositivo móvil.

Dynamic Media Classic puede manejar toda la transcodificación por usted, una gran ventaja. Puede transcodificar el vídeo usted mismo y cargar los archivos ya convertidos a MP4, pero puede ser un proceso complejo que requiere software sofisticado. A menos que sepa lo que está haciendo, normalmente no obtendrá buenos resultados en su primer intento.

Dynamic Media Classic no solo convierte los archivos por usted, sino que también facilita el uso de los ajustes preestablecidos. Realmente no necesitan saber mucho sobre el lado técnico de este proceso — todo lo que deben saber es aproximadamente el tamaño final que quieren sacar del sistema y una sensación del ancho de banda que tienen los usuarios finales.

Aunque los ajustes preestablecidos son prácticos y cubren la mayoría de las necesidades, a veces desea algo más personalizado. En ese caso, puede crear su propio ajuste preestablecido de codificación. En Dynamic Media Classic, un ajuste preestablecido de codificación se denomina ajuste preestablecido de vídeo. Esto se explicará más adelante en este capítulo.

### Acerca de la transmisión

Otra característica importante que vale la pena destacar es la transmisión de vídeo, una característica estándar de la plataforma de vídeo de Dynamic Media Classic. Los medios de transmisión se reciben constantemente por un usuario final y se presentan al mismo tiempo que se entregan. Esto es significativo y deseable por varias razones.

La transmisión suele requerir menos ancho de banda que la descarga progresiva, ya que solo se entrega la parte del vídeo que se ve. El servidor y los visores de flujo continuo de vídeo de Dynamic Media Classic utilizan la detección automática del ancho de banda para ofrecer el mejor flujo posible para la conexión a Internet de un usuario.

Con la transmisión, el vídeo comienza a reproducirse antes de lo que hace con otros métodos. También hace un uso más eficiente de los recursos de red porque solo las partes del vídeo que se ven se envían al cliente.

El otro método de envío es la descarga progresiva. En comparación con la transmisión de vídeo, hay una única ventaja consistente para la descarga progresiva: no necesita un servidor de transmisión para entregar el vídeo. Y aquí es donde viene Dynamic Media Classic — Dynamic Media Classic tiene un servidor de transmisión integrado en la plataforma, por lo que no necesita el inconveniente o el costo adicional de mantener este hardware dedicado.

El vídeo de descarga progresiva se puede servir desde cualquier servidor web normal. Aunque esto puede ser conveniente y potencialmente rentable, tenga en cuenta que las descargas progresivas tienen capacidades limitadas de búsqueda y navegación, y los usuarios pueden acceder y reutilizar su contenido. En algunas situaciones, como la reproducción tras cortafuegos de red muy estrictos, la entrega de flujo puede bloquearse; en estos casos, puede ser deseable volver a la entrega progresiva.

La descarga progresiva es una buena opción para aficionados o sitios web con requisitos de poco tráfico; si no les importa si su contenido se almacena en caché en el equipo de un usuario; si solo necesitan enviar vídeos de menor longitud (menos de 10 minutos); o si los visitantes no pueden recibir vídeo de flujo continuo por alguna razón.

Deberá transmitir el vídeo si necesita funciones avanzadas y control sobre la entrega de vídeo o si necesita mostrar vídeo a audiencias más grandes (por ejemplo, varios cientos de espectadores simultáneos), realizar un seguimiento del uso o las estadísticas de visualización de los informes, o si desea ofrecer a los espectadores la mejor experiencia de reproducción interactiva.

Por último, si le preocupa la seguridad de los medios en los problemas de propiedad intelectual o gestión de derechos, la transmisión por secuencias ofrece una entrega de vídeo más segura, ya que los medios no se guardan en la caché del cliente cuando se transmiten por secuencias.

## Ajustes preestablecidos de vídeo

Al cargar el vídeo, puede elegir entre uno o varios ajustes preestablecidos que contengan la configuración para convertir el vídeo maestro a un formato compatible con la web mediante la codificación. Los ajustes preestablecidos de vídeo tienen dos sabores: ajustes preestablecidos de vídeo adaptables y ajustes preestablecidos de codificación única.

Consulte [Ajustes preestablecidos de vídeo disponibles](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

Los ajustes preestablecidos de vídeo adaptables se activan de forma predeterminada, lo que significa que están disponibles para la codificación. Si desea utilizar un ajuste preestablecido de codificación única, su administrador deberá activarlo para que aparezca en la lista de ajustes preestablecidos de vídeo.

Obtenga información sobre cómo [activar o desactivar ajustes preestablecidos de vídeo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

Puede elegir uno de los muchos ajustes preestablecidos pregenerados que se incluyen con Dynamic Media Classic o puede crear el suyo propio; sin embargo, no hay ningún ajuste preestablecido seleccionado para la carga de forma predeterminada. En otras palabras, **si no selecciona un ajuste preestablecido de vídeo en la carga, el vídeo no se convertirá y es posible que no se pueda publicar**. Sin embargo, puede convertir el vídeo sin conexión y cargarlo y publicarlo correctamente. Los ajustes preestablecidos de vídeo solo son necesarios si desea que Dynamic Media Classic realice la conversión por usted.

Al cargar, seleccione un ajuste preestablecido de vídeo seleccionando **Opciones de vídeo** en el panel Opciones de trabajo. A continuación, elija si desea codificar para equipo, móvil o tableta.

- El equipo es para uso de escritorio. Aquí encontrará ajustes preestablecidos de mayor tamaño (como HD) que consumen más ancho de banda.
- Mobile y Tablet crean vídeo MP4 para dispositivos como iPhone y smartphones Android. La única diferencia entre Mobile y Tablet es que los ajustes preestablecidos de Tablet suelen tener un ancho de banda mayor, ya que se basan en el uso de WiFi. Los ajustes preestablecidos móviles están optimizados para un uso 3G más lento.

### Preguntas que debe hacerse antes de elegir un ajuste preestablecido

Al elegir un ajuste preestablecido, debe conocer su audiencia, así como el material de archivo de origen. ¿Qué sabes de tu cliente? ¿Cómo están viendo el vídeo — con un monitor de computadora o un dispositivo móvil?

¿Cuál es la resolución del vídeo? Si elige un ajuste preestablecido mayor que el original, puede obtener un vídeo borroso/pixelado. Está bien si el vídeo es más grande que el ajuste preestablecido, pero no elija un ajuste preestablecido más grande que el vídeo de origen.

¿Cuál es su relación de aspecto? Si ve barras negras alrededor del vídeo convertido, entonces elige la proporción de aspecto incorrecta. Dynamic Media Classic no puede detectar automáticamente estos ajustes porque primero tendría que examinar el archivo antes de la carga.

### Desglose de las opciones de vídeo

Los ajustes preestablecidos de vídeo determinan cómo se codificará el vídeo especificando estos ajustes. Si no está familiarizado con estos términos, consulte el tema Conceptos básicos de vídeo y terminología, más arriba.

![image](assets/video-overview/video-overview-3.jpg)

- **Proporción de aspecto.** Normalmente estándar 4:3 o pantalla ancha16:9.
- **Tamaño.** Es la misma que la resolución de visualización y se mide en píxeles. Esto está relacionado con la relación de aspecto. A una proporción de 16:9, un vídeo será de 432 x 240 píxeles, mientras que a 4:3 será de 320 x 240 píxeles.
- **FPS.** Las velocidades de fotogramas estándar son de 30, 25 o 24 fotogramas por segundo (fps), según el estándar de vídeo: NTSC, PAL o Film. Este ajuste no importa, ya que Dynamic Media Classic siempre usará la misma velocidad de fotogramas que el vídeo de origen.
- **Formato.** Esto será MP4.
- **Ancho de banda.** Esta es la velocidad de conexión deseada del usuario de destino. ¿Tienen una conexión rápida a Internet o una lenta? ¿Utilizan normalmente equipos de escritorio o dispositivos móviles? Esto también está relacionado con la resolución (tamaño), ya que cuanto más grande sea el vídeo, más ancho de banda requiere.

### Determinación de la velocidad de datos o la &quot;velocidad de bits&quot; del vídeo

El cálculo de la velocidad de bits del vídeo es uno de los factores menos conocidos para servir vídeo a la web, pero potencialmente el más importante, ya que afecta directamente a la experiencia del usuario. Si establece una tasa de bits demasiado alta, tendrá una alta calidad de vídeo, pero un rendimiento deficiente. Los usuarios con conexiones a Internet más lentas se verán obligados a esperar mientras el vídeo se detenga constantemente mientras se reproduce. Sin embargo, si la pones demasiado baja, la calidad se verá afectada. Dentro del ajuste preestablecido de vídeo, Dynamic Media Classic sugiere una serie de datos en función del ancho de banda objetivo. Ese es un buen lugar para empezar.

Sin embargo, si desea averiguarlo usted mismo, necesitará una calculadora de velocidad de bits. Se trata de una herramienta que suelen utilizar los profesionales y entusiastas del vídeo para estimar la cantidad de datos que cabrá en un determinado flujo o parte del medio (como un DVD).

## Creación de un ajuste preestablecido de vídeo personalizado

A veces, es posible que necesite un ajuste preestablecido de vídeo especial que no coincida con la configuración de los ajustes preestablecidos de vídeo de codificación integrados. Esto puede suceder si tiene un vídeo personalizado de un tamaño específico, como un vídeo creado a partir de un software de animación 3D o uno que se haya recortado de su tamaño original. Tal vez quiera experimentar con diferentes configuraciones de ancho de banda para ofrecer vídeo de mayor o menor calidad. En cualquier caso, deberá crear un ajuste preestablecido de vídeo de codificación única personalizado.

### Flujo de trabajo de ajustes preestablecidos de vídeo

1. Los ajustes preestablecidos de vídeo se encuentran en **Configuración > Configuración de la aplicación > Ajustes preestablecidos de vídeo**. Aquí encontrará una lista de todos los ajustes preestablecidos de codificación disponibles para su empresa.

   - Cada cuenta de vídeo de flujo continuo tiene decenas de ajustes preestablecidos preestablecidos y si crea sus propios ajustes preestablecidos personalizados, también los verá aquí.
   - Puede filtrar por tipo utilizando el menú desplegable. Los ajustes preestablecidos se dividen en Ordenador, Móvil y Tableta.
      ![image](assets/video-overview/video-overview-4.jpg)

2. La columna Activo le permite elegir si desea mostrar todos los ajustes preestablecidos en la carga o solo los que elija. Si se encuentra en EE. UU., es posible que desee desmarcar los ajustes preestablecidos PAL europeos y, si está en UK/ EMEA, desmarque los ajustes preestablecidos NTSC.
3. Haga clic en el botón **Add** para crear un ajuste preestablecido personalizado. Se abrirá el panel Agregar ajuste preestablecido de vídeo . El proceso aquí es similar a la creación de un ajuste preestablecido de imagen.
4. Primero, asígnele un **Nombre de ajuste preestablecido** para que aparezca en la lista de ajustes preestablecidos. En el ejemplo anterior, este ajuste preestablecido es para vídeos de tutoriales de captura de pantalla.
5. La **Descripción** es opcional, pero proporciona a los usuarios una información del objeto que describirá el propósito de este ajuste preestablecido.
6. El **Encode File Suffix** se añadirá al final del nombre del vídeo que está creando aquí. Recuerde que tendrá un vídeo maestro, así como este vídeo codificado, que es una derivación del maestro, y que no hay dos recursos en Dynamic Media Classic que puedan tener el mismo ID de recurso.
7. **Los** dispositivos de reproducción permiten elegir el formato de archivo de vídeo que desea (equipo, móvil o tableta). Recuerde que Mobile y Tablet producen el mismo formato MP4. Dynamic Media Classic solo necesita saber en qué categoría colocar el ajuste preestablecido; sin embargo, la diferencia teórica es que los ajustes preestablecidos de Tablet suelen ser para una conexión a Internet más rápida, ya que todos admiten WiFi.
8. **La** tasa de datos de Target es algo que tendrá que averiguar por su cuenta, aunque en la imagen siguiente verá un intervalo sugerido. También puede arrastrar el control deslizante al ancho de banda aproximado del destino. Para obtener una cifra más precisa, utilice una calculadora de velocidad de bits. Hay un poco de prueba y error involucrados.

   ![image](assets/video-overview/video-overview-5.jpg)

9. Establezca la **Proporción de aspecto** del archivo de origen. Esta configuración está directamente ligada al tamaño indicado a continuación. Si elige _Personalizado_, tendrá que introducir manualmente tanto la anchura como la altura.
10. Si elige una proporción de aspecto, establezca un valor para **Tamaño de resolución** y Dynamic Media Classic rellenará el otro valor automáticamente. Sin embargo, para una relación de aspecto personalizada, rellene ambos valores. Su tamaño debe estar en línea con la velocidad de datos. Si establece una tasa de datos muy baja y un tamaño grande, se espera que la calidad sea deficiente.
11. Haga clic en **Guardar** para guardar el ajuste preestablecido. A diferencia de todos los demás ajustes preestablecidos, no es necesario publicar en este momento, ya que los ajustes preestablecidos solo sirven para cargar archivos. Más tarde, tendrá que publicar los vídeos codificados, pero los ajustes preestablecidos solo son para uso interno de Dynamic Media Classic.
12. Para verificar que el ajuste preestablecido de vídeo está en la lista de carga, vaya a **Cargar**. Elija **Opciones de trabajo** y amplíe **Opciones de vídeo**. El ajuste preestablecido se incluirá en la categoría del dispositivo de reproducción elegido (Equipo, Móvil o Tablet).

Obtenga más información sobre [Adición o edición de un ajuste preestablecido de vídeo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## Agregar subtítulos al vídeo

En algunos casos, puede resultar útil añadir subtítulos al vídeo; por ejemplo, cuando necesite proporcionar el vídeo a los visualizadores en varios idiomas, pero no desee duplicar el audio en otro idioma o volver a grabar el vídeo en otros idiomas. Además, la adición de subtítulos proporciona mayor accesibilidad para quienes sufren deficiencias auditivas y utilizan subtítulos. Dynamic Media Classic facilita la adición de subtítulos a los vídeos.

Obtenga información sobre cómo [agregar subtítulos a vídeo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/adding-captions-video.html).

## Agregar marcadores de capítulo al vídeo

Para los vídeos de formato largo, es probable que los visitantes aprecien la capacidad y la comodidad que ofrece la navegación por el vídeo con marcadores de capítulo. Dynamic Media Classic proporciona la capacidad de agregar fácilmente marcadores de capítulo al vídeo.

Aprenda a [Agregar marcadores de capítulo a vídeo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/adding-chapter-markers-video.html).

## Temas de implementación de vídeo

### Publicar y copiar URL

El último paso en el flujo de trabajo de Dynamic Media Classic es publicar el contenido del vídeo. Sin embargo, el vídeo tiene su propio trabajo de publicación, denominado Video Server publish, que se encuentra en Advanced.

![image](assets/video-overview/video-overview-6.jpg)

Obtenga información sobre cómo [publicar su vídeo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

Una vez que ejecute una publicación de vídeo, podrá obtener una URL para acceder a sus vídeos y a cualquier ajuste preestablecido de visualizador de Dynamic Media Classic disponible en un navegador web. Sin embargo, si personaliza o crea su propio ajuste preestablecido de visualizador de vídeo, tendrá que ejecutar una publicación independiente del servidor de imágenes.

- Obtenga información sobre cómo [vincular una dirección URL a un sitio móvil o a un sitio web](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website).
- Aprenda a [Incrustar el visualizador de vídeo en una página web](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

También puede implementar el vídeo utilizando un reproductor de vídeo creado por terceros o personalizado.

Obtenga información sobre cómo [implementar vídeo usando un reproductor de vídeo de terceros](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player).

Además, si también desea utilizar las miniaturas de vídeo (la imagen extraída del vídeo), también deberá ejecutar una publicación del servidor de imágenes. Esto se debe a que la imagen en miniatura del vídeo reside en el servidor de imágenes, mientras que el propio vídeo está en el servidor de vídeo. Las miniaturas de vídeo se pueden usar en los resultados de búsqueda de vídeo, en las listas de reproducción de vídeo y pueden utilizarse como el &quot;fotograma de póster&quot; inicial que aparece en el visor de vídeo antes de que se reproduzca el vídeo.

Obtenga más información sobre [Uso de miniaturas de vídeo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### Selección y personalización de un ajuste preestablecido de visualizador

El proceso de selección y personalización de un ajuste preestablecido de visualizador es exactamente el mismo que el de las imágenes. Puede crear un nuevo ajuste preestablecido o modificar uno existente y guardarlo con un nuevo nombre, realizar ediciones y ejecutar una publicación de servicio de imágenes. Todos los ajustes preestablecidos de visor se publican en el servidor de imágenes, no solo los ajustes preestablecidos para imágenes, por lo que debe ejecutar una publicación de imagen para ver los ajustes preestablecidos nuevos o modificados.

>[!TIP]
>
>Ejecute una publicación de servicio de imágenes después de publicar el servidor de vídeo para publicar cualquier imagen en miniatura asociada a los vídeos.

## Optimización del motor de búsqueda de vídeo

La optimización de los motores de búsqueda (SEO) es el proceso para mejorar la visibilidad de un sitio web o una página web en los motores de búsqueda. Aunque los motores de búsqueda destacan en la recopilación de información sobre contenido basado en texto, no pueden adquirir adecuadamente información sobre vídeo a menos que se les proporcione esta información. Con Dynamic Media Classic Video SEO, puede usar metadatos para ofrecer a los motores de búsqueda descripciones de sus vídeos. La función Video SEO permite crear mapas de sitios de vídeo y fuentes mRSS.

- **Mapa del sitio del vídeo**. Informa a Google exactamente dónde y qué contenido del vídeo se encuentra en un sitio. Por lo tanto, los vídeos se pueden buscar completamente en Google. Por ejemplo, un mapa de sitio de vídeo puede especificar el tiempo de ejecución y las categorías de los vídeos.
- **fuente mRSS**. Utilizado por los editores de contenido para enviar archivos multimedia a Yahoo! Búsqueda de vídeo. Google es compatible con el protocolo de fuentes mRSS (Video Sitemap) y Media RSS (mRSS) para enviar información a los motores de búsqueda.

Al crear mapas del sitio de vídeo y fuentes mRSS, debe decidir qué campos de metadatos de los archivos de vídeo incluir. De este modo, los vídeos se describen a los motores de búsqueda de modo que estos puedan dirigir con mayor precisión el tráfico a los vídeos de su sitio web.

Una vez creada la fuente o el mapa del sitio, puede hacer que Dynamic Media Classic lo publique automáticamente, lo publique manualmente o simplemente genere un archivo que pueda editar más adelante. Además, Dynamic Media Classic puede generar y publicar automáticamente este archivo cada día.

Al final del proceso, enviará el archivo o la dirección URL a su motor de búsqueda. Esta tarea se realiza fuera de Dynamic Media Classic; sin embargo, lo discutiremos brevemente a continuación.

### Requisitos para los archivos Sitemap/mRSS

Para que Google y otros motores de búsqueda no rechacen sus archivos, deben tener el formato adecuado e incluir ciertos datos. Dynamic Media Classic generará un archivo con el formato correcto; sin embargo, si la información no está disponible para algunos de sus vídeos, no se incluirá en el archivo .

Los campos obligatorios son Página de aterrizaje (la dirección URL de la página que sirve el vídeo, no la dirección URL del propio vídeo), Título y Descripción. Cada vídeo debe tener una entrada para estos elementos o no se incluirá en el archivo generado. Los campos opcionales son Etiquetas y Categoría.

Existen otros dos campos obligatorios: URL de contenido, URL del recurso de vídeo y Miniatura, URL de una imagen en miniatura del vídeo; sin embargo, Dynamic Media Classic rellenará automáticamente esos valores.

El flujo de trabajo recomendado es incrustar estos datos en los vídeos antes de cargarlos mediante metadatos XMP, y Dynamic Media Classic los extraerá al cargarlos. Utilizaría una aplicación como Adobe Bridge (que se incluye en todas las aplicaciones de Adobe Creative Cloud) para rellenar los datos en campos de metadatos estándar.

Si sigue este método, no tendrá que introducir manualmente estos datos mediante Dynamic Media Classic. Sin embargo, también puede utilizar ajustes preestablecidos de metadatos en Dynamic Media Classic, como una forma rápida de introducir los mismos datos cada vez.

Para obtener más información sobre ese tema, consulte [Visualización, adición y exportación de metadatos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html).

![image](assets/video-overview/video-overview-7.jpg)

Una vez rellenados los metadatos, podrá verlos en la vista de detalles de ese recurso de vídeo. Las palabras clave también pueden estar presentes, pero se encuentran en la ficha Palabras clave .

- Obtenga más información sobre [Adición de palabras clave](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- Obtenga más información sobre [Video SEO](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- Obtenga más información sobre [Configuración para Video SEO](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### Configuración de Video SEO

La configuración de la SEO de vídeo comienza con la elección del tipo de formato que desea, el método de generación y los campos de metadatos que deben incluirse en el archivo.

1. Vaya a **Configuración > Configuración de la aplicación > Video SEO > Configuración**.
2. En el menú **Generation Mode**, elija un formato de archivo. El valor predeterminado es Desactivado, por lo que para habilitarlo, elija Mapa del sitio del vídeo, mRSS o Ambos.
3. Elija si desea generar automáticamente o manualmente. Para simplificar, le recomendamos que lo establezca en **Modo automático**. Si elige Automático, establezca también la opción **Marcar para publicación**; de lo contrario, los archivos no se activarán. Los archivos Sitemap y RSS son tipos de documento XML y deben publicarse como cualquier otro recurso. Utilice uno de los modos manuales si no tiene toda la información lista ahora, o solo desea realizar una generación única.
4. Rellene las etiquetas de metadatos que se utilizarán en los archivos. Este paso no es opcional. Como mínimo, debe incluir los tres campos marcados con un asterisco (\*): **Página de aterrizaje** , **Título** y **Descripción**. Para utilizar los metadatos para estas tareas, arrastre y suelte los campos del panel Metadatos a la derecha en un campo correspondiente del formulario. Dynamic Media Classic rellenará automáticamente el campo de marcador de posición con los datos reales de cada vídeo. No es necesario utilizar campos de metadatos. En su lugar, puede escribir texto estático aquí, pero ese mismo texto aparecerá para cada vídeo.
5. Una vez que haya introducido la información en los tres campos obligatorios, Dynamic Media Classic habilitará los botones **Guardar** y **Guardar y generar**. Haga clic en uno para guardar la configuración. Utilice **Guardar** si está en modo automático y desea que Dynamic Media Classic genere los archivos más adelante. Utilice **Guardar y generar** para crear el archivo inmediatamente.

### Prueba y publicación del mapa del sitio del vídeo, la fuente mRSS o ambos archivos

Los archivos generados aparecerán en el directorio raíz (base) de su cuenta.

![image](assets/video-overview/video-overview-8.jpg)

Estos archivos deben publicarse, ya que la herramienta Video SEO no puede ejecutar una publicación por sí sola. Siempre que estén marcados para publicación, se enviarán a los servidores de publicación la próxima vez que se ejecute una publicación.

Después de la publicación, los archivos estarán disponibles con este formato de URL.

![image](assets/video-overview/video-url-format.png)

Ejemplo:

![image](assets/video-overview/video-format-example.png)

### Envío a los motores de búsqueda

El paso final del proceso es enviar los archivos o las direcciones URL a los motores de búsqueda. Dynamic Media Classic no puede realizar este paso por usted; sin embargo, suponiendo que envíe la dirección URL y no el archivo XML en sí, la fuente debe actualizarse la próxima vez que se genere el archivo y se produzca una publicación.

El método para enviar al motor de búsqueda variará, sin embargo, para Google se usan las herramientas de Google Webmaster. Una vez allí, vaya a **Configuración del sitio > Mapas del sitio** y haga clic en el botón **Enviar un mapa del sitio**. Aquí puede colocar la URL de Dynamic Media Classic en sus archivos SEO.

### Informe de SEO de vídeo

Dynamic Media Classic proporciona un informe para mostrarle cuántos vídeos se incluyeron correctamente en los archivos y, lo que es más importante, que no se incluyeron debido a errores. Para acceder al informe, vaya a **Configuración > Configuración de la aplicación > Video SEO > Informe**.

![image](assets/video-overview/video-overview-9.jpg)

## Implementación móvil para vídeo MP4

Dynamic Media Classic no incluye ajustes preestablecidos de visor para dispositivos móviles porque los visores no son necesarios para reproducir vídeo en dispositivos móviles compatibles. Siempre y cuando codifique al formato H.264 MP4 (mediante la conversión en la carga o la precodificación en el escritorio), los tabletas y smartphones compatibles podrán reproducir sus vídeos sin necesidad de un visor. Esto es compatible con dispositivos Android e iOS (iPhone y iPad).

La razón por la que no se requiere ningún visor es que ambas plataformas tienen compatibilidad nativa con H.264. Puede incrustar el vídeo en una página web HTML5 o incrustarlo en la propia aplicación, y los sistemas operativos Android e iOS proporcionarán un controlador para reproducir el vídeo.

Por este motivo, Dynamic Media Classic no le proporciona una URL a un visor para dispositivos móviles, sino que le proporciona una URL directamente al vídeo. En la ventana Vista previa de un vídeo MP4, habrá vínculos para escritorio y móvil. La URL móvil apunta al vídeo publicado.

Una cuestión importante a tener en cuenta sobre los vídeos publicados es que la URL enumera la ruta completa al vídeo, no solo el ID del recurso. Al tratar con imágenes, la imagen se llama por su ID de recurso, independientemente de la estructura de carpetas. Sin embargo, para el vídeo, también debe especificar la estructura de carpetas. En las direcciones URL anteriores, el vídeo se almacena en la ruta:

![image](assets/video-overview/mobile-implement-1.png)

Esto también se puede expresar como nombre de empresa/ruta de carpeta/nombre del vídeo.

### Método 1: Reproducción del explorador: código HTML5

Para incrustar el vídeo MP4 en una página web, utilice la etiqueta de vídeo HTML5.

![image](assets/video-overview/browser-playback.png)

Este método también funcionará para la web de escritorio, aunque puede tener problemas con la compatibilidad con el explorador; no todos los exploradores web de escritorio admiten de forma nativa el vídeo H.264, incluido Firefox.

### Método 2: Reproducción de aplicaciones en iOS: Media Player Framework

También puede incrustar el vídeo de Dynamic Media Classic MP4 en su código de aplicación móvil. Este es un ejemplo genérico para iOS que utiliza el marco del Reproductor de medios que se proporciona únicamente con fines ilustrativos:

![image](assets/video-overview/app-playback.png)

## Recursos adicionales

Vea el [Generador de habilidades de Dynamic Media: Vídeo en un seminario web bajo demanda de Dynamic Media Classic](https://seminars.adobeconnect.com/p2ueiaswkuze) para aprender a utilizar las funciones de vídeo de Dynamic Media Classic.
