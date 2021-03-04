---
title: Recorte, imágenes ajustadas y destinos de zoom
description: La imagen maestra de Dynamic Media Classic permite crear versiones recortadas independientes de cada imagen para mostrar detalles o muestras sin tener que crear versiones recortadas independientes de cada imagen. Obtenga información sobre cómo recortar imágenes en Dynamic Media Classic y guardarlas como un nuevo archivo maestro o una imagen virtual, guardar imágenes ajustadas virtuales y utilizarlas en lugar de recursos maestros, y crear destinos de zoom en las imágenes para mostrar detalles resaltados.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Administración de contenido
role: Profesional empresarial
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2673'
ht-degree: 0%

---


# Recorte, imágenes ajustadas y destinos de zoom {#crop-adjusted-zoom-targets}

Una de las principales ventajas del concepto de imagen maestra de Dynamic Media Classic es que puede reutilizar el recurso de imagen para muchos usos. Tradicionalmente, tendría que crear versiones separadas recortadas de cada imagen para mostrar detalles o muestras. Al utilizar Dynamic Media Classic, puede realizar las mismas tareas en el maestro único y guardar las versiones recortadas como nuevos archivos físicos o como derivados virtuales que no necesitan espacio de almacenamiento.

Al final de esta sección del tutorial, sabrá cómo:

