---
title: Recorte, imágenes ajustadas y Destinatarios de zoom
description: La imagen principal de Dynamic Media Classic admite la creación de versiones recortadas independientes de cada imagen para mostrar detalles o muestras sin tener que crear versiones recortadas independientes de cada imagen. Descubra cómo recortar imágenes en Dynamic Media Classic y guardarlas como un nuevo archivo maestro o una imagen virtual, guardar imágenes ajustadas virtuales y utilizarlas en lugar de recursos principales y crear Destinatarios de zoom en las imágenes para mostrar los detalles resaltados.
sub-product: Dynamic-media
feature: smart-crop
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '2665'
ht-degree: 0%

---


# Recorte, imágenes ajustadas y Destinatarios de zoom {#crop-adjusted-zoom-targets}

Una de las principales ventajas del concepto de imagen principal de Dynamic Media Classic es que puede reutilizar el recurso de imagen para muchos usos. Tradicionalmente, tendría que crear versiones separadas recortadas de cada imagen para mostrar detalles o muestras. Al utilizar Dynamic Media Classic, puede realizar las mismas tareas en su único maestro y guardar las versiones recortadas como nuevos archivos físicos o como derivados virtuales que no tienen espacio en almacenamiento.

Al final de esta sección del tutorial, sabrá cómo:

