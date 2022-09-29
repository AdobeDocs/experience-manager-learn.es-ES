---
title: Flujo de trabajo principal de Dynamic Media Classic y vista previa de recursos
description: 'Obtenga información sobre el flujo de trabajo principal en Dynamic Media Classic, que incluye los tres pasos: Crear (y Cargar), Autor (y Publicar) y Enviar. A continuación, aprenda a previsualizar recursos en Dynamic Media Classic.'
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2703'
ht-degree: 1%

---

# Flujo de trabajo principal de Dynamic Media Classic y vista previa de recursos {#main-workflow}

Dynamic Media es compatible con los procesos de flujo de trabajo Crear (y Cargar), Autor (y Publicar) y Entregar . Para empezar, cargue los recursos, luego haga algo con ellos, como crear un conjunto de imágenes, y finalmente publique para hacerlos activos. El paso Generar es opcional para algunos flujos de trabajo. Por ejemplo, si su objetivo es hacer solo el ajuste de tamaño dinámico y el zoom en imágenes, o convertir y publicar vídeo para flujo continuo, no es necesario realizar pasos de compilación.

![imagen](assets/main-workflow/create-author-deliver.jpg)

El flujo de trabajo de las soluciones de Dynamic Media Classic consta de tres pasos principales:

1. Crear (y cargar) SourceContent
2. Creación (y publicación) de recursos
3. Enviar recursos

## Paso 1: Crear (y cargar)

Este es el principio del flujo de trabajo. En este paso, puede recopilar o crear el contenido de origen que se ajuste al flujo de trabajo que está utilizando y cargarlo en Dynamic Media Classic. El sistema admite varios tipos de archivos para imágenes, vídeos y fuentes, pero también para PDF, Adobe Illustrator y Adobe InDesign.