- Recorte imágenes en Dynamic Media Classic y guárdelas como archivos maestros nuevos o como imágenes virtuales. [Más información](https://docs.adobe.com/help/en/dynamic-media-classic/using/master-files/cropping-image.html).
- Guarde las imágenes virtuales ajustadas y utilícelas en lugar de los recursos maestros. [Más información](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/adjusting-image.html).
- Cree destinos de zoom en las imágenes para mostrar sus aspectos destacados. [Más información](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html).

## Recortar

Dynamic Media Classic cuenta con algunas herramientas de edición de imágenes disponibles en la interfaz de usuario, incluida la herramienta Recortar. Es posible que desee recortar la imagen principal en Dynamic Media Classic por varios motivos. Por ejemplo:

- No tiene acceso al archivo original. Desea mostrar la imagen con una relación de aspecto o recorte diferente, pero no tiene el archivo original en el equipo o está trabajando desde casa. En este caso, puede ir a Dynamic Media Classic, buscar la imagen, recortarla y guardarla, o guardarla como una nueva versión.
- Para eliminar el exceso de espacio en blanco. La imagen fue fotografiada con demasiado espacio en blanco, lo que hace que el producto parezca pequeño. Desea que las imágenes en miniatura llenen el lienzo lo más posible.
- Para crear imágenes ajustadas, copias virtuales de imágenes que no ocupan espacio en disco. Algunas empresas tienen reglas comerciales que les obligan a guardar copias separadas de la misma imagen, pero con un nombre diferente. O quizá quiera una versión recortada y sin recortar de la misma imagen.
- Para crear nuevas imágenes a partir de una imagen de origen. Por ejemplo, es posible que desee crear muestras de color o un detalle de la imagen principal. Puede hacerlo en Adobe Photoshop y cargarlo por separado o utilizar la herramienta Recortar en Dynamic Media Classic.

>[!NOTE]
>
>Todas las direcciones URL de los debates siguientes sobre el recorte tienen fines ilustrativos únicamente; no son vínculos en directo.

### Uso de la herramienta Recortar

Puede acceder a la herramienta Recortar en Dynamic Media Classic desde la página Detalles de un recurso o haciendo clic en el botón **Editar**. Puede utilizar la herramienta para recortar de dos formas:

- El modo de recorte predeterminado en el que se arrastran los controles de la ventana de recorte o se escriben valores en el cuadro Tamaño. Aprenda a [Recortar manualmente](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop).
- Recortar. Utilice esto para eliminar espacios en blanco adicionales alrededor de la imagen calculando el número de píxeles que no coinciden con la imagen. Obtenga información sobre cómo [Recortar recortando](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#crop-to-remove-white-space-around-an-image).

### _Recorte manual_

Cuando guarda una versión recortada manualmente, parece que la imagen se recorta de forma permanente; Dynamic Media Classic oculta los píxeles añadiendo un modificador de URL interno para recortar la imagen. Cuando publique, todos verán que la imagen está recortada, aunque puede volver al Editor de recorte y quitarla posteriormente.

A continuación, puede elegir si desea guardar como imagen maestra nueva o como vista adicional del patrón. Un nuevo maestro es un nuevo archivo físico (como un TIFF o JPEG) que ocupa espacio de almacenamiento. Una vista adicional es una imagen virtual que no ocupa espacio en el servidor. No recomendamos que elija Reemplazar original, ya que esto sobrescribirá al maestro y hará que el recorte sea permanente. Si guarda como un nuevo maestro o como una vista adicional, debe elegir un nuevo ID de recurso. Al igual que otros ID de recurso, debe ser un nombre único en Dynamic Media Classic.

### _Recortar recorte_

Si carga una imagen con demasiados espacios en blanco (lienzo adicional) alrededor del asunto principal de la imagen, se verá mucho más pequeña en la web al cambiar su tamaño. Esto es especialmente cierto en el caso de las imágenes en miniatura que son de 150 píxeles o más pequeñas — el tema de la foto puede perderse en todo el espacio adicional que lo rodea.

Compare estas dos versiones de la misma imagen.

![image](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

La imagen de la derecha es mucho más prominente al eliminar el espacio adicional alrededor del producto. El recorte se puede realizar de una imagen a la vez, mediante la herramienta Recortar o ejecutar como un proceso por lotes mientras se carga. Se recomienda ejecutar como un proceso por lotes si desea que todas las imágenes se recorten de forma consistente hasta los límites del asunto principal. Recortar recortes hasta el cuadro delimitador: el rectángulo que rodea la imagen.

>[!NOTE]
>
>El recorte no crea transparencia alrededor de la imagen. Para ello, debe incrustar una ruta de recorte en la imagen y utilizar la opción de carga **Create Mask from Clip Path**.
>
>Además, para restaurar una imagen a su estado original después de recortarla al utilizar la opción **Guardar**, muestre la imagen en la pantalla Editor de recorte y seleccione el botón **Restaurar**.

### _Recorte al cargar_

Como se ha mencionado anteriormente, también puede elegir recortar las imágenes a medida que las carga. Para utilizar recortes al cargar, haga clic en el botón **Opciones de trabajo** y, en Opciones de recorte, seleccione **Recortar**.

Dynamic Media Classic recordará esta opción para la siguiente carga. Aunque es posible que desee recortar imágenes para esta carga, es posible que no desee recortarlas para cada carga. Otra opción sería configurar un trabajo de carga de FTP programado especial y colocar las opciones de recorte allí. De esta forma, solo ejecutaría el trabajo cuando necesitara recortar sus imágenes.

>[!IMPORTANT]
>
>Si establece un recorte para la carga, Dynamic Media Classic colocará una cookie para recordar esa configuración la próxima vez. Como práctica recomendada, haga clic en el botón **Reset to Company Defaults** antes de la siguiente carga para eliminar las opciones de recorte que queden en la última carga; de lo contrario, podría recortar accidentalmente el siguiente lote de imágenes.

### Recorte por dirección URL

Aunque no es obvio en Dynamic Media Classic, también puede recortar únicamente a través de la URL (o incluso agregar recorte a un ajuste preestablecido de imagen).

Siempre que utilice la herramienta Recortar, verá los valores de URL en el campo de la parte inferior. Puede tomar esos valores y aplicarlos directamente a una imagen como modificadores de URL.

![](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_modificadores del comando imageCrop en la parte inferior del Editor de recorte_

![image](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

Dado que el tamaño debe calcularse según la imagen cuando se utiliza el recorte mediante recorte, no se puede automatizar mediante la dirección URL. Recortar recorte solo se puede ejecutar en la carga o aplicándolo de una imagen a la vez.

### _Recorte en el ajuste preestablecido de imagen_

Los ajustes preestablecidos de imagen tienen un campo en el que puede añadir comandos adicionales de servicio de imágenes. Para agregar el mismo recorte que el anterior al ajuste preestablecido de imagen, edite el ajuste preestablecido, pegue o escriba los valores en el campo Modificadores de URL y, a continuación, guarde y publique.

![](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_imageAgregue comandos de recorte (o cualquier comando) a los modificadores de URL del ajuste preestablecido de imagen._

El recorte ahora forma parte de ese ajuste preestablecido de imagen y se aplica automáticamente cada vez que se utiliza. Por supuesto, este método depende de todas las imágenes que necesiten la misma cantidad de recorte. Si no todas las imágenes se filman de la misma manera, este método no funcionaría para usted.

## Imágenes ajustadas

Cuando utiliza la herramienta Recortar, tiene la opción de **Guardar como vista adicional del patrón**. Cuando se guarda, se crea un nuevo tipo de recurso de Dynamic Media Classic, una imagen ajustada. Una imagen ajustada, también denominada derivada, es una imagen virtual. En realidad no es una imagen en absoluto; es una referencia de base de datos (como un alias o acceso directo) a la imagen maestra física.

### ¿Se pondrá en pie la imagen real`?`

¿Puede saber cuál es el maestro y cuál es la imagen ajustada?

![image](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

No debería poder decirlo sin mirar en Dynamic Media Classic y ver el tipo de recurso &quot;Imagen ajustada&quot; para SBR_MAIN2.

Una imagen ajustada no utiliza espacio en disco, ya que solo existe como elemento de línea en la base de datos. También está permanentemente vinculado al activo original; si se elimina el original, también se eliminará la imagen ajustada. Puede consistir en una imagen completa sin recortar o en una parte de una imagen (un recorte).

![image](assets/crop-adjusted-zoom-targets/adjusted-image.png)

Normalmente se crean imágenes ajustadas con la herramienta Recortar; sin embargo, también se pueden crear con otros editores de imágenes: las herramientas Ajustar y Enfocar .

Las imágenes ajustadas requieren un ID de recurso único. Cuando se publican (debe publicar como cualquier otro recurso), actúan como cualquier otra imagen y su ID de recurso los llama en una URL. En la página Detalle, puede ver las imágenes ajustadas asociadas a una imagen maestra en la pestaña **Compilación y derivados**.

![](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_Vistas ajustadas de imagen para la imagen maestra ASIAN_BR_MAIN_

## Destinos de zoom

Los destinos de zoom también se encuentran en el menú **Edit** y en la página **Details** de una imagen. Permiten establecer &quot;zonas interactivas&quot; para resaltar características específicas de comercialización de una imagen de zoom. En lugar de crear imágenes independientes recortando un gran patrón, el visor de zoom puede mostrar los detalles sobre la imagen, junto con una etiqueta corta que usted cree.

![image](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Debido a que los destinos de zoom son esencialmente una característica de comercialización y requieren conocer los puntos de venta de un producto, normalmente los crearía una persona del equipo de comercialización o producto de una empresa.

El proceso es muy sencillo: haga clic en la función, asígnele un nombre descriptivo y guárdelo. Los objetivos se pueden copiar de una imagen a otra si son similares, aunque el proceso es manual. No hay forma en Dynamic Media Classic de automatizar la creación de destinos de zoom, ya que cada imagen es diferente y tiene diferentes funciones.

Otro factor a la hora de decidir si usar destinos de zoom es su elección de visor. No todos los tipos de visores pueden mostrar Destinos de zoom (por ejemplo, el visor de salida no los admite).

Obtenga información sobre cómo [Crear destinos de zoom](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html#creating-and-editing-zoom-targets).

![image](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Uso de la herramienta Destino de zoom

Este es el flujo de trabajo para crear destinos en Dynamic Media Classic.

1. Vaya a la imagen, haga clic en el botón **Edit** y seleccione **Zoom Targets**.
2. Se cargará el Editor de destino de zoom. Verá la imagen en el centro, algunos botones en la parte superior y un panel de destino vacío en la derecha. En la parte inferior izquierda, verá un ajuste preestablecido de visor seleccionado. El valor predeterminado es &quot;Zoom1-Guided&quot;.
3. Mueva el cuadro rojo con el ratón y haga clic en para crear un nuevo destino.

   - El cuadro rojo es el área objetivo. Cuando un usuario hace clic en ese destino, se amplía el área dentro del cuadro.
   - El tamaño objetivo viene determinado por el tamaño de vista dentro del ajuste preestablecido de visor. Esto determina el tamaño de la imagen de zoom principal. Consulte _Configuración del tamaño de vista_ a continuación.

4. Verá que el destino que acaba de crear se vuelve azul, y a la derecha verá una versión en miniatura de ese destino, así como el nombre predeterminado &quot;target-0&quot;.
5. Para cambiar el nombre del objetivo, haga clic en su miniatura, escriba un nuevo **Name** y haga clic en **Enter** o **Tab**; si hace clic en el botón, no se guardará el nombre.
6. Mientras se selecciona el objetivo, el cuadro tendrá líneas discontinuas verdes alrededor de él y puede cambiar el tamaño y moverlo. Arrastre las esquinas para cambiar el tamaño o arrastre el cuadro de destino para moverlo.

   - Esto cargará la imagen dentro del visor de zoom personalizado predeterminado. Asegúrese de que el ajuste preestablecido de visor sea compatible con los destinos de zoom; en general, todos los ajustes preestablecidos estándar que tienen la palabra &quot;-Guided&quot; se han diseñado para utilizarse con los destinos de zoom. Para utilizar los objetivos, pase el ratón sobre la miniatura de destino (o el icono de zona interactiva) para ver la etiqueta y haga clic en ella para ver cómo el visor amplía esa función.
   - Al igual que el resto del trabajo que realiza en Dynamic Media Classic, debe publicar para que los destinos de zoom estén activos en la web. Si ya utiliza un visor que admita los objetivos, estos aparecerán inmediatamente (una vez que se borre la caché). Sin embargo, si no utiliza un visor con la opción Destino de zoom habilitada, permanecerán ocultos.

      ![image](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. Además, si necesita eliminar un destinatario, selecciónelo haciendo clic en su miniatura y presione el botón **Delete Target** o presione la tecla SUPR en el teclado.
8. Siga haciendo clic en para agregar nuevos destinos, cambiar el nombre o cambiar el tamaño después de agregarlos.
9. Cuando haya terminado, haga clic en el botón **Save** y, a continuación, en **Preview**.

### Configuración del tamaño de vista en el ajuste preestablecido del visor de zoom

Hablemos un momento de dónde viene el tamaño de los objetivos de zoom. Dentro del ajuste preestablecido de visor para el visor de zoom hay un ajuste denominado tamaño de vista. El tamaño de la vista es el tamaño de la imagen de zoom dentro del visor. Es diferente del tamaño del escenario, que es el tamaño total del visor, incluidos los componentes de la interfaz de usuario y la ilustración.

Al crear un nuevo destino, obtiene su tamaño y relación de aspecto del tamaño de la vista. Por ejemplo, si el tamaño de la vista es de 200 x 200, solo podrá realizar objetivos cuadrados, con un área de zoom máxima de 200 píxeles. Los objetivos pueden ser más grandes que 200 píxeles, pero siempre cuadrados. Pero esto también significa que la imagen dentro del visor de zoom es de solo 200 píxeles, el tamaño del objetivo de zoom tiene una relación directa con el tamaño del visor. Por lo tanto, primero debe decidir el diseño del visor antes de establecer los objetivos.

Sin embargo, de forma predeterminada, el tamaño de la vista está en blanco (establecido en 0 x 0), ya que el tamaño de la imagen de la vista principal es dinámico y se deriva automáticamente según el tamaño del escenario. El problema es que si no establece explícitamente un tamaño de vista en el ajuste preestablecido, la herramienta Zoom Target no sabrá qué tamaño hacer los objetivos.

Al cargar la herramienta Destino de zoom, el tamaño de la vista se muestra junto al nombre del ajuste preestablecido. Compare el tamaño de la vista entre el ajuste preestablecido integrado Guiado por Zoom1 y el ajuste preestablecido personalizado ZT_AUTHORING.

![image](assets/crop-adjusted-zoom-targets/view-size.jpg)

Se puede ver que el ajuste preestablecido integrado tiene un tamaño de 900 x 550, lo que significa que el objetivo nunca se puede reducir más que ese tamaño bastante grande. Probablemente sea demasiado grande. Si tiene una imagen de 2000 píxeles, solo puede llamar a una función que tenga un mínimo de 900 píxeles de ancho. El usuario puede hacer un zoom manual aún más, pero no puede guiarlos más de cerca. Si establece un tamaño de vista de 350 x 350, los objetivos pueden acercarse bastante o cambiar su tamaño. Pero si desea una imagen de zoom más grande en el visor, debe crear un nuevo ajuste preestablecido porque el suyo está bloqueado a 350 píxeles.

### Creación o edición de un ajuste preestablecido de visualizador compatible con destinos de zoom

Para definir el tamaño de la vista, cree o edite un ajuste preestablecido de visor que admita destinos de zoom.

1. En Ajustes preestablecidos de visor, vaya a la opción **Ajustes de zoom**.
2. Defina la anchura y la altura.
3. Guarde el ajuste preestablecido y cierre. Si desea utilizar ese ajuste preestablecido en el sitio de lanzamiento, más adelante también deberá publicarlo.
4. Vaya a la herramienta Destino de zoom y elija el ajuste preestablecido que ha editado en la parte inferior izquierda. Verá inmediatamente el nuevo tamaño de vista reflejado en sus objetivos.
