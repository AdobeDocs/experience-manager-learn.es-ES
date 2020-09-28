---
title: Flujo de trabajo principal de Dynamic Media Classic y vista previa de recursos
description: Obtenga información sobre el flujo de trabajo principal en Dynamic Media Classic, que incluye los tres pasos Crear (y cargar), Autor (y Publicar) y Entregar. A continuación, aprenda a previsualización de recursos en Dynamic Media Classic.
sub-product: Dynamic-media
feature: workflow
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '2729'
ht-degree: 1%

---


# Flujo de trabajo principal de Dynamic Media Classic y vista previa de recursos {#main-workflow}

Dynamic Media admite un proceso de flujo de trabajo de creación (y carga), creación (y publicación) y entrega. Para inicio, cargue los recursos, luego haga algo con ellos, como crear un conjunto de imágenes y, finalmente, publique para que estén activos. El paso Generar es opcional para algunos flujos de trabajo. Por ejemplo, si el objetivo es solo aplicar un tamaño y un zoom dinámicos a las imágenes o convertir y publicar vídeo para flujo continuo, no es necesario realizar pasos de creación.

![image](assets/main-workflow/create-author-deliver.jpg)

El flujo de trabajo de las soluciones de Dynamic Media Classic consta de tres pasos principales:

1. Crear (y cargar) SourceContent
2. Creación (y publicación) de recursos
3. Entregar recursos

## Paso 1: Crear (y cargar)

Este es el comienzo del flujo de trabajo. En este paso, puede recopilar o crear el contenido de origen que se ajuste al flujo de trabajo que está utilizando y cargarlo en Dynamic Media Classic. El sistema admite varios tipos de archivo para imágenes, vídeo y fuentes, pero también para PDF, Adobe Illustrator y Adobe InDesign.

Consulte la lista completa de los tipos [de archivo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats)admitidos.

Puede cargar el contenido de origen de varias formas:

- Directamente desde el escritorio o la red local. [Conozca cómo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- Desde un servidor FTP de Dynamic Media Classic. [Conozca cómo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

El modo predeterminado es Desde el escritorio, donde se buscan archivos en la red local y se realiza el inicio de la carga.

![image](assets/main-workflow/upload.jpg)

>[!TIP]
>
>No agregue manualmente las carpetas. En su lugar, ejecute una carga desde FTP y utilice la opción **Incluir subcarpetas** para volver a crear la estructura de carpetas dentro de Dynamic Media Classic.

Las dos opciones de carga más importantes están activadas de forma predeterminada: **Marcar para publicación**, que hemos analizado anteriormente, y **Sobrescribir**. Sobrescribir significa que si el archivo que se está cargando tiene el mismo nombre que un archivo que ya está en el sistema, el nuevo archivo reemplazará a la versión existente. Si desactiva esta opción, es posible que el archivo no se cargue.

### Sobrescribir opciones al cargar imágenes

Existen cuatro variaciones de la opción Sobrescribir imagen que se pueden configurar para toda la compañía y que a menudo no se comprenden bien. En resumen, puede establecer las reglas de modo que desee que los recursos con el mismo nombre se sobrescriban con más frecuencia o desea que las sobrescrituras se produzcan con menos frecuencia (en cuyo caso se cambiará el nombre de la nueva imagen por una extensión &quot;-1&quot; o &quot;-2&quot;).

- **Sobrescribir en la carpeta actual, con el mismo nombre/extensión**de imagen base.
Esta opción es la regla más estricta de reemplazo. Requiere que la imagen de sustitución se cargue en la misma carpeta que la imagen original y que la imagen de sustitución tenga la misma extensión de nombre de archivo que la imagen original. Si no se cumplen estos requisitos, se crea un duplicado.

- **Sobrescribir en la carpeta actual, el mismo nombre de recurso base independientemente de la extensión**.
Requiere que la imagen de sustitución se cargue en la misma carpeta que la original, aunque la extensión del nombre de archivo puede ser diferente a la original. Por ejemplo, silla.tif reemplaza a silla.jpg.

- **Sobrescribir en cualquier carpeta, con el mismo nombre o extensión**de recurso base.
Requiere que la imagen de reemplazo tenga la misma extensión de nombre de archivo que la imagen original (por ejemplo, silla.jpg debe reemplazar silla.jpg, no silla.tif ). Sin embargo, puede cargar la imagen de reemplazo en una carpeta distinta a la original. La imagen actualizada reside en la nueva carpeta; el archivo ya no se encuentra en su ubicación original.

- **Sobrescribir en cualquier carpeta, el mismo nombre de recurso base independientemente de la extensión**.
Esta opción es la regla de reemplazo más inclusiva. Puede cargar una imagen de sustitución en una carpeta distinta a la original, cargar un archivo con una extensión de nombre de archivo diferente y reemplazar el archivo original. Si el archivo original se encuentra en una carpeta diferente, la imagen de reemplazo reside en la nueva carpeta en la que se cargó.

Obtenga más información sobre la opción [](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option)Sobrescribir imágenes.

Aunque no es necesario, al cargar mediante cualquiera de los dos métodos anteriores, puede especificar las opciones de trabajo para esa carga en particular: por ejemplo, para programar una carga recurrente, defina las opciones de recorte al cargarla, entre otras muchas. Estos pueden ser valiosos para algunos flujos de trabajo, así que vale la pena considerar si pueden ser para los tuyos.

Obtenga más información sobre las opciones [de trabajo](https://docs.adobe.com/content/help/es-ES/dynamic-media-classic/using/upload-publish/uploading-files.translate.html#upload-options).

La carga es el primer paso necesario en cualquier flujo de trabajo, ya que Dynamic Media Classic no puede trabajar con contenido que no esté ya en su sistema. Entre bastidores durante la carga, el sistema registra todos los recursos cargados con la base de datos centralizada de Dynamic Media Classic, asigna un ID y lo copia en almacenamiento. Además, el sistema convierte los archivos de imagen a un formato que permite el cambio de tamaño y zoom dinámicos y convierte los archivos de vídeo al formato compatible con la Web MP4.

### Concepto: Esto es lo que ocurre con las imágenes al cargarlas en Dynamic Media Classic

Al cargar una imagen de cualquier tipo en Dynamic Media Classic, se convierte a un formato de imagen principal denominado TIFF piramidal o P-TIFF. Un P-TIFF es similar al formato de una imagen de mapa de bits TIFF con capas, excepto que en lugar de capas diferentes, el archivo contiene varios tamaños (resoluciones) de la misma imagen.

![image](assets/main-workflow/pyramid-p-tiff.png)

A medida que se convierte la imagen, Dynamic Media Classic toma una &quot;instantánea&quot; del tamaño completo de la imagen, la escala a la mitad y la guarda, la escala a la mitad de nuevo y la guarda, y así sucesivamente hasta que se rellene con múltiplos pares del tamaño original. Por ejemplo, un P-TIFF de 2000 píxeles tendrá tamaños de 1000, 500, 250 y 125 píxeles (y más pequeños) en el mismo archivo. El archivo P-TIFF es el formato de lo que se denomina &quot;imagen principal&quot; en Dynamic Media Classic.

Cuando se solicita una imagen de cierto tamaño, la creación del P-TIFF permite al servidor de imágenes de Dynamic Media Classic encontrar rápidamente el siguiente tamaño mayor y reducirlo. Por ejemplo, si carga una imagen de 2000 píxeles y solicita una imagen de 100 píxeles, Dynamic Media Classic buscará la versión de 125 píxeles y la reducirá a 100 píxeles en lugar de escalar de 2000 a 100 píxeles. Esto hace que la operación sea muy rápida. Además, al hacer zoom en una imagen, esto permite al visor de zoom solicitar únicamente un mosaico de la imagen que se está haciendo zoom, en lugar de toda la imagen de resolución completa. Así es como el formato de imagen principal, el archivo P-TIFF, admite tanto el tamaño dinámico como el zoom.

Del mismo modo, puede cargar el vídeo de origen maestro en Dynamic Media Classic y, al cargarlo, Dynamic Media Classic puede cambiarle el tamaño automáticamente y convertirlo al formato compatible con la Web MP4.

### Reglas de miniatura para determinar el tamaño óptimo de las imágenes cargadas

**Cargue imágenes con el tamaño más grande que necesite.**

- Si necesita aplicar zoom, cargue una imagen de alta resolución de un rango de 1500 a 2500 píxeles en la dimensión más larga. Tenga en cuenta los detalles que desea proporcionar, la calidad de las imágenes de origen y el tamaño del producto que se muestra. Por ejemplo, cargue una imagen de 1000 píxeles para un anillo pequeño, pero una imagen de 3000 píxeles para un círculo de habitaciones completo.
- Si no necesita aplicar zoom, cárguelo con el tamaño exacto que verá. Por ejemplo, si tiene logotipos o imágenes de bienvenida o pancarta para colocar en las páginas, cárguelas exactamente en su tamaño 1:1 y llámelas exactamente en ese tamaño.

**Nunca muestree o haga estallar las imágenes antes de cargarlas a Dynamic Media Classic.** Por ejemplo, no aumente la resolución de una imagen más pequeña para convertirla en una imagen de 2000 píxeles. No se verá bien. Haga que las imágenes estén lo más cerca posible antes de cargarlas.

**No hay un tamaño mínimo para el zoom, pero de forma predeterminada los visores no aplican un zoom superior al 100 %.** Si la imagen es demasiado pequeña, no se aplicará ningún zoom o solo se aplicará un pequeño zoom para evitar que se vea mal.

**Aunque no hay un mínimo de tamaño de imagen, no recomendamos cargar imágenes gigantes.** Una imagen gigante puede considerarse más de 4000 píxeles. Cargar imágenes de este tamaño puede mostrar posibles defectos como granos de polvo o pelos en la imagen. Estas imágenes también ocuparán más espacio en el servidor de Dynamic Media Classic, lo que puede hacer que supere los límites de almacenamiento contraídos.

Obtenga más información sobre la [carga de archivos](https://docs.adobe.com/content/help/es-ES/dynamic-media-classic/using/upload-publish/uploading-files.translate.html#uploading-your-files).

## Paso 2: Autor (y publicación)

Después de crear y cargar el contenido, podrá crear nuevos recursos de medios enriquecidos a partir de los recursos cargados realizando uno o varios flujos de trabajo secundarios. Esto incluye todos los diferentes tipos de colecciones de conjuntos: Conjuntos de imágenes, muestras, giros y medios mixtos, así como plantillas. También incluye vídeo. Más adelante analizaremos muchos buenos detalles sobre cada tipo de conjunto de colecciones de imágenes y medios enriquecidos en vídeo. Sin embargo, en casi todos los casos, el inicio se realiza seleccionando uno o varios recursos (o no hay ningún recurso seleccionado) y eligiendo el tipo de recurso que se desea crear. Por ejemplo, puede seleccionar una imagen principal y unas pocas vistas de esa imagen y elegir crear un conjunto de imágenes, una colección de vistas alternativas del mismo producto.

>[!IMPORTANT]
>
>Asegúrese de que todos los recursos están marcados para la publicación. Aunque de forma predeterminada todos los recursos se marcan automáticamente para la publicación durante la carga, los recursos recién creados del contenido cargado también deberán marcarse para la publicación.

Después de crear el nuevo recurso, ejecutará un trabajo de publicación. Puede hacerlo manualmente o programar un trabajo de publicación que se ejecute automáticamente. La publicación copia todo el contenido de la esfera privada de Dynamic Media Classic en la esfera pública del servidor de publicación de la ecuación. El producto de un trabajo de publicación de Dynamic Media es una dirección URL única para cada recurso publicado.

El servidor en el que realiza la publicación depende del tipo de contenido y del flujo de trabajo. Por ejemplo, todas las imágenes van al servidor de imágenes y transmiten vídeo al servidor FMS. Para mayor comodidad, hablaremos de una &quot;publicación&quot; como un solo evento en un único servidor.

Publicación publica todo el contenido marcado para publicación — no solo tu contenido. Normalmente, un único administrador publica en nombre de todos, en lugar de usuarios individuales que ejecutan una publicación. El administrador puede publicar según sea necesario o configurar un trabajo recurrente diario, semanal o incluso cada 10 minutos que se publicará automáticamente. Publique en un programa que tenga sentido para su empresa.

>[!TIP]
>
>Automatice los trabajos de publicación y programe una publicación completa para que se ejecute todos los días a las 12:00 AM o a cualquier hora de la tarde.

### Concepto: Explicación de la URL de Dynamic Media Classic

El producto final de un flujo de trabajo de Dynamic Media Classic es una URL que apunta al recurso (ya sea un conjunto de imágenes o un conjunto de vídeos adaptables). Estas direcciones URL son muy predecibles y siguen el mismo patrón. En el caso de las imágenes, cada imagen se genera a partir de la imagen principal P-TIFF.

Esta es la sintaxis de la URL de una imagen con un par de ejemplos:

![image](assets/main-workflow/dmc-url.jpg)

En la URL, todo lo que se encuentra a la izquierda del signo de interrogación es la ruta virtual a una imagen específica. Todo lo que se encuentra a la derecha del signo de interrogación es un modificador del servidor de imágenes, una instrucción para procesar la imagen. Cuando hay varios modificadores, se separan con ampersands.

En el primer ejemplo, la ruta virtual a la imagen &quot;Backpack_A&quot; es `http://sample.scene7.com/is/image/s7train/BackpackA`. Los modificadores del servidor de imágenes cambian el tamaño de la imagen a una anchura de 250 píxeles (desde wid=250) y vuelven a muestrear la imagen con el algoritmo de interpolación Lanczos, que se enfoca a medida que cambia de tamaño (desde resMode=sharp2).

El segundo ejemplo aplica lo que se conoce como &quot;ajuste preestablecido de imagen&quot; a la misma imagen Backpack_A, como indica $!_template300$. Los símbolos $ de cada lado de la expresión indican que se está aplicando a la imagen un ajuste preestablecido de imagen, un conjunto empaquetado de modificadores de imagen.

Una vez que haya comprendido cómo se agrupan las direcciones URL de Dynamic Media Classic, comprenderá cómo cambiarlas mediante programación y cómo integrarlas más profundamente en el sitio y en los sistemas back-end.

### Concepto: Explicación del retraso en el almacenamiento en caché

Los recursos recién cargados y publicados se verán de inmediato, mientras que los recursos actualizados pueden estar sujetos al retraso de 10 horas en el almacenamiento en caché. De forma predeterminada, todos los recursos publicados tienen un mínimo de 10 horas antes de que caduquen. Decimos mínimo, porque cada vez que se ve la imagen, inicio un reloj que no caducará hasta que pasen 10 horas en las que nadie haya visto esa imagen. Este período de 10 horas es el &quot;Tiempo de vida&quot; de un recurso. Una vez que la caché caduque para ese recurso, se puede entregar la versión actualizada.

Normalmente, no se trata de un problema a menos que se produzca un error y la imagen o el recurso tengan el mismo nombre que la versión publicada anteriormente, pero haya un problema con la imagen. Por ejemplo, se cargó accidentalmente una versión de baja resolución o el director de arte no aprobó la imagen. En este caso, desea recuperar la imagen original y reemplazarla por una nueva versión con el mismo ID de recurso.

Obtenga información sobre cómo borrar [manualmente la caché de las direcciones URL que deben actualizarse](https://docs.adobe.com/content/help/en/experience-manager-65/assets/dynamic/invalidate-cdn-cached-content.html).

>[!TIP]
>
>Para evitar problemas con el retraso del almacenamiento en caché, trabaje siempre con antelación: una noche, un día, dos semanas, etc. Genere a tiempo el control de calidad/aceptación para que las partes internas prueba su trabajo antes de publicarlo al público. Incluso trabajar una noche antes te permite hacer cambios y volver a publicar esa noche. Por la mañana, han pasado 10 horas y la caché se actualiza con la imagen correcta.

- Obtenga más información sobre la [creación de un trabajo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job)de publicación.
- Obtenga más información sobre [Publicación](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Paso 3: Entregar

Recuerde que el producto final de un flujo de trabajo de Dynamic Media Classic es una URL que apunta al recurso. La dirección URL puede indicar una imagen individual, un conjunto de imágenes, un conjunto de giros o cualquier otra colección o vídeo de conjuntos de imágenes. Debe tomar esa dirección URL y hacer algo con ella, como editar el HTML para que las `<IMG>` etiquetas apunten a la imagen de Dynamic Media Classic en lugar de apuntar a una imagen que proviene del sitio actual.

En el paso Entregar, debe integrar esas direcciones URL en el sitio web, la aplicación móvil, la campaña por correo electrónico o cualquier otro punto táctil digital en el que desee mostrar el recurso.

Ejemplo de integración de la URL de Dynamic Media Classic para una imagen en un sitio web:

![image](assets/main-workflow/example-url-1.png)

La URL en rojo es el único elemento específico de Dynamic Media Classic.

El equipo de TI o el socio de integración pueden tomar la delantera en la escritura y el cambio de código para integrar las direcciones URL de Dynamic Media Classic en el sitio. Adobe cuenta con un equipo de asesoría que puede ayudarle en este esfuerzo, ya sea proporcionando orientación técnica, creativa o general.

Para soluciones más complejas, como visores de zoom o visores que combinan zoom con vistas alternativas, la URL normalmente apunta a un visor alojado por Dynamic Media Classic y también dentro de esa URL es una referencia a un ID de recurso.

Ejemplo de vínculo (en rojo) que abrirá un conjunto de imágenes en un visor en una nueva ventana emergente:

![image](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Debe integrar las URL de Dynamic Media Classic en el sitio web, la aplicación móvil, el correo electrónico y otros puntos de contacto digitales: ¡Dynamic Media Classic no puede hacer eso por usted!

## Previsualización de recursos

Es probable que desee realizar la previsualización de los recursos que ha cargado o que esté creando o editando para asegurarse de que aparecen como desea cuando los clientes los vista. Para acceder a la ventana Previsualización, haga clic en cualquier botón de **Previsualización** , ya sea en la miniatura del recurso, en la parte superior del panel **Examinar/Generar, o vaya a** Archivo > Previsualización ****. En una ventana del navegador, se previsualización cualquier recurso que esté actualmente en el panel, ya sea una imagen, un vídeo o un recurso generado como un conjunto de imágenes.

### Previsualización de tamaño dinámico (ajustes preestablecidos de imagen)

Puede previsualización de las imágenes en varios tamaños mediante la previsualización **Tamaños** . Esto carga una lista de los ajustes preestablecidos de imagen disponibles. Más adelante analizaremos los ajustes preestablecidos de imagen, pero pensaremos en ellos como &quot;fórmulas&quot; para cargar la imagen a un tamaño con nombre con cantidades específicas de enfoque y calidad de imagen.

### Previsualización de zoom

También puede utilizar la opción **Zoom** para previsualización de la imagen en uno de los muchos ajustes preestablecidos de zoom creados previamente, basados en distintos visores de zoom incluidos.

Obtenga más información sobre la [vista previa de recursos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/previewing-asset.html).
