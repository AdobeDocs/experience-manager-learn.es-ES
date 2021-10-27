---
title: Tutorial de prácticas recomendadas de Dynamic Media Classic
description: Dynamic Media Classic es el centro en el que los clientes crean, crean y distribuyen contenido multimedia enriquecido. Este tutorial de prácticas recomendadas se ha creado para ayudar a los usuarios actuales y nuevos de Dynamic Media Classic a comprender mejor qué pueden hacer con esta potente solución de medios enriquecidos desde Adobe. En esta parte del tutorial, aprenderá qué es Dynamic Media Classic y obtendrá una breve visión de sus funcionalidades principales y de la interfaz de usuario.
sub-product: dynamic-media
doc-type: tutorial
topics: best-practices, development, authoring, configuring
audience: all
activity: develop, use
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
exl-id: 975b85af-ca6a-419e-ab2a-6e1781bfee4a
source-git-commit: eb669d1e2493d9b4a973314ab1323764920ba220
workflow-type: tm+mt
source-wordcount: '885'
ht-degree: 1%

---

# Tutorial de prácticas recomendadas de Dynamic Media Classic

Esta guía está diseñada para ayudar a los usuarios actuales y nuevos de Dynamic Media Classic a comprender mejor qué pueden hacer con su potente solución de medios enriquecidos desde el Adobe. Lo haremos de la siguiente manera:

- Le presentamos Dynamic Media Classic, describimos qué es y le proporcionamos una descripción general de sus funciones principales y de la interfaz de usuario (IU).
- Explicar el flujo de trabajo general Crear, Autor y Entregar que seguirá cuando trabaje con recursos en la solución.
- Analizando los elementos importantes que se deben configurar antes de saltar y utilizar la solución.
- Inmersión en el uso de varias de las funcionalidades principales de la solución.

En toda la guía, se proporcionan ejemplos, sugerencias y prácticas recomendadas. También explicaremos términos y conceptos importantes con los que debería estar familiarizado al trabajar con Dynamic Media Classic. Y cuando estén disponibles para un tema determinado, le indicaremos seminarios web relevantes, publicaciones en blogs y documentación en línea.

Esperamos que esta guía le proporcione la información necesaria para desbloquear un valor tremendo de su solución de Dynamic Media Classic. Para desplazarse con mayor facilidad por los capítulos de esta guía, haga clic en el icono de marcador que hay en la parte izquierda de la guía para ver su contenido.

## Información general sobre Dynamic Media Classic

Dynamic Media Classic es el centro en el que los clientes crean, crean y distribuyen contenido multimedia enriquecido. Dynamic Media Classic es un entorno de administración, publicación y servicio de medios enriquecido e integrado. Los medios enriquecidos se pueden entregar a todos los canales de marketing y ventas, incluidos la web, el material impreso, las campañas de correo electrónico, las aplicaciones web, los escritorios y los dispositivos.

El servicio de imágenes es quizás la función más usada de Dynamic Media Classic. De hecho, la mayoría de los clientes utilizan Dynamic Media Classic para mostrar todas las imágenes de sus sitios web, incluidas las imágenes para zoom o medios enriquecidos. Sin embargo, también se puede utilizar para muchos otros fines, como la entrega de vídeo y el uso de IA para optimizar las imágenes entregadas.

## Funcionalidades principales de Dynamic Media Classic

En esta guía analizaremos las siguientes funcionalidades principales de Dynamic Media Classic.

- **Imágenes dinámicas.** Término general para la edición, el formato y el tamaño en tiempo real, y zoom y panorámica interactivos; cambio de color y textura; giro de 360 grados; plantillas de imagen; y visualizadores multimedia.
- **Vídeo.** Cargue vídeos finales, publíquelos y descargue progresivamente los mismos en visualizadores de vídeo configurables.
- **Imágenes inteligentes.** Tecnología que aprovecha las capacidades de Adobe Sensei AI y funciona con los &quot;ajustes preestablecidos de imagen&quot; existentes para mejorar el rendimiento de la entrega de imágenes optimizando automáticamente el formato, el tamaño y la calidad de las imágenes en función de las capacidades del navegador del cliente.

Para descubrir funciones adicionales de la solución, visite [Documentación para Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/intro/introduction.html).

## Interfaz de usuario (IU) de Dynamic Media Classic

La interfaz de usuario principal de Dynamic Media Classic consta de tres áreas principales: la barra de navegación global, la biblioteca de recursos y el panel Examinar/crear.

![image](assets/overview/overview-dmc-ui-ew.png)

_Interfaz de usuario de Dynamic Media Classic_

**Barra de navegación global.** Situado en la parte superior de la pantalla, utilizará los botones de esta barra para acceder a las áreas clave y las capacidades de la solución. Por ejemplo, lo usará para acceder a las funciones de carga, abrir varias áreas de creación de recursos (conjunto de imágenes, conjunto de giros, etc.), realizar tareas importantes como configurar ajustes preestablecidos de imagen y de visor, y publicar sus recursos. Desde aquí también puede supervisar sus trabajos, ver actividades recientes y elegir entre una variedad de opciones de ayuda.

**Biblioteca de recursos.** En la parte izquierda de la pantalla se encuentra la biblioteca de recursos, un panel que se utiliza para organizar los recursos en carpetas y subcarpetas que cree. En la parte superior del panel, encontrará filtros y búsquedas que le ayudarán a encontrar los recursos. La búsqueda avanzada permite realizar búsquedas especificando varias opciones como criterios para la búsqueda, incluidos los campos de metadatos ocultos adjuntos a ese recurso. En la parte inferior del panel, puede ver los elementos eliminados haciendo clic en el icono Papelera . Inicialmente, no se inicia con ninguna carpeta, excepto la carpeta de nivel superior, que tiene el mismo nombre que el nombre de la cuenta.

>[!NOTE]
>
>Los activos de la Papelera se eliminarán automáticamente de forma permanente siete días después de haberlos colocado, a menos que los restaure.

**Panel Examinar/Generar.** Este es el centro de la interfaz de usuario, donde podrá examinar los recursos en el modo Examinar o, si está en el modo Generar, los utilizará como lienzo para crear recursos como parte de un flujo de trabajo. Cuando inicie sesión por primera vez, verá el panel Examinar. En el centro de la pantalla se encuentran las versiones en miniatura de las imágenes en una vista de cuadrícula. Puede cambiar a una vista de lista o seleccionar un recurso y ver los detalles sobre él mediante la vista de detalles.

>[!IMPORTANT]
>
>Junto a cada ID de recurso se encuentra la variable **Marcar para publicación** . Cuando el botón de alternancia está activado (verde), indica que el recurso está marcado para su publicación.

![image](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>Seleccione el **Publicar después de la carga** en el cuadro de diálogo Cargar para publicar automáticamente los recursos al cargarlos.

Más información sobre [Navegación por la IU de Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/navigation-basics.html).