Consulte la lista completa de [Tipos de archivo compatibles](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

Puede cargar el contenido de origen de varias formas:

- Directamente desde el escritorio o la red local. [Descubra cómo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- Desde un servidor FTP de Dynamic Media Classic. [Descubra cómo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

El modo predeterminado es Desde escritorio, donde se buscan archivos en la red local y se inicia la carga.

![imagen](assets/main-workflow/upload.jpg)

>[!TIP]
>
>No agregue manualmente sus carpetas. En su lugar, ejecute una carga desde FTP y use el **Incluir subcarpetas** para volver a crear la estructura de carpetas dentro de Dynamic Media Classic.

Las dos opciones de carga más importantes están habilitadas de forma predeterminada: **Marcar para publicación**, que hemos discutido anteriormente, y **Sobrescribir**. Sobrescribir significa que si el archivo que se está cargando tiene el mismo nombre que un archivo que ya se encuentra en el sistema, el nuevo archivo reemplazará a la versión existente. Si desactiva esta opción, es posible que el archivo no se cargue.

### Sobrescribir opciones al cargar imágenes

Existen cuatro variaciones de la opción Sobrescribir imagen que se pueden configurar para toda la empresa y que a menudo se malinterpretan. En resumen, puede establecer las reglas de modo que desee que los recursos con el mismo nombre se sobrescriban con mayor frecuencia, o bien desea que las sobrescrituras se produzcan con menor frecuencia (en cuyo caso se cambiará el nombre de la nueva imagen por una extensión &quot;-1&quot; o &quot;-2&quot;).

- **Sobrescribir en la carpeta actual, el mismo nombre/extensión de imagen base**.
Esta opción es la regla más estricta para la sustitución. Requiere que cargue la imagen de reemplazo en la misma carpeta que la original y que la imagen de reemplazo tenga la misma extensión de nombre de archivo que la original. Si no se cumplen estos requisitos, se crea un duplicado.

- **Sobrescribir en la carpeta actual, el mismo nombre de recurso base independientemente de la extensión**.
Requiere que cargue la imagen de reemplazo en la misma carpeta que el original, aunque la extensión del nombre de archivo puede ser diferente de la original. Por ejemplo, chair.tif reemplaza a chair.jpg.

- **Sobrescribir en cualquier carpeta con mismo nombre y ext. de recurso base**.
Requiere que la imagen de reemplazo tenga la misma extensión de nombre de archivo que la imagen original (por ejemplo, chair.jpg debe reemplazar chair.jpg, no chair.tif ). Sin embargo, puede cargar la imagen de reemplazo en una carpeta diferente a la original. La imagen actualizada reside en la nueva carpeta; el archivo ya no se puede encontrar en su ubicación original.

- **Sobrescribir en cualquier carpeta, mismo nombre de base independientemente de la extensión**.
Esta opción es la regla de reemplazo más inclusiva. Puede cargar una imagen de reemplazo en una carpeta distinta a la original, cargar un archivo con una extensión de nombre de archivo diferente y reemplazar el archivo original. Si el archivo original se encuentra en una carpeta diferente, la imagen de reemplazo reside en la nueva carpeta a la que se cargó.

Obtenga más información sobre [Opción Sobrescribir imágenes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Aunque no es necesario, al cargar mediante cualquiera de los dos métodos anteriores, puede especificar Opciones de trabajo para esa carga en particular, por ejemplo, para programar una carga recurrente, establecer opciones de recorte al cargarla y muchas otras. Pueden ser útiles para algunos flujos de trabajo, por lo que vale la pena considerar si pueden ser para el suyo.

Más información sobre [Opciones de trabajo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

La carga es el primer paso necesario en cualquier flujo de trabajo, ya que Dynamic Media Classic no puede trabajar con contenido que no esté en su sistema. Entre bastidores durante la carga, el sistema registra cada recurso cargado con la base de datos centralizada de Dynamic Media Classic, asigna un ID y lo copia en el almacenamiento. Además, el sistema convierte los archivos de imagen a un formato que permite el cambio de tamaño y el zoom dinámicos y convierte los archivos de vídeo al formato compatible con la web de MP4.

### Concepto: Esto es lo que sucede con las imágenes al cargarlas en Dynamic Media Classic

Cuando se carga una imagen de cualquier tipo en Dynamic Media Classic, se convierte a un formato de imagen principal denominado TIFF pirámide o TIFF P. Un TIFF P es similar al formato de una imagen de mapa de bits de TIFF en capas, excepto que en lugar de capas diferentes, el archivo contiene varios tamaños (resoluciones) de la misma imagen.

![imagen](assets/main-workflow/pyramid-p-tiff.png)

A medida que la imagen se convierte, Dynamic Media Classic toma una &quot;instantánea&quot; del tamaño completo de la imagen, la escala a la mitad y la guarda, la escala a la mitad de nuevo y la guarda, y así sucesivamente hasta que se rellene con múltiplos pares del tamaño original. Por ejemplo, un TIFF P de 2000 píxeles tiene tamaños de 1000, 500, 250 y 125 píxeles (y más pequeños) en el mismo archivo. El archivo TIFF P es el formato de lo que se denomina &quot;imagen maestra&quot; en Dynamic Media Classic.

Cuando se solicita una imagen de cierto tamaño, la creación del TIFF P permite que el Servidor de imágenes para Dynamic Media Classic encuentre rápidamente el siguiente tamaño mayor y lo reduzca. Por ejemplo, si carga una imagen de 2000 píxeles y solicita una imagen de 100 píxeles, Dynamic Media Classic encuentra la versión de 125 píxeles y la escala a 100 píxeles en lugar de escalar de 2000 a 100 píxeles. Esto hace que la operación sea muy rápida. Además, al ampliar una imagen, esto permite al visor de zoom solicitar únicamente un mosaico de la imagen que se está ampliando, en lugar de toda la imagen de resolución completa. Así es como el formato de imagen principal, el archivo TIFF P, admite el tamaño dinámico y el zoom.

Del mismo modo, puede cargar el vídeo de origen maestro en Dynamic Media Classic y, al cargarlo, Dynamic Media Classic puede cambiar su tamaño automáticamente y convertirlo al formato compatible con la web de MP4.

### Reglas de rastreo para determinar el tamaño óptimo de las imágenes cargadas

**Cargue imágenes con el tamaño más grande que necesite.**

- Si necesita hacer zoom, cargue una imagen de alta resolución de un rango de 1500-2500 píxeles en la dimensión más larga. Tenga en cuenta cuánto detalle desea dar, la calidad de las imágenes de origen y el tamaño del producto que se muestra. Por ejemplo, cargue una imagen de 1000 píxeles para un anillo pequeño, pero una imagen de 3000 píxeles para toda la escena de la sala.
- Si no necesita hacer zoom, cárguelo con el tamaño exacto en que aparece. Por ejemplo, si tiene logotipos o imágenes de splash/banner para colocarlas en sus páginas, cárguelas exactamente en su tamaño 1:1 y llámelas exactamente en ese tamaño.

**Nunca realice una muestra o una explosión de las imágenes antes de cargarlas en Dynamic Media Classic.** Por ejemplo, no actualice una imagen más pequeña para convertirla en una imagen de 2000 píxeles. No se verá bien. Haga que sus imágenes estén lo más cerca posible de la perfección antes de cargarlas.

**No hay un tamaño mínimo para el zoom, pero de forma predeterminada los visores no se acercan más del 100%.** Si la imagen es demasiado pequeña, no se amplía o solo se reduce una pequeña cantidad para evitar que se vea mal.

**Aunque no hay un mínimo de tamaño de imagen, no se recomienda cargar imágenes gigantes.** Una imagen gigante se puede considerar más de 4000 píxeles. Cargar imágenes de este tamaño puede mostrar posibles defectos como granos de polvo o pelos en la imagen. Estas imágenes ocupan más espacio en el servidor Dynamic Media Classic, lo que puede hacer que supere los límites de almacenamiento contratados.

Más información sobre [Carga de archivos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## Paso 2: Autor (y publicación)

Después de crear y cargar el contenido, puede crear nuevos recursos de medios enriquecidos a partir de los recursos cargados realizando uno o más subflujos de trabajo. Esto incluye todos los tipos de conjuntos de colecciones: conjuntos de imágenes, muestras, giros y medios mixtos, así como plantillas. También incluye vídeo. Más adelante veremos muchos buenos detalles sobre cada tipo de conjunto de recopilación de imágenes y medios enriquecidos de vídeo. Sin embargo, en casi todos los casos, puede empezar por seleccionar uno o varios recursos (o no tener recursos seleccionados) y elegir el tipo de recurso que desea crear. Por ejemplo, puede seleccionar una imagen principal y unas pocas vistas de esa imagen y elegir crear un conjunto de imágenes, una colección de vistas alternativas del mismo producto.

>[!IMPORTANT]
>
>Asegúrese de que todos los recursos estén marcados para su publicación. Aunque de forma predeterminada todos los recursos se marcan automáticamente para su publicación en la carga, cualquier recurso recién creado del contenido cargado también deberá marcarse para su publicación.

Después de crear el nuevo recurso, ejecutará un trabajo de publicación. Puede hacerlo manualmente o programar un trabajo de publicación que se ejecute automáticamente. La publicación copia todo el contenido de la esfera privada de Dynamic Media Classic al público y a la esfera del servidor de publicación de la ecuación. El producto de un trabajo de publicación de Dynamic Media es una dirección URL única para cada recurso publicado.

El servidor para el que publica depende del tipo de contenido y del flujo de trabajo. Por ejemplo, todas las imágenes van al servidor de imágenes y transmiten vídeo al servidor FMS. Para su comodidad, hablaremos de una &quot;publicación&quot; como un solo evento en un solo servidor.

La publicación publica todo el contenido marcado para publicación, no solo el contenido. Un solo administrador suele publicar en nombre de todos, en lugar de usuarios individuales que ejecutan una publicación. El administrador puede publicar según sea necesario o configurar un trabajo periódico diario, semanal o incluso cada 10 minutos que se publicará automáticamente. Publique en una programación que tenga sentido para su negocio.

>[!TIP]
>
>Automatice los trabajos de publicación y programe una publicación completa para que se ejecute todos los días a las 12:00 a.m. o en cualquier momento tarde por la noche.

### Concepto: Explicación de la URL de Dynamic Media Classic

El producto final de un flujo de trabajo de Dynamic Media Classic es una URL que apunta al recurso (ya sea un conjunto de imágenes o un conjunto de vídeos adaptables). Estas direcciones URL son muy predecibles y siguen el mismo patrón. En el caso de las imágenes, cada imagen se genera a partir de la imagen maestra del TIFF P.

Esta es la sintaxis de la URL de una imagen con un par de ejemplos:

![imagen](assets/main-workflow/dmc-url.jpg)

En la dirección URL, todo lo que aparece a la izquierda del signo de interrogación es la ruta virtual a una imagen específica. Todo a la derecha del signo de interrogación es un modificador de Image Server, una instrucción para procesar la imagen. Cuando hay varios modificadores, estos se separan con el símbolo &amp;.

En el primer ejemplo, la ruta virtual a la imagen &quot;Backpack_A&quot; es `http://sample.scene7.com/is/image/s7train/BackpackA`. Los modificadores del servidor de imágenes cambian el tamaño de la imagen a una anchura de 250 píxeles (a partir de wid=250) y vuelven a muestrear la imagen utilizando el algoritmo de interpolación de Lanczos, que se ajusta a medida que cambia de tamaño (de resMode=Sharp2).

El segundo ejemplo aplica lo que se conoce como &quot;ajuste preestablecido de imagen&quot; a la misma imagen Backpack_A, como se indica con $!_template300$. Los símbolos $ a ambos lados de la expresión indican que se está aplicando a la imagen un ajuste preestablecido de imagen, un conjunto empaquetado de modificadores de imagen.

Una vez que sepa cómo se agrupan las URL de Dynamic Media Classic, entonces comprenderá cómo cambiarlas mediante programación y cómo integrarlas más profundamente en su sitio y en los sistemas back-end.

### Concepto: Explicación del retraso en el almacenamiento en caché

Los recursos recién cargados y publicados se ven de inmediato, mientras que los recursos actualizados pueden estar sujetos al retraso de 10 horas en el almacenamiento en caché. De forma predeterminada, todos los recursos publicados tienen un mínimo de 10 horas antes de que caduquen. Decimos mínimo, porque cada vez que se ve la imagen, comienza un reloj que no caduca hasta que han transcurrido 10 horas desde que nadie ha visto la imagen. Este período de 10 horas es el &quot;Tiempo de vida&quot; de un recurso. Una vez que la caché caduca para ese recurso, se puede entregar la versión actualizada.

Normalmente, esto no supone un problema a menos que se produzca un error y la imagen o el recurso tengan el mismo nombre que la versión publicada anteriormente, pero haya un problema con la imagen. Por ejemplo, accidentalmente cargaste una versión de baja resolución o tu director de arte no aprobó la imagen. En este caso, desea recuperar la imagen original y reemplazarla con una nueva versión utilizando el mismo ID de recurso.

Obtenga información sobre cómo [Borre manualmente la caché de las direcciones URL que deben actualizarse](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=es).

>[!TIP]
>
>Para evitar problemas con el retraso del almacenamiento en caché, trabaje siempre con anticipación: una tarde, un día, dos semanas, etc. Cree una versión a tiempo para el control de calidad/aceptación de las partes internas para probar su trabajo antes de publicarlo al público. Incluso trabajar una tarde antes te permite hacer cambios y volver a publicar esa tarde. Por la mañana, han pasado las 10 horas y la caché se actualiza con la imagen correcta.

- Más información sobre [Creación de un trabajo de publicación](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- Más información sobre [Publicación](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Paso 3: Entrega

Recuerde que el producto final de un flujo de trabajo de Dynamic Media Classic es una URL que apunta al recurso. La dirección URL puede apuntar a una imagen individual, a un conjunto de imágenes, a un conjunto de giros o a cualquier otra colección o vídeo de conjunto de imágenes. Debe tomar esa dirección URL y hacer algo con ella, como editar el HTML para que el `<IMG>` Las etiquetas apuntan a la imagen de Dynamic Media Classic en lugar de apuntar a una imagen proveniente de su sitio actual.

En el paso Entregar , debe integrar esas direcciones URL en su sitio web, aplicación móvil, campaña de correo electrónico o en cualquier otro punto de contacto digital en el que desee mostrar el recurso.

Ejemplo de integración de la URL de Dynamic Media Classic para una imagen en un sitio web:

![imagen](assets/main-workflow/example-url-1.png)

La dirección URL en rojo es el único elemento específico de Dynamic Media Classic.

Su equipo de TI o socio de integración puede tomar la iniciativa en la escritura y el cambio de código para integrar las URL de Dynamic Media Classic en su sitio. Adobe cuenta con un equipo de consultoría que puede ayudarle en este esfuerzo, ya sea proporcionando orientación técnica, creativa o general.

Para soluciones más complejas, como visores de zoom o visores que combinan zoom con vistas alternativas, normalmente la URL apunta a un visor alojado por Dynamic Media Classic y también dentro de esa URL es una referencia a un ID de recurso.

Ejemplo de vínculo (en rojo) que abrirá un conjunto de imágenes en un visor en una nueva ventana emergente:

![imagen](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Debe integrar las URL de Dynamic Media Classic en su sitio web, aplicación móvil, correo electrónico y otros puntos de contacto digitales: Dynamic Media Classic no puede hacerlo por usted.

## Previsualización de recursos

Probablemente quiera previsualizar los recursos que ha cargado, o que está creando o editando para asegurarse de que aparecen como desea cuando los clientes los ven. Para acceder a la ventana Vista previa, haga clic en cualquier **Vista previa** , en la miniatura del recurso, en la parte superior del **Panel Examinar/Generar**, o bien, accediendo a **Archivo > Vista previa**. En una ventana del explorador, se previsualizará cualquier recurso que haya en el panel, ya sea una imagen, un vídeo o un recurso creado como un conjunto de imágenes.

### Vista previa de tamaño dinámico (ajustes preestablecidos de imagen)

Puede obtener una vista previa de las imágenes en varios tamaños utilizando la variable **Tamaños** vista previa. Esto carga una lista de los ajustes preestablecidos de imagen disponibles. Discutiremos los Ajustes preestablecidos de imagen más adelante, pero pensaremos en ellos como &quot;recetas&quot; para cargar la imagen a un tamaño determinado con cantidades específicas de nitidez y calidad de imagen.

### Vista previa de zoom

También puede usar la variable **Zoom** para obtener una vista previa de la imagen en uno de los muchos ajustes preestablecidos de zoom predefinidos, que se basan en distintos visores de zoom incluidos.

Más información sobre [Vista previa de recursos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html).
