---
title: Flujo de trabajo principal de Dynamic Media Classic y vista previa de recursos
description: 'Obtenga información acerca del flujo de trabajo principal en Dynamic Media Classic, que incluye los tres pasos: Crear (y cargar), Autor (y publicar) y Entregar. A continuación, aprenda a obtener una vista previa de los recursos en Dynamic Media Classic.'
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2703'
ht-degree: 1%

---

# Flujo de trabajo principal de Dynamic Media Classic y vista previa de recursos {#main-workflow}

Dynamic Media admite los procesos de flujo de trabajo Crear (y cargar), Crear (y publicar) y Enviar. Comience cargando recursos y luego haciendo algo con ellos, como crear un conjunto de imágenes y, finalmente, publicando para activarlos. El paso Generar es opcional para algunos flujos de trabajo. Por ejemplo, si su objetivo es realizar únicamente dimensionamiento dinámico y zoom en las imágenes o convertir y publicar vídeo para streaming, no hay pasos de compilación necesarios.

![imagen](assets/main-workflow/create-author-deliver.jpg)

El flujo de trabajo en las soluciones de Dynamic Media Classic consta de tres pasos principales:

1. Crear (y cargar) contenido de origen
2. Crear (y publicar) recursos
3. Entregar recursos

## Paso 1: Crear (y cargar)

Este es el principio del flujo de trabajo. En este paso, recopile o cree el contenido de origen que se ajuste al flujo de trabajo que está utilizando y cárguelo en Dynamic Media Classic. El sistema admite varios tipos de archivo para imágenes, vídeo y fuentes, pero también para PDF, Adobe Illustrator y Adobe InDesign.