- Recorte imágenes en Dynamic Media Classic y guárdelas como nuevos archivos maestros o como imágenes virtuales. [Más información](https://docs.adobe.com/help/en/dynamic-media-classic/using/master-files/cropping-image.html).
- Guarde las imágenes virtuales ajustadas y utilícelas en lugar de los recursos principales. [Más información](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/adjusting-image.html).
- Cree Destinatarios de zoom en las imágenes para mostrar sus resaltados. [Más información](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html).

## Recortar

Dynamic Media Classic cuenta con algunas herramientas de edición de imágenes que están disponibles en la interfaz de usuario, incluida la herramienta Recortar. Es posible que desee recortar la imagen principal dentro de Dynamic Media Classic por varios motivos. Por ejemplo:

- No tiene acceso al archivo original. Desea mostrar la imagen con una proporción de aspecto o recorte diferente, pero no tiene el archivo original en el equipo o está trabajando desde casa. En este caso, puede ir a Dynamic Media Classic, buscar la imagen, recortarla y guardarla, o guardarla como una nueva versión.
- Para eliminar el exceso de espacio en blanco. La imagen fue fotografiada con demasiado espacio en blanco, lo que hace que el producto parezca pequeño. Desea que las imágenes en miniatura rellenen el lienzo todo lo posible.
- Para crear imágenes ajustadas, copie las imágenes virtuales que no tienen espacio en disco. Algunas compañías tienen reglas comerciales que les obligan a guardar copias independientes de la misma imagen, pero con un nombre diferente. O quizás quiera una versión recortada y sin recortar de la misma imagen.
- Para crear nuevas imágenes a partir de una imagen de origen. Por ejemplo, puede que desee crear muestras de color o un detalle de la imagen principal. Puede hacerlo en Adobe Photoshop y cargarlo por separado o utilizar la herramienta Recortar en Dynamic Media Classic.

>[!NOTE]
>
>Todas las direcciones URL de los siguientes debates sobre el recorte tienen fines ilustrativos únicamente; no son vínculos activos.

### Uso de la herramienta Recortar

Puede acceder a la herramienta Recortar en Dynamic Media Classic desde la página Detalles de un recurso o haciendo clic en el botón **Editar**. Puede utilizar la herramienta para recortar de dos formas:

- Modo de recorte predeterminado en el que se arrastran los controladores de la ventana de recorte o se escriben valores en el cuadro Tamaño. Aprenda a [Recortar manualmente](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop).
- Recortar. Utilice este parámetro para eliminar espacios en blanco adicionales alrededor de la imagen calculando el número de píxeles que no coinciden con la imagen. Obtenga información sobre cómo [Recortar](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#crop-to-remove-white-space-around-an-image) recortar.

### _Recorte manual_

Al guardar una versión recortada manualmente, parece que la imagen se recorta de forma permanente; En realidad, Dynamic Media Classic está ocultando los píxeles agregando un modificador de URL interno para recortar la imagen. Cuando publique, todos tendrán la impresión de que la imagen se recorta, aunque podrá volver al Editor de recorte y quitarlo posteriormente.

A continuación, puede elegir si desea guardar como imagen principal nueva o como Vista adicional del patrón. Un nuevo patrón es un nuevo archivo físico (como un TIFF o JPEG) que ocupa espacio de almacenamiento. Una vista adicional es una imagen virtual que no ocupa espacio en el servidor. No recomendamos que elija Reemplazar original, ya que esto sobrescribirá al patrón y hará que el recorte sea permanente. Si guarda como un nuevo maestro o como una vista adicional, debe elegir un nuevo ID de recurso. Al igual que otros ID de recurso, este debe ser un nombre único en Dynamic Media Classic.

### _Recortar recorte_

Si carga una imagen con demasiado espacio en blanco (lienzo adicional) alrededor del asunto principal de la imagen, tendrá un aspecto mucho más pequeño en la web al cambiar el tamaño. Esto es especialmente cierto para las imágenes en miniatura que tienen 150 píxeles o menos — el sujeto de la foto puede perderse en todo el espacio adicional que la rodea.

Compare estas dos versiones de la misma imagen.

![image](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

La imagen de la derecha se hace mucho más prominente eliminando el espacio adicional alrededor del producto. El recorte se puede realizar de una en una imagen, con la herramienta Recortar o como un proceso por lotes a medida que se carga. Recomendamos que se ejecute como un proceso por lotes si desea que todas las imágenes se recorten de forma consistente hasta los límites del asunto principal. Recortar cultivos hasta el cuadro delimitador — el rectángulo que rodea la imagen.

>[!NOTE]
>
>El recorte no crea transparencia alrededor de la imagen. Para ello, debe incrustar una ruta de recorte en la imagen y utilizar la opción de carga **Crear máscara a partir de ruta de recorte**.
>
>Además, para restaurar una imagen a su estado original después de recortarla cuando haya utilizado la opción **Guardar**, muestre la imagen en la pantalla Editor de recorte y seleccione el botón **Restaurar**.

### _Recorte en la carga_

Como se mencionó anteriormente, también puede elegir recortar las imágenes al cargarlas. Para utilizar recortes al cargar, haga clic en el botón **Opciones de trabajo** y, en Opciones de recorte, elija **Recortar**.

Dynamic Media Classic recordará esta opción para la siguiente carga. Si bien es posible que desee recortar imágenes para esta carga, es posible que no desee recortarlas para cada carga. Otra opción sería establecer un trabajo de carga de FTP programado especial y colocar las opciones de recorte allí. De este modo, solo ejecutaría el trabajo cuando necesitara recortar las imágenes.

>[!IMPORTANT]
>
>Si establece un recorte para la carga, Dynamic Media Classic colocará una cookie para recordar ese ajuste la próxima vez. Como práctica recomendada, haga clic en el botón **Restablecer valores predeterminados de Compañía** antes de la siguiente carga para eliminar las opciones de recorte que quedan en la última carga; de lo contrario, podría recortar accidentalmente el siguiente lote de imágenes.

### Recorte por dirección URL

Aunque no es obvio en Dynamic Media Classic, también puede recortar únicamente a través de la URL (o incluso agregar el recorte a un ajuste preestablecido de imagen).

Siempre que utilice la herramienta Recortar, verá los valores de URL en el campo de la parte inferior. Puede tomar esos valores y aplicarlos directamente a una imagen como modificadores de URL.

![Modificadores de comando ](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_imageCrop en la parte inferior del Editor de recorte_

![image](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

Dado que el tamaño debe calcularse por imagen cuando se recorta el recorte, no se puede automatizar mediante la dirección URL. El recorte solo se puede ejecutar durante la carga o aplicándolo de una imagen a la vez.

### _Recorte del ajuste preestablecido de imagen_

Los ajustes preestablecidos de imagen tienen un campo en el que puede agregar comandos de servicio de imágenes adicionales. Para añadir el mismo recorte que el anterior al ajuste preestablecido de imagen, edite el ajuste preestablecido y pegue o escriba los valores en el campo Modificadores de URL y, a continuación, guarde y publique.

![](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_imageAgregue comandos de recorte (o cualquier comando) a los modificadores de URL del ajuste preestablecido de imagen._

El recorte ahora formará parte de ese ajuste preestablecido de imagen y se aplicará automáticamente cada vez que se utilice. Por supuesto, este método depende de todas las imágenes que necesiten la misma cantidad de recorte. Si no todas las imágenes se graban de la misma manera, este método no funcionaría para usted.

## Imágenes ajustadas

Al utilizar la herramienta Recortar, tiene la opción de **Guardar como Vista adicional del patrón**. Cuando se guarda, se crea un nuevo tipo de recurso de Dynamic Media Classic: una imagen ajustada. Una imagen ajustada, también denominada derivado, es una imagen virtual. En realidad no es una imagen en absoluto; es una referencia de base de datos (como un alias o un método abreviado) a la imagen principal física.

### ¿Se levantará la imagen real por favor`?`

¿Puede saber cuál es el patrón y cuál es la imagen ajustada?

![image](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

No debería poder saberlo sin mirar en Dynamic Media Classic y ver el tipo de recurso de &quot;Imagen ajustada&quot; para SBR_MAIN2.

Una imagen ajustada no utiliza espacio en disco, ya que sólo existe como elemento de línea en la base de datos. También está permanentemente vinculado al activo original; si se elimina el original, también se eliminará la imagen ajustada. Puede consistir en una imagen entera sin recortar o en una parte de una imagen (un recorte).

![image](assets/crop-adjusted-zoom-targets/adjusted-image.png)

Normalmente, las imágenes ajustadas se crean con la herramienta Recortar; sin embargo, también se pueden crear con otros editores de imágenes — las herramientas Ajustar y Enfocar.

Las imágenes ajustadas requieren un ID de recurso único. Cuando se publican (debe realizar la publicación como cualquier otro recurso), actúan como cualquier otra imagen y su ID de recurso les llama en una URL. En la página Detalle, puede realizar la vista de imágenes ajustadas asociadas a una imagen principal en la ficha **Creación y derivados**.

![vistas ](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_de imagen ajustadas para la imagen principal ASIAN_BR_MAIN_

## Destinatarios de zoom

Los Destinatarios de zoom también se encuentran en el menú **Editar** y en la página **Detalles** de una imagen. Permiten configurar &quot;zonas interactivas&quot; para resaltar las características de comercialización específicas de una imagen de zoom. En lugar de crear imágenes independientes al recortar un patrón grande, el visor de zoom puede proporcionar los detalles sobre la imagen, junto con una etiqueta corta que se crea.

![image](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Dado que los Destinatarios de zoom son esencialmente una característica de comercialización y requieren conocer los puntos de venta de un producto, normalmente los crearía una persona del equipo de mercadotecnia o producto de una compañía.

El proceso es muy fácil — haga clic en la función, asígnele un nombre descriptivo y guárdelo. Los destinatarios se pueden copiar de una imagen a otra si son similares, pero el proceso es manual. En Dynamic Media Classic no hay forma de automatizar la creación de Destinatarios de zoom, ya que cada imagen es diferente y tiene diferentes funciones.

Otro factor a la hora de decidir si se utilizan Destinatarios de zoom es la elección del visor. No todos los tipos de visor pueden mostrar Destinatarios de zoom (por ejemplo, el visor de salida rápida no los admite).

Obtenga información sobre cómo [crear Destinatarios de zoom](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html#creating-and-editing-zoom-targets).

![image](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Uso de la herramienta Destinatario de zoom

Este es el flujo de trabajo para crear destinatarios en Dynamic Media Classic.

1. Vaya a la imagen, haga clic en el botón **Editar** y elija **Destinatarios de zoom**.
2. Se cargará el Editor de Destinatarios de zoom. Verá la imagen en el centro, algunos botones en la parte superior y un panel de destinatario vacío en la derecha. En la parte inferior izquierda, verá un ajuste preestablecido de visor seleccionado. El valor predeterminado es &quot;Zoom1-Guided&quot;.
3. Mueva el cuadro rojo con el ratón y haga clic para crear un nuevo destinatario.

   - El cuadro rojo es el área del destinatario. Cuando un usuario hace clic en ese destinatario, se acercará al área dentro del cuadro.
   - El tamaño del destinatario viene determinado por el tamaño de vista dentro del ajuste preestablecido de visor. Esto determina el tamaño de la imagen de zoom principal. Consulte _Configuración del tamaño de la Vista_ más adelante.

4. Verás el destinatario que acabas de crear de color azul, y a la derecha verás una versión en miniatura de ese destinatario, así como el nombre predeterminado &quot;destinatario-0&quot;.
5. Para cambiar el nombre del destinatario, haga clic en su miniatura, escriba un nuevo **Nombre** y haga clic en **Entrar** o **Tab** — si sólo hace clic en él, su nombre no se guardará.
6. Mientras el destinatario está seleccionado, el cuadro tendrá líneas discontinuas verdes alrededor de él, y puede cambiar su tamaño y moverlo. Arrastre las esquinas para cambiar el tamaño o arrastre el cuadro de destinatario para moverlo.

   - Esto cargará la imagen dentro del visor de zoom personalizado predeterminado. Asegúrese de que el ajuste preestablecido de visor admite Destinatarios de zoom — en general, todos los ajustes preestablecidos estándar que tienen la palabra &quot;-Guided&quot; se han diseñado para utilizarse con Destinatarios de zoom. Para utilizar los destinatarios, coloque el puntero sobre la miniatura del destinatario (o icono de zona interactiva) para ver la etiqueta y haga clic en ella para ver cómo el visor se acerca a esa función.
   - Al igual que cualquier otro trabajo que realice en Dynamic Media Classic, debe publicar sus Destinatarios de zoom para que estén activos en la web. Si ya está utilizando un visor que admite destinatarios, aparecerán inmediatamente (una vez que se borre la caché). Sin embargo, si no utiliza un visor con Destinatario de zoom habilitado, permanecerán ocultos.

      ![image](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. Además, si necesita eliminar un destinatario, selecciónelo haciendo clic en su miniatura y presione el botón **Eliminar Destinatario** o presione la tecla DELETE del teclado.
8. Siga haciendo clic para agregar nuevos destinatarios, cambiar el nombre o cambiar el tamaño después de agregarlos.
9. Cuando haya terminado, haga clic en el botón **Guardar** y, a continuación, **Previsualización**.

### Configuración del tamaño de Vista en el ajuste preestablecido de visor de zoom

Hablemos un momento de dónde viene el tamaño de los Destinatarios de zoom. Dentro del ajuste preestablecido de visor para el visor de zoom hay un ajuste denominado tamaño de vista. El tamaño de vista es el tamaño de la imagen de zoom dentro del visor. Es diferente del tamaño del escenario, que es el tamaño total del visor, incluidos los componentes de la interfaz de usuario y la ilustración.

Cuando se crea un nuevo destinatario, se obtiene su tamaño y proporción de aspecto del tamaño de la vista. Por ejemplo, si el tamaño de la vista es 200 x 200, solo podrá realizar destinatarios cuadrados, con un área de zoom máxima de 200 píxeles. Los destinatarios pueden ser mayores que 200 píxeles, pero siempre cuadrados. Pero esto también significa que la imagen dentro del visor de zoom es de sólo 200 píxeles — el tamaño del destinatario de zoom tiene una relación directa con el tamaño del visor. Por lo tanto, primero debe decidir el diseño del visor antes de establecer los destinatarios.

Sin embargo, de forma predeterminada, el tamaño de vista está en blanco (definido en 0 x 0), ya que el tamaño de la imagen de vista principal es dinámico y se deriva automáticamente según el tamaño del escenario. El problema es que si no establece explícitamente un tamaño de vista en el ajuste preestablecido, la herramienta de Destinatario de zoom no sabrá qué tamaño debe tener el destinatario.

Al cargar la herramienta Destinatario de zoom, se muestra el tamaño de vista junto al nombre del ajuste preestablecido. Compare el tamaño de vista entre el ajuste preestablecido integrado Zoom1-Guided y el ajuste preestablecido personalizado ZT_AUTHORING.

![image](assets/crop-adjusted-zoom-targets/view-size.jpg)

Se puede ver que el ajuste preestablecido integrado tiene un tamaño de 900 x 550, lo que significa que el destinatario nunca puede ser más pequeño que ese tamaño bastante grande. Probablemente sea demasiado grande — si tiene una imagen de 2000 píxeles, solo puede llamar a una función con un mínimo de 900 píxeles de ancho. El usuario puede hacer zoom manualmente, pero no puede guiarlo más de cerca. Si se establece un tamaño de vista de 350 x 350, los destinatarios pueden acercarse bastante o ampliarse. Pero si desea una imagen de zoom más grande en el visor, deberá crear un nuevo ajuste preestablecido porque el suyo está bloqueado en 350 píxeles.

### Creación o edición de un ajuste preestablecido de visor compatible con Destinatarios de zoom

Para definir el tamaño de la vista, cree o edite un ajuste preestablecido de visor que admita Destinatarios de zoom.

1. En Ajustes preestablecidos de visor, vaya a la opción **Configuración de zoom**.
2. Defina la anchura y la altura.
3. Guarde el ajuste preestablecido y cierre. Si desea utilizar ese ajuste preestablecido en el sitio de lanzamiento, tendrá que publicarlo posteriormente.
4. Vaya a la herramienta Destinatario de zoom y elija el ajuste preestablecido que ha editado en la parte inferior izquierda. Verá inmediatamente el nuevo tamaño de vista reflejado en sus destinatarios.