Consulte la lista completa de [Tipos de archivo compatibles](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

Puede cargar el contenido de origen de varias formas diferentes:

- Directamente desde el escritorio o desde la red local. [Descubra cómo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- Desde un servidor FTP de Dynamic Media Classic. [Descubra cómo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

El modo predeterminado es Desde el escritorio, donde se buscan los archivos de la red local y se inicia la carga.

![imagen](assets/main-workflow/upload.jpg)

>[!TIP]
>
>No agregue manualmente las carpetas. En su lugar, ejecute una carga desde FTP y utilice el **Incluir subcarpetas** para volver a crear la estructura de carpetas dentro de Dynamic Media Classic.

Las dos opciones de carga más importantes están habilitadas de forma predeterminada: **Marcar para publicación**, del que ya hemos hablado anteriormente, y **Sobrescribir**. Sobrescribir significa que si el archivo que se está cargando tiene el mismo nombre que un archivo que ya se encuentra en el sistema, el nuevo archivo reemplazará la versión existente. Si desactiva esta opción, es posible que el archivo no se cargue.

### Sobrescribir opciones al cargar imágenes

Existen cuatro variaciones de la opción Sobrescribir imagen que se pueden configurar para toda la compañía y que a menudo se malinterpretan. En resumen, puede establecer las reglas de modo que desee que los recursos que tienen el mismo nombre se sobrescriban con más frecuencia o que las sobrescrituras se produzcan con menos frecuencia (en cuyo caso, se cambiará el nombre de la nueva imagen con una extensión &quot;-1&quot; o &quot;-2&quot;).

- **Sobrescribir en la carpeta actual, mismo nombre/extensión de la imagen base**.
Esta opción es la regla más estricta para el reemplazo. Requiere que cargue la imagen de reemplazo en la misma carpeta que el original y que la imagen de reemplazo tenga la misma extensión de nombre de archivo que el original. Si no se cumplen estos requisitos, se crea un duplicado.

- **Sobrescribir en la carpeta actual con mismo nombre de recurso base independientemente de la extensión**.
Requiere que cargue la imagen de reemplazo en la misma carpeta que el original, aunque la extensión del nombre del archivo puede ser diferente del original. Por ejemplo, chair.tif reemplaza chair.jpg.

- **Sobrescribir en cualquier carpeta con mismo nombre y ext. de recurso base**.
Requiere que la imagen de reemplazo tenga la misma extensión de nombre de archivo que la imagen original (por ejemplo, chair.jpg debe reemplazar chair.jpg, no chair.tif ). Sin embargo, puede cargar la imagen de sustitución en una carpeta diferente a la original. La imagen actualizada reside en la nueva carpeta; el archivo ya no se puede encontrar en su ubicación original.

- **Sobrescribir en cualquier carpeta, mismo nombre de base independientemente de la extensión**.
Esta opción es la regla de reemplazo más inclusiva. Puede cargar una imagen de reemplazo en una carpeta diferente a la original, cargar un archivo con una extensión de nombre de archivo diferente y reemplazar el archivo original. Si el archivo original está en una carpeta diferente, la imagen de reemplazo reside en la nueva carpeta a la que se cargó.

Obtenga más información acerca de [Opción Sobrescribir imágenes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Aunque no es necesario, al cargar mediante cualquiera de los dos métodos anteriores, puede especificar las opciones de trabajo para esa carga en particular; por ejemplo, para programar una carga recurrente, definir opciones de recorte al cargar y muchas otras. Pueden ser útiles para algunos flujos de trabajo, por lo que vale la pena considerar si pueden serlo para los suyos.

Más información sobre [Opciones de trabajo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

La carga es el primer paso necesario en cualquier flujo de trabajo, ya que Dynamic Media Classic no puede trabajar con contenido que no esté ya en su sistema. Durante la carga, el sistema registra en segundo plano todos los recursos cargados con la base de datos centralizada de Dynamic Media Classic, asigna un ID y lo copia en el almacenamiento. Además, el sistema convierte los archivos de imagen a un formato que permite cambiar el tamaño y el zoom de forma dinámica, y convierte los archivos de vídeo al formato compatible con la web MP4.

### Concepto: Esto es lo que sucede con las imágenes cuando se cargan en Dynamic Media Classic

Al cargar una imagen de cualquier tipo en Dynamic Media Classic, se convierte a un formato de imagen principal denominado TIFF piramidal o TIFF P. Un TIFF P es similar al formato de una imagen de mapa de bits de TIFF con capas, excepto que, en lugar de capas diferentes, el archivo contiene varios tamaños (resoluciones) de la misma imagen.

![imagen](assets/main-workflow/pyramid-p-tiff.png)

A medida que se convierte la imagen, Dynamic Media Classic toma una &quot;instantánea&quot; del tamaño completo de la imagen, la escala a la mitad y la guarda, la escala a la mitad de nuevo y la guarda, y así sucesivamente hasta que se rellena con múltiplos pares del tamaño original. Por ejemplo, un TIFF P de 2000 píxeles tiene tamaños de 1000, 500, 250 y 125 píxeles (y más pequeños) en el mismo archivo. El archivo P-TIFF es el formato de lo que se llama una &quot;imagen maestra&quot; en Dynamic Media Classic.

Cuando solicita una imagen de determinado tamaño, la creación del TIFF P permite al servidor de imágenes para Dynamic Media Classic encontrar rápidamente el siguiente tamaño más grande y reducirlo. Por ejemplo, si carga una imagen de 2000 píxeles y solicita una imagen de 100 píxeles, Dynamic Media Classic encuentra la versión de 125 píxeles y la reduce a 100 píxeles en lugar de escalarla de 2000 a 100 píxeles. Esto hace que la operación sea muy rápida. Además, al aplicar zoom a una imagen, el visor de zoom solo puede solicitar un mosaico de la imagen a la que se está aplicando el zoom, en lugar de toda la imagen con resolución completa. Así es como el formato de imagen principal, el archivo P-TIFF, admite el tamaño dinámico y el zoom.

Del mismo modo, puede cargar el vídeo de origen principal en Dynamic Media Classic y, al cargarlo, Dynamic Media Classic puede cambiar automáticamente su tamaño y convertirlo al formato compatible con la web MP4.

### Reglas básicas para determinar el tamaño óptimo de las imágenes que carga

**Cargue imágenes en el tamaño más grande que necesite.**

- Si necesita hacer zoom, cargue una imagen de alta resolución de un rango de 1500-2500 píxeles en la dimensión más larga. Considere la cantidad de detalle que desea dar, la calidad de las imágenes de origen y el tamaño del producto que se muestra. Por ejemplo, cargue una imagen de 1000 píxeles para un anillo diminuto, pero una imagen de 3000 píxeles para toda una escena de sala.
- Si no necesita hacer zoom, cárguelo al tamaño exacto que se muestra. Por ejemplo, si tiene logotipos o imágenes de salpicaduras/banners para colocar en sus páginas, cárguelos exactamente en su tamaño 1:1 y llámelos exactamente a ese tamaño.

**Nunca aumente de tamaño ni aumente de tamaño sus imágenes antes de cargarlas en Dynamic Media Classic.** Por ejemplo, no aumente la resolución de una imagen más pequeña para convertirla en una imagen de 2000 píxeles. No se verá bien. Haga que las imágenes sean lo más perfectas posible antes de cargarlas.

**No hay un tamaño mínimo para el zoom, pero de forma predeterminada los visores no harán zoom superior al 100%.** Si la imagen es demasiado pequeña, no hará zoom en absoluto o solo hará zoom en una pequeña cantidad para evitar que se vea mal.

**Aunque no hay un mínimo para el tamaño de imagen, no recomendamos cargar imágenes gigantes.** Una imagen gigante puede considerarse de más de 4000 píxeles. Cargar imágenes de este tamaño puede mostrar posibles defectos como granos de polvo o pelos en la imagen. Estas imágenes ocupan más espacio en el servidor de Dynamic Media Classic, lo que puede hacer que supere los límites de almacenamiento contratados.

Más información sobre [Carga de archivos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## Paso 2: Crear (y publicar)

Después de crear y cargar el contenido, creará nuevos recursos de medios enriquecidos a partir de los recursos cargados realizando uno o más subflujos de trabajo. Esto incluye todos los diferentes tipos de colecciones de conjuntos: conjuntos de imágenes, muestras, giros y medios mixtos, así como plantillas. También incluye vídeo. Más adelante analizaremos con mucho bueno detalle cada tipo de conjunto de recopilación de imágenes y medios enriquecidos de vídeo. Sin embargo, en casi todos los casos, para empezar, debe seleccionar uno o más recursos (o no tener ningún recurso seleccionado) y elegir el tipo de recurso que desea crear. Por ejemplo, puede seleccionar una imagen principal y algunas vistas de esa imagen y elegir crear un conjunto de imágenes, una colección de vistas alternativas del mismo producto.

>[!IMPORTANT]
>
>Asegúrese de que todos los recursos estén marcados para la publicación. Mientras que, de forma predeterminada, todos los recursos se marcan automáticamente para su publicación en el momento de la carga, cualquier recurso recién creado a partir del contenido cargado también deberá marcarse para su publicación.

Después de crear el nuevo recurso, se ejecutará un trabajo de publicación. Puede hacerlo manualmente o programar un trabajo de publicación que se ejecute automáticamente. La publicación copia todo el contenido de la esfera privada de Dynamic Media Classic en la esfera pública del servidor de publicación de la ecuación. El producto de un trabajo de publicación de Dynamic Media es una dirección URL única para cada recurso publicado.

El servidor en el que publica depende del tipo de contenido y flujo de trabajo. Por ejemplo, todas las imágenes van al servidor de imágenes y transmiten vídeo al servidor de FMS. Para mayor comodidad, hablaremos de una &quot;publicación&quot; como un solo evento para un solo servidor.

La publicación publica todo el contenido marcado para la publicación, no solo el contenido. Un solo administrador suele publicar en nombre de todos los usuarios, no en nombre de los usuarios individuales que ejecutan una publicación. El administrador puede publicar según sea necesario o configurar un trabajo recurrente diario, semanal o incluso cada 10 minutos que se publicará automáticamente. Publique según un programa que sea adecuado para su negocio.

>[!TIP]
>
>Automatice los trabajos de publicación y programe una publicación completa para que se ejecute todos los días a las 12:00 a. m. o a cualquier hora tarde por la noche.

### Concepto: Explicación de la URL de Dynamic Media Classic

El producto final de un flujo de trabajo de Dynamic Media Classic es una dirección URL que apunta al recurso (ya sea un conjunto de imágenes o un conjunto de vídeos adaptables). Estas direcciones URL son muy predecibles y siguen el mismo patrón. En el caso de las imágenes, cada imagen se genera a partir de la imagen principal de P-TIFF.

Esta es la sintaxis para la URL de una imagen con un par de ejemplos:

![imagen](assets/main-workflow/dmc-url.jpg)

En la dirección URL, todo lo que hay a la izquierda del signo de interrogación es la ruta virtual a una imagen específica. Todo lo que hay a la derecha del signo de interrogación es un modificador Image Server, una instrucción para procesar la imagen. Cuando hay varios modificadores, se separan con el símbolo &quot;et&quot;.

En el primer ejemplo, la ruta virtual a la imagen &quot;Backpack_A&quot; es `http://sample.scene7.com/is/image/s7train/BackpackA`. Los modificadores del servidor de imágenes cambian el tamaño de la imagen a una anchura de 250 píxeles (desde anchura=250) y vuelven a muestrear la imagen con el algoritmo de interpolación Lanczos, que se enfoca a medida que cambia el tamaño (desde resMode=sharp2).

El segundo ejemplo aplica lo que se conoce como &quot;ajuste preestablecido de imagen&quot; a la misma imagen Backpack_A, tal como indica $!_template300$. Los símbolos $ de ambos lados de la expresión indican que se está aplicando a la imagen un ajuste preestablecido de imagen, un conjunto empaquetado de modificadores de imagen.

Una vez que sepa cómo se crean las URL de Dynamic Media Classic, verá cómo cambiarlas mediante programación y cómo integrarlas más profundamente en el sitio y en los sistemas back-end.

### Concepto: Explicación del retraso en el almacenamiento en caché

Los recursos recién cargados y publicados se ven de inmediato, mientras que los recursos actualizados pueden estar sujetos al retraso de 10 horas en el almacenamiento en caché. De forma predeterminada, todos los recursos publicados tienen un mínimo de 10 horas antes de que caduquen. Decimos mínimo, porque cada vez que se ve la imagen, se inicia un reloj que no caduca hasta que han transcurrido 10 horas en las que nadie ha visto esa imagen. Este periodo de 10 horas es el &quot;Tiempo de vida&quot; de un recurso. Una vez que la caché caduca para ese recurso, se puede entregar la versión actualizada.

Esto no suele ser un problema a menos que se produzca un error y la imagen o el recurso tenga el mismo nombre que la versión publicada anteriormente, pero hay un problema con la imagen. Por ejemplo, subió accidentalmente una versión de baja resolución o su director de arte no aprobó la imagen. En este caso, desea recuperar la imagen original y sustituirla por una nueva versión con el mismo ID de recurso.

Obtenga información sobre cómo [Borre manualmente la caché de las direcciones URL que deben actualizarse](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=es).

>[!TIP]
>
>Para evitar problemas con el retraso del almacenamiento en caché, siempre trabaje con anticipación: una noche, un día, dos semanas, etc. Consiga tiempo de control de calidad/aceptación para que las partes internas prueben su trabajo antes de publicarlo. Incluso trabajar una noche antes le permite hacer cambios y volver a publicar esa noche. Por la mañana, han transcurrido las 10 horas y la caché se actualiza con la imagen correcta.

- Más información sobre [Creación de un trabajo de publicación](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- Más información sobre [Publicación](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Paso 3: Entrega

Recuerde que el producto final de un flujo de trabajo de Dynamic Media Classic es una URL que señala al recurso. La dirección URL puede señalar a una imagen individual, un conjunto de imágenes, un conjunto de giros o cualquier otra colección de conjuntos de imágenes o vídeo. Debe tomar esa dirección URL y hacer algo con ella, como editar el HTML para que la `<IMG>` Las etiquetas de apuntan a la imagen de Dynamic Media Classic en lugar de apuntar a una imagen proveniente del sitio actual.

En el paso Enviar, debe integrar esas direcciones URL en su sitio web, aplicación móvil, campaña de correo electrónico o cualquier otro punto de contacto digital en el que desee mostrar el recurso.

Ejemplo de integración de la URL de Dynamic Media Classic para una imagen en un sitio web:

![imagen](assets/main-workflow/example-url-1.png)

La dirección URL en rojo es el único elemento específico de Dynamic Media Classic.

Su equipo de TI o socio de integración puede tomar la iniciativa en la escritura y el cambio de código para integrar las URL de Dynamic Media Classic en su sitio. Adobe tiene un equipo de consultoría que puede ayudar en este esfuerzo, ya sea proporcionando orientación técnica, creativa o general.

Para soluciones más complejas, como visores de zoom o visores que combinan zoom con vistas alternativas, normalmente la URL apunta a un visor alojado por Dynamic Media Classic, y también dentro de esa URL hay una referencia a un ID de recurso.

Ejemplo de un vínculo (en rojo) que abrirá un conjunto de imágenes en un visor en una nueva ventana emergente:

![imagen](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Debe integrar las URL de Dynamic Media Classic en su sitio web, aplicación móvil, correo electrónico y otros puntos de contacto digitales; Dynamic Media Classic no puede hacerlo por usted.

## Vista previa de recursos

Probablemente quiera obtener una vista previa de los recursos que ha cargado o que está creando o editando para asegurarse de que aparecen como desea cuando los clientes los ven. Puede acceder a la ventana Preview haciendo clic en cualquiera **Previsualizar** en la miniatura del recurso, en la parte superior del botón **Panel Examinar/Generar**, o yendo a **Archivo > Vista previa**. En una ventana del explorador, previsualizará cualquier recurso que se encuentre actualmente en el panel, ya sea una imagen, un vídeo o un recurso creado, como un conjunto de imágenes.

### Vista previa de tamaño dinámico (ajustes preestablecidos de imagen)

Puede obtener una vista previa de las imágenes en varios tamaños utilizando **Tamaños** vista previa. Esto carga una lista de los ajustes preestablecidos de imagen disponibles. Discutiremos los ajustes preestablecidos de imagen más adelante, pero piense en ellos como &quot;recetas&quot; para cargar la imagen en un tamaño con nombre con cantidades específicas de enfoque y calidad de imagen.

### Vista previa de zoom

También puede utilizar la variable **Zoom** opción para previsualizar la imagen en uno de los muchos ajustes preestablecidos de zoom creados previamente, que se basan en diferentes visores de zoom incluidos.

Más información sobre [Vista previa de recursos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html).
