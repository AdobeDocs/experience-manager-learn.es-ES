---
title: 'Capítulo 1: Conceptos, patrones y antipatrones de Dispatcher'
description: En este capítulo se proporciona una breve introducción sobre el historial y la mecánica de Dispatcher, y se analiza cómo influye esto en la forma en que un desarrollador de AEM diseña sus componentes.
feature: Dispatcher
topic: Architecture
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 3bdb6e36-4174-44b5-ba05-efbc870c3520
duration: 3855
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '17382'
ht-degree: 0%

---

# Capítulo 1: Conceptos, patrones y antipatrones de Dispatcher

## Información general

En este capítulo se proporciona una breve introducción sobre el historial y la mecánica de Dispatcher, y se analiza cómo influye esto en la forma en que un desarrollador de AEM diseña sus componentes.

## Por qué los desarrolladores deberían preocuparse por la infraestructura

Dispatcher es una parte esencial de la mayoría de las instalaciones de AEM, si no de todas. Puede encontrar muchos artículos en línea que tratan sobre cómo configurar Dispatcher, así como consejos y trucos.

Sin embargo, estos fragmentos de información siempre comienzan en un nivel muy técnico, suponiendo que ya sepa lo que desea hacer y, por lo tanto, solo proporcione detalles sobre cómo lograr lo que desea. Nunca hemos encontrado artículos conceptuales que describan _qué es y por qué_ en cuanto a lo que puede y no puede hacer con Dispatcher.

### Antipattern: Dispatcher as a Afterthink

Esta falta de información básica conduce a una serie de patrones antidiscriminación que hemos visto en una serie de proyectos de AEM:

1. Como Dispatcher está instalado en el servidor web Apache, es tarea de los &quot;dioses Unix&quot; del proyecto configurarlo. Un &quot;desarrollador de Java mortal&quot; no necesita preocuparse por ello.

2. El desarrollador de Java debe asegurarse de que su código funcione... Dispatcher lo hará más rápido por arte de magia. El despachante siempre es una idea tardía. Sin embargo, esto no funciona. Un desarrollador debe diseñar su código teniendo en cuenta al distribuidor. Y necesita conocer sus conceptos básicos para hacerlo.

### &quot;Primero haz que funcione, luego hazlo rápido&quot; No siempre es correcto

Es posible que haya escuchado el consejo de programación _&quot;Primero hágalo funcionar, luego hágalo rápido&quot;._. No es del todo erróneo. Sin embargo, sin el contexto adecuado, se tiende a malinterpretar y a no aplicarse correctamente.

El consejo debe evitar que el desarrollador optimice el código de forma prematura, ya que es posible que nunca se ejecute, o que se ejecute tan raramente, que una optimización no tenga el impacto suficiente para justificar el esfuerzo que se realiza en la optimización. Además, la optimización podría generar código más complejo e introducir errores. Por lo tanto, si es desarrollador, no invierta demasiado tiempo en microoptimizar cada línea de código. Asegúrese de elegir las estructuras de datos, los algoritmos y las bibliotecas adecuados y espere al análisis del punto interactivo de un generador de perfiles para ver dónde una optimización más completa podría aumentar el rendimiento general.

### Decisiones y artefactos arquitectónicos

Sin embargo, el consejo de &quot;primero que funcione, luego que sea rápido&quot; es totalmente erróneo cuando se trata de decisiones &quot;arquitectónicas&quot;. ¿Qué son las decisiones arquitectónicas? En pocas palabras, son las decisiones que son costosas, difíciles y/o imposibles de cambiar posteriormente. Tenga en cuenta que &quot;caro&quot; a veces es lo mismo que &quot;imposible&quot;.  Por ejemplo, cuando el proyecto se está quedando sin presupuesto, es imposible implementar cambios costosos. Los cambios de infraestructura a son el primer tipo de cambios en esa categoría que llegan a la mente de la mayoría de la gente. Pero también hay otro tipo de artefactos &quot;arquitectónicos&quot; que pueden volverse muy desagradables de cambiar:

1. Fragmentos de código en el &quot;centro&quot; de una aplicación, en los que se basan muchas otras partes. Para cambiarlas, es necesario cambiar todas las dependencias y volver a probarlas a la vez.

2. Artefactos, que están involucrados en algún escenario asincrónico, dependiente del tiempo donde la entrada - y por lo tanto el comportamiento del sistema puede variar muy aleatoriamente. Los cambios pueden tener efectos impredecibles y pueden ser difíciles de probar.

3. Patrones de software que se utilizan y reutilizan una y otra vez, en todas las piezas y partes del sistema. Si el patrón de software resulta ser subóptimo, es necesario volver a codificar todos los artefactos que utilizan el patrón.

¿Recuerdas? En la parte superior de esta página dijimos, que el Dispatcher es una parte esencial de una aplicación de AEM. El acceso a una aplicación web es muy aleatorio: los usuarios van y vienen en momentos impredecibles. Al final, todo el contenido se almacenará (o deberá almacenarse) en la caché de Dispatcher. Por lo tanto, si ha prestado mucha atención, es posible que se haya dado cuenta de que el almacenamiento en caché podría verse como un artefacto &quot;arquitectónico&quot; y, por lo tanto, debería ser entendido por todos los miembros del equipo, desarrolladores y administradores por igual.

No decimos que un desarrollador deba configurar realmente Dispatcher. Necesitan conocer los conceptos, especialmente los límites, para asegurarse de que Dispatcher también pueda aprovechar su código.

La Dispatcher no mejora mágicamente la velocidad del código. Un desarrollador debe crear estos componentes teniendo en cuenta Dispatcher. Por lo tanto, necesita saber cómo funciona.

## Almacenamiento en caché de Dispatcher: principios básicos

### Dispatcher como almacenamiento en caché Http: equilibrador de carga

¿Qué es el Dispatcher y por qué se llama &quot;Dispatcher&quot; en primer lugar?

La Dispatcher es

* En primer lugar, una caché

* Un proxy inverso

* Un módulo para el servidor web Apache httpd, que agrega funciones relacionadas con AEM a la versatilidad de Apache y funciona sin problemas junto con todos los demás módulos Apache (como SSL o incluso SSI incluye, como veremos más adelante)

En los primeros días de la web, se esperaría unos cientos de visitantes a un sitio. La configuración de un Dispatcher &quot;distribuyó&quot; o equilibró la carga de solicitudes a varios servidores de publicación de AEM y eso generalmente era suficiente, por lo tanto, el nombre &quot;Dispatcher&quot;. Sin embargo, en la actualidad ya no se utiliza con mucha frecuencia.

Más adelante en este artículo veremos diferentes formas de configurar Dispatcher y sistemas de publicación. En primer lugar, vamos a empezar con algunos conceptos básicos del almacenamiento en caché http.

![Funcionalidad básica de una caché de Dispatcher](assets/chapter-1/basic-functionality-dispatcher.png)

*Funcionalidad básica de una caché de Dispatcher*

<br> 

Los conceptos básicos de Dispatcher se explican aquí. Dispatcher es un proxy inverso de almacenamiento en caché simple con la capacidad de recibir y crear solicitudes HTTP. Un ciclo normal de solicitud/respuesta es el siguiente:

1. Un usuario solicita una página
2. El Dispatcher comprueba si ya tiene una versión procesada de esa página. Supongamos que es la primera solicitud de esta página y que Dispatcher no puede encontrar una copia en caché local.
3. Dispatcher solicita la página desde el sistema de publicación
4. En el sistema de publicación, la página se procesa mediante una plantilla JSP o HTL
5. La página se devuelve a Dispatcher
6. Dispatcher almacena en caché la página
7. Dispatcher devuelve la página al explorador
8. Si se solicita la misma página por segunda vez, se puede servir directamente desde la caché de Dispatcher sin necesidad de volver a procesarla en la instancia de publicación. Esto ahorra tiempo de espera para los ciclos de usuario y CPU en la instancia de publicación.

Estábamos hablando de &quot;páginas&quot; en la última sección. Pero el mismo esquema también se aplica a otros recursos como imágenes, archivos CSS, descargas de PDF, etc.

#### Cómo se almacenan en caché los datos

El módulo Dispatcher aprovecha las funciones que proporciona el servidor Apache de alojamiento. Los recursos como las páginas de HTML, las descargas y la imagen se almacenan como archivos simples en el sistema de archivos Apache. Es así de simple.

El nombre de archivo se deriva de la dirección URL del recurso solicitado. Si solicita un archivo `/foo/bar.html`, se almacena, por ejemplo, en /`var/cache/docroot/foo/bar.html`.

En principio, si todos los archivos se almacenan en caché y, por lo tanto, se almacenan estáticamente en Dispatcher, puede extraer el complemento del sistema de publicación y Dispatcher actuaría como un sencillo servidor web. Pero esto es solo para ilustrar el principio. La vida real es más complicada. No puede almacenar todo en caché, y la caché nunca está completamente &quot;llena&quot;, ya que el número de recursos puede ser infinito debido a la naturaleza dinámica del proceso de renderización. El modelo de un sistema de archivos estático ayuda a generar una imagen aproximada de las capacidades de Dispatcher. Y ayuda a explicar las limitaciones de Dispatcher.

#### Estructura de la URL de AEM y asignación del sistema de archivos

Para comprender Dispatcher con más detalle, volvamos a revisar la estructura de una URL de ejemplo simple.  Veamos el siguiente ejemplo...

`http://domain.com/path/to/resource/pagename.selectors.html/path/suffix.ext?parameter=value&otherparameter=value#fragment`

* `http` indica el protocolo

* `domain.com` es el nombre de dominio

* `path/to/resource` es la ruta en la que se almacena el recurso en CRX y, posteriormente, en el sistema de archivos del servidor Apache

A partir de aquí, las cosas difieren un poco entre el sistema de archivos AEM y el sistema de archivos Apache.

En AEM,

* `pagename` es la etiqueta de recursos

* `selectors` representa una serie de selectores utilizados en Sling para determinar cómo se procesa el recurso. Una dirección URL puede tener un número arbitrario de selectores. Están separados por un punto. Una sección de selectores podría ser, por ejemplo, algo así como &quot;french.mobile.fantasía&quot;. Los selectores solo deben contener letras, dígitos y guiones.

* `html`, como el último de los &quot;selectores&quot;, se denomina extensión. En AEM/Sling, también determina parcialmente el script de renderización.

* `path/suffix.ext` es una expresión de ruta de acceso que puede ser un sufijo a la dirección URL.  Se puede utilizar en scripts de AEM para controlar aún más cómo se procesa un recurso. Tendremos una sección completa sobre esta parte más adelante. Por ahora, debería ser suficiente con saber que puede utilizarlo como parámetro adicional. Los sufijos deben tener una extensión.

* `?parameter=value&otherparameter=value` es la sección de consulta de la dirección URL. Se utiliza para pasar parámetros arbitrarios a AEM. Las URL con parámetros no se pueden almacenar en caché y, por lo tanto, los parámetros deben limitarse a casos en los que sean absolutamente necesarios.

* `#fragment`, la parte de fragmento de una dirección URL no se pasa a AEM; solo se utiliza en el explorador; en los marcos de JavaScript como &quot;parámetros de enrutamiento&quot; o para saltar a una parte determinada de la página.

En Apache (*haga referencia al diagrama siguiente*),

* `pagename.selectors.html` se usa como nombre de archivo en el sistema de archivos de la caché.

Si la dirección URL tiene un sufijo `path/suffix.ext`,

* `pagename.selectors.html` se ha creado como una carpeta

* `path` una carpeta en la carpeta `pagename.selectors.html`

* `suffix.ext` es un archivo en la carpeta `path`. Nota: Si el sufijo no tiene una extensión, el archivo no se almacena en caché.

![Diseño del sistema de archivos después de obtener las direcciones URL del Dispatcher](assets/chapter-1/filesystem-layout-urls-from-dispatcher.png)

*Diseño del sistema de archivos después de obtener las direcciones URL del Dispatcher*

<br> 

#### Limitaciones básicas

La asignación entre una dirección URL, el recurso y el nombre de archivo es bastante sencilla.

Sin embargo, es posible que haya notado algunas trampas,

1. Las direcciones URL pueden llegar a ser muy largas. Añadir la parte &quot;path&quot; de `/docroot` en el sistema de archivos local podría superar fácilmente los límites de algunos sistemas de archivos. La ejecución de Dispatcher en NTFS en Windows puede ser un desafío. Sin embargo, está seguro con Linux.

2. Las direcciones URL pueden contener caracteres especiales y diéresis. Esto no suele ser un problema para Dispatcher. Sin embargo, tenga en cuenta que la dirección URL se interpreta en muchos lugares de la aplicación. La mayoría de las veces, hemos visto comportamientos extraños de una aplicación, solo para descubrir que un fragmento de código (personalizado) raramente utilizado no se probó a fondo para caracteres especiales. Deberías evitarlos si puedes. Y si no puedes, planea hacer pruebas exhaustivas.

3. En CRX, los recursos tienen subrecursos. Por ejemplo, una página tendrá una serie de páginas secundarias. Esto no puede coincidir en un sistema de archivos, ya que los sistemas de archivos tienen archivos o carpetas.

#### Las URL sin extensión no se almacenan en caché

Las direcciones URL siempre deben tener una extensión. Aunque puede ofrecer direcciones URL sin extensiones en AEM. Estas direcciones URL no se almacenarán en la caché de Dispatcher.

**Ejemplos**

`http://domain.com/home.html` es **almacenable en caché**

`http://domain.com/home` **no se puede almacenar en caché**

La misma regla se aplica cuando la dirección URL contiene un sufijo. El sufijo debe tener una extensión para poder almacenarse en caché.

**Ejemplos**

`http://domain.com/home.html/path/suffix.html` es **almacenable en caché**

`http://domain.com/home.html/path/suffix` **no se puede almacenar en caché**

Podría preguntarse qué sucede si la parte del recurso no tiene extensión, pero el sufijo sí la tiene. Bueno, en este caso la URL no tiene sufijo. Mire el siguiente ejemplo:

**Ejemplo**

`http://domain.com/home/path/suffix.ext`

`/home/path/suffix` es la ruta de acceso al recurso... por lo que no hay sufijo en la dirección URL.

**Conclusión**

Agregue siempre extensiones tanto a la ruta como al sufijo. Las personas con conocimiento de SEO a veces argumentan, que esto te está clasificando en los resultados de búsqueda. Pero una página sin caché sería muy lenta y se clasificaría aún más abajo.

#### URL de sufijos en conflicto

Imagine que tiene dos direcciones URL válidas

`http://domain.com/home.html`

y

`http://domain.com/home.html/suffix.html`

Son absolutamente válidas en AEM. No vería ningún problema en su equipo de desarrollo local (sin un Dispatcher). Lo más probable es que tampoco encuentre ningún problema en las pruebas de carga o UAT. El problema que enfrentamos es tan sutil que se desliza por la mayoría de las pruebas.  Esto le golpeará fuertemente cuando esté en las horas de mayor actividad y tenga un tiempo limitado para solucionarlo, probablemente no tenga acceso al servidor ni recursos para solucionarlo. Hemos estado allí...

Entonces... ¿cuál es el problema?

`home.html` en un sistema de archivos puede ser un archivo o una carpeta. No ambas al mismo tiempo que en AEM.

Si solicita `home.html` primero, se creará como un archivo.

Las solicitudes posteriores a `home.html/suffix.html` devuelven resultados válidos, pero como el archivo `home.html` &quot;bloquea&quot; la posición en el sistema de archivos, `home.html` no se puede crear por segunda vez como carpeta y, por lo tanto, `home.html/suffix.html` no se almacena en caché.

![Posición de bloqueo de archivos en el sistema de archivos que impide que se almacenen en caché los subrecursos](assets/chapter-1/file-blocking-position-in-filesystem.png)

*Posición de bloqueo de archivos en el sistema de archivos que impide que se almacenen en caché los subrecursos*

<br> 

Si lo hace al revés, primero debe solicitar `home.html/suffix.html` y después `suffix.html` se almacena en la caché en una carpeta `/home.html` al principio. Sin embargo, esta carpeta se elimina y se reemplaza por un archivo `home.html` cuando posteriormente se solicita `home.html` como recurso.

![Eliminando una estructura de ruta de acceso cuando se busca un recurso principal](assets/chapter-1/deleting-path-structure.png)

*Eliminando una estructura de ruta de acceso cuando se busca un recurso principal*

<br> 

Por lo tanto, el resultado de lo que se almacena en caché es completamente aleatorio y depende del orden de las solicitudes entrantes. Lo que hace que las cosas sean aún más complicadas es el hecho de que normalmente tiene más de un distribuidor. Además, el rendimiento, la tasa de aciertos de caché y el comportamiento pueden variar de un Dispatcher a otro. Si desea saber por qué su sitio web no responde, debe asegurarse de que está viendo el Dispatcher correcto con el desafortunado orden de almacenamiento en caché. Si estás buscando en el Dispatcher que - por suerte - tenía un patrón de solicitud más favorable, te perderás en tratar de encontrar el problema.

#### Evitar URL en conflicto

Puede evitar &quot;URL en conflicto&quot;, donde un nombre de carpeta y un nombre de archivo &quot;compiten&quot; por la misma ruta en el sistema de archivos, cuando utiliza una extensión diferente para el recurso y tiene un sufijo.

**Ejemplo**

* `http://domain.com/home.html`

* `http://domain.com/home.dir/suffix.html`

Ambos son perfectamente almacenables en caché,

![](assets/chapter-1/cacheable.png)

Elegir una extensión dedicada &quot;dir&quot; para un recurso cuando se solicita un sufijo o se evita el uso del sufijo por completo. Hay casos raros en los que son útiles. Y es fácil implementar estos casos correctamente.  Como veremos en el siguiente capítulo, cuando hablemos de invalidación y vaciado de caché.

#### Solicitudes no almacenables en caché

Revisemos un resumen rápido del último capítulo más algunas excepciones más. Dispatcher puede almacenar en caché una dirección URL si está configurada para que se pueda almacenar en caché y si es una solicitud de GET. No se puede almacenar en caché con una de las siguientes excepciones.

**Solicitudes en caché**

* La solicitud está configurada para que se pueda almacenar en caché en la configuración de Dispatcher
* La solicitud es una solicitud de GET sin formato

**Solicitudes o respuestas que no se pueden almacenar en caché**

* Solicitud que la configuración deniega el almacenamiento en caché (ruta, patrón, tipo MIME)
* Respuestas que devuelven un encabezado &quot;Dispatcher: sin caché&quot;
* Respuesta que devuelve un encabezado &quot;Cache-Control: no-cache|private&quot;
* Respuesta que devuelve un encabezado &quot;Pragma: sin caché&quot;
* Solicitud con parámetros de consulta
* URL sin extensión
* URL con un sufijo que no tiene extensión
* Respuesta que devuelve un código de estado distinto de 200
* Solicitud POST

## Invalidar y vaciar la caché

### Información general

El último capítulo enumeraba un gran número de excepciones, cuando Dispatcher no puede almacenar en caché una solicitud. Pero hay más cosas que considerar: Solo porque el Dispatcher _puede_ almacenar en caché una solicitud, no significa necesariamente que _deba_.

El punto es que el almacenamiento en caché suele ser fácil. El Dispatcher solo necesita almacenar el resultado de una respuesta y devolverlo la próxima vez que la misma solicitud sea entrante. ¿Cierto? ¡Equivocado!

La parte difícil es la _invalidación_ o el _vaciado_ de la caché. La Dispatcher debe averiguar cuándo ha cambiado un recurso y cuándo debe volver a procesarse.

Esto parece ser una tarea trivial a primera vista... pero no lo es. Lea más y descubrirá algunas diferencias complicadas entre recursos únicos y simples y páginas que dependen de una estructura con muchas mallas de varios recursos.

### Recursos simples y vaciado

Hemos configurado nuestro sistema de AEM para crear dinámicamente una representación de miniaturas para cada imagen cuando se solicite con un selector especial de &quot;miniatura&quot;:

`/content/dam/path/to/image.thumb.png`

Y, por supuesto, proporcionamos una URL para ofrecer la imagen original con una URL sin selector:

`/content/dam/path/to/image.png`

Si descargamos ambas, la miniatura y la imagen original, terminaremos con algo como,

```
/var/cache/dispatcher/docroot/content/dam/path/to/image.thumb.png

/var/cache/dispatcher/docroot/content/dam/path/to/image.png
```

en el sistema de archivos de Dispatcher.

Ahora el usuario carga y activa una nueva versión de ese archivo. Finalmente, se envía una solicitud de invalidación desde AEM a Dispatcher,

```
GET /invalidate
invalidate-path:  /content/dam/path/to/image

<no body>
```

La invalidación es así de fácil: una simple solicitud GET a una URL especial &quot;/invalidate&quot; en Dispatcher. No se requiere un cuerpo HTTP, la &quot;carga útil&quot; es solo el encabezado &quot;invalidate-path&quot;. Tenga en cuenta también que la ruta de invalidación en el encabezado es el recurso que AEM conoce, y no el archivo o archivos que Dispatcher ha almacenado en caché. AEM solo conoce los recursos. Las extensiones, selectores y sufijos se utilizan durante la ejecución cuando se solicita un recurso. AEM no lleva a cabo ninguna contabilidad sobre los selectores que se han utilizado en un recurso, por lo que la ruta del recurso es todo lo que sabe con seguridad al activar un recurso.

Esto es suficiente en nuestro caso. Si un recurso ha cambiado, podemos suponer con seguridad que todas las representaciones de ese recurso también han cambiado. En nuestro ejemplo, si la imagen ha cambiado, también se representa una nueva miniatura.

Dispatcher puede eliminar de forma segura el recurso con todas las representaciones que haya almacenado en caché. Hará algo como,

`$ rm /content/dam/path/to/image.*`

eliminando `image.png` y `image.thumb.png`, así como todas las demás representaciones que coincidan con ese patrón.

Muy simple, en efecto... siempre y cuando utilice un solo recurso para responder a una solicitud.

### Referencias y contenido con malla

#### El problema del contenido enlazado

A diferencia de las imágenes u otros archivos binarios cargados en AEM, las páginas de HTML no son animales solitarios. Viven en bandadas y están altamente interconectadas entre sí por hipervínculos y referencias. El vínculo simple es inofensivo, pero se vuelve complicado cuando hablamos de referencias de contenido. Los teasers de navegación superior ubicuos de las páginas son referencias de contenido.

#### Referencias de contenido y por qué son un problema

Veamos un ejemplo sencillo... Una agencia de viajes tiene una página web que promueve un viaje a Canadá. Esta promoción aparece en la sección de teaser de otras dos páginas, en la página de &quot;Inicio&quot; y en una página de &quot;Ofertas especiales de invierno&quot;.

Dado que ambas páginas muestran el mismo teaser, sería innecesario pedir al autor que cree el teaser varias veces para cada página en la que se debe mostrar. En su lugar, la página de destino &quot;Canadá&quot; reserva una sección en las propiedades de página para proporcionar la información del teaser o, mejor aún, para proporcionar una URL que represente por completo ese teaser:

`<sling:include resource="/content/home/destinations/canada" addSelectors="teaser" />`

o

`<sling:include resource="/content/home/destinations/canada/jcr:content/teaser" />`

![](assets/chapter-1/content-references.png)

En AEM solo funciona como un encanto, pero si utiliza un Dispatcher en la instancia de publicación, sucede algo extraño.

Imagine que ha publicado su sitio web. El título de la página de Canadá es &quot;Canadá&quot;. Cuando un visitante solicita su página de inicio (que tiene una referencia de teaser a esa página), el componente de la página &quot;Canadá&quot; representa algo como

```
<div class="teaser">
  <h3>Canada</h3>
  <img …>
</div>
```

*en* la página principal. Dispatcher almacena la página principal como un archivo .html estático, que incluye el teaser y su titular en el archivo.

Ahora el experto en marketing ha aprendido que los titulares de teaser deben ser procesables. Por lo tanto, decide cambiar el título de &quot;Canadá&quot; a &quot;Visitar Canadá&quot;, y actualiza la imagen también.

Publica la página editada &quot;Canada&quot; y vuelve a visitar la página principal publicada anteriormente para ver sus cambios. Pero - nada cambió allí. Sigue mostrando el teaser antiguo. Comprueba dos veces el &quot;Especial de Invierno&quot;. Esa página nunca se ha solicitado antes y, por lo tanto, no se almacena en caché de forma estática en Dispatcher. Por lo tanto, Publish representa recientemente esta página y esta página contiene ahora el nuevo teaser &quot;Visit Canada&quot;.

![Dispatcher almacena contenido incluido obsoleto en la página principal](assets/chapter-1/dispatcher-storing-stale-content.png)

*Dispatcher almacena contenido incluido obsoleto en la página principal*

<br> 

¿Qué pasó? Dispatcher almacena una versión estática de una página que contiene todo el contenido y el marcado que se ha extraído de otros recursos durante el procesamiento.

El Dispatcher, que es un servidor web basado en un sistema de archivos, es rápido pero también relativamente simple. Si un recurso incluido cambia, no se da cuenta de eso. Sigue adhiriéndose al contenido que había cuando se representó la página incluida.

La página &quot;Oferta especial de invierno&quot; aún no se ha procesado, por lo que no hay ninguna versión estática en Dispatcher y, por lo tanto, se muestra con el nuevo teaser, ya que se procesa recién a petición.

Podría pensar que Dispatcher haría un seguimiento de todos los recursos que toca mientras procesa y vacía todas las páginas que han utilizado este recurso, cuando ese recurso cambia. Sin embargo, Dispatcher no procesa las páginas. El sistema Publish realiza la representación. Dispatcher no sabe qué recursos van a un archivo .html procesado.

¿Todavía no estás convencido? Podría pensar *&quot;debe haber una forma de implementar algún tipo de seguimiento de dependencia&quot;*. Bueno, lo hay, o más exactamente, *era*. Comunicado 3 el tatarabuelo de AEM tenía un rastreador de dependencias implementado en la _sesión_ que se utilizó para procesar una página.

Durante una solicitud, cada recurso adquirido a través de esta sesión se seguía como una dependencia de la URL que se estaba representando actualmente.

Pero resultó, que hacer un seguimiento de las dependencias era muy costoso. La gente pronto descubrió que el sitio web es más rápido si desactivaban por completo la función de seguimiento de dependencias y dependían de volver a procesar todas las páginas html después de cambiar una página html. Es más, ese esquema tampoco era perfecto -había una serie de escollos y excepciones en el camino. En algunos casos, no utilizaba la sesión predeterminada de solicitudes para obtener un recurso, sino una sesión de administración para obtener algunos recursos de ayuda para procesar una solicitud. Estas dependencias generalmente no se rastreaban y provocaban dolores de cabeza y llamadas telefónicas al equipo de operaciones pidiendo vaciar manualmente la caché. Tuviste suerte si tenían un procedimiento estándar para hacer eso. Hubo más problemas en el camino, pero... dejemos de recordar. Esto nos lleva al 2005. En última instancia, esa función se desactivó en el Comunicado 4 de forma predeterminada y no volvió a entrar en el CQ5 sucesor, que luego se convirtió en AEM.

### Invalidación automática

#### Cuando El Vaciado Total Es Más Barato Que El Seguimiento De Dependencias

Desde CQ5 dependemos totalmente de invalidar, más o menos, todo el sitio si solo cambia una de las páginas. Esta función se denomina &quot;Invalidación automática&quot;.

Pero de nuevo, ¿cómo puede ser que tirar y volver a procesar cientos de páginas sea más barato que hacer un seguimiento de dependencia adecuado y una nueva representación parcial?

Hay dos razones principales:

1. En un sitio web promedio, solo se solicita con frecuencia un pequeño subconjunto de las páginas. Por lo tanto, incluso si desecha todo el contenido procesado, solo unas pocas docenas se solicitarán inmediatamente después. La renderización de la larga cola de páginas se puede distribuir a lo largo del tiempo, cuando realmente se solicitan. Por lo tanto, la carga en la renderización de páginas no es tan alta como cabría esperar. Por supuesto, siempre hay excepciones... discutiremos algunos trucos sobre cómo manejar cargas igualmente distribuidas en sitios web más grandes con cachés vacías de Dispatcher, más adelante.

2. Todas las páginas están conectadas mediante la navegación principal de todos modos. Así que casi todas las páginas dependen unas de otras. Esto significa que incluso el rastreador de dependencias más inteligente descubrirá lo que ya sabemos: si una de las páginas cambia, tiene que invalidar todas las demás.

¿No lo crees? Ilustremos el último punto.

Se utiliza el mismo argumento que en el último ejemplo con teasers que hacen referencia al contenido de una página remota. Solo que ahora estamos usando un ejemplo más extremo: una navegación principal procesada automáticamente. Al igual que con el teaser, el título de navegación se extrae de la página vinculada o &quot;remota&quot; como referencia de contenido. Los títulos de navegación remota no se almacenan en la página procesada actualmente. Recuerde que la navegación se procesa en todas y cada una de las páginas del sitio web. Por lo tanto, el título de una página se utiliza una y otra vez en todas las páginas que tienen una navegación principal. Y si desea cambiar un título de navegación, desea hacerlo solo una vez en la página remota, no en todas y cada una de las páginas que hacen referencia a la página.

Por lo tanto, en nuestro ejemplo, la navegación une todas las páginas utilizando el &quot;NavTitle&quot; de la página de destino para representar un nombre en la navegación. El título de navegación para &quot;Islandia&quot; se extrae de la página &quot;Islandia&quot; y se representa en todas y cada una de las páginas que tienen una navegación principal.

![La navegación principal inevitablemente une el contenido de todas las páginas al extraer sus &quot;NavTitles&quot;](assets/chapter-1/nav-titles.png)

*La navegación principal inevitablemente une el contenido de todas las páginas al extraer sus &quot;NavTitles&quot;*

<br> 

Si cambia el NavTitle en la página de Islandia de &quot;Islandia&quot; a &quot;Hermosa Islandia&quot;, ese título cambiará inmediatamente en el menú principal de todas las demás páginas. Por lo tanto, las páginas procesadas y almacenadas en caché antes de ese cambio, todas quedan obsoletas y deben invalidarse.

#### Implementación de la invalidación automática: el archivo .stat

Ahora, si tiene un sitio grande con miles de páginas, tardaría bastante tiempo en recorrer todas las páginas y eliminarlas físicamente. Durante ese periodo, Dispatcher podría ofrecer contenido obsoleto de forma involuntaria. Peor aún: pueden producirse algunos conflictos al acceder a los archivos de caché, puede que se solicite una página mientras se elimina o que se vuelva a eliminar una página debido a una segunda invalidación que se produjo después de una activación posterior inmediata. Piensen en el desastre que sería. Por suerte, esto no es lo que pasa. El Dispatcher utiliza un truco inteligente para evitarlo: en lugar de eliminar cientos y miles de archivos, coloca un archivo simple y vacío en la raíz del sistema de archivos cuando se publica un archivo y, por lo tanto, todos los archivos dependientes se consideran no válidos. Este archivo se denomina &quot;archivo de estado&quot;. El archivo de estado es un archivo vacío: lo que importa sobre el archivo de estado es su fecha de creación.

Todos los archivos de Dispatcher que tienen una fecha de creación anterior al archivo de estado, se han procesado antes de la última activación (e invalidación) y, por lo tanto, se consideran &quot;no válidos&quot;. Todavía están físicamente presentes en el sistema de archivos, pero Dispatcher los ignora. Son &quot;anticuados&quot;. Cada vez que se realiza una solicitud a un recurso obsoleto, Dispatcher solicita al sistema de AEM que vuelva a procesar la página. Esa página recién procesada se almacena en el sistema de archivos, ahora con una nueva fecha de creación y se actualiza de nuevo.

![La fecha de creación del archivo .stat define qué contenido está obsoleto y cuál es nuevo](assets/chapter-1/creation-date.png)

*La fecha de creación del archivo .stat define qué contenido está obsoleto y cuál es nuevo*

<br> 

¿Puede preguntar por qué se llama &quot;.stat&quot;? ¿Y no quizás &quot;.invalidada&quot;? Bueno, puede imaginar que tener ese archivo en su sistema de archivos ayuda a Dispatcher a determinar qué recursos se podrían servir *estáticamente*, al igual que desde un servidor web estático. Estos archivos ya no necesitan procesarse de forma dinámica.

La verdadera naturaleza del nombre, sin embargo, es menos metafórica. Se deriva de la llamada al sistema Unix `stat()`, que devuelve la hora de modificación de un archivo (entre otras propiedades).

#### Combinación de validación simple y automática

Pero espera... antes dijimos, que los recursos individuales se borran físicamente. Ahora decimos, que un archivo de estado más reciente virtualmente los haría inválidos a los ojos de la Dispatcher. ¿Por qué entonces la eliminación física, primero?

La respuesta es simple. Normalmente se utilizan ambas estrategias en paralelo, pero para distintos tipos de recursos. Los recursos binarios, como las imágenes, son independientes. No están conectados a otros recursos en el sentido de que necesitan que se represente su información.

Las páginas de HTML, por otro lado, son altamente interdependientes. Por lo tanto, debe aplicar la invalidación automática en esos casos. Esta es la configuración predeterminada de Dispatcher. Todos los archivos que pertenecen a un recurso invalidado se eliminan físicamente. Además, los archivos que terminan con &quot;.html&quot; se invalidan automáticamente.

Dispatcher decide la extensión del archivo, aplique o no el esquema de invalidación automática.

Los finales de archivo para la invalidación automática se pueden configurar. En teoría, puede incluir todas las extensiones para la invalidación automática. Pero tenga en cuenta que esto tiene un precio muy alto. No verá que los recursos antiguos se entreguen de forma involuntaria, pero el rendimiento de la entrega se degrada considerablemente debido a la invalidación excesiva.

Imagine, por ejemplo, que implementa un esquema en el que los archivos PNG y JPG se representan dinámicamente y dependen de otros recursos para hacerlo. Es posible que desee cambiar la escala de las imágenes de alta resolución a una resolución más pequeña compatible con la Web. Mientras estás en esto también cambia la tasa de compresión. La resolución y la tasa de compresión de este ejemplo no son constantes fijas, sino parámetros configurables en el componente que utiliza la imagen. Ahora, si se cambia este parámetro, es necesario invalidar las imágenes.

No hay problema: acabamos de aprender que podemos añadir imágenes a la invalidación automática y que siempre tenemos imágenes recién procesadas cada vez que cambia algo.

#### Tirando al bebé con el agua de baño

Así es, y ese es un gran problema. Vuelva a leer el último párrafo. &quot;...imágenes recién procesadas cada vez que cambia algo.&quot; Como ya sabe, un buen sitio web cambia constantemente; se añade nuevo contenido aquí, se corrige un error tipográfico allí, y se modifica un teaser en otro lugar. Esto significa que todas las imágenes se invalidan constantemente y deben volver a procesarse. No subestimes eso. El procesamiento y la transferencia dinámicos de datos de imagen funcionan en milisegundos en la máquina de desarrollo local. El entorno de producción debe hacerlo cien veces más a menudo, por segundo.

Y seamos claros aquí, sus jpgs necesitan ser renderizados, cuando cambia una página html y viceversa. Solo hay un &quot;bloque&quot; de archivos que se va a invalidar automáticamente. Se limpia como un todo. Sin ningún medio de descomponerse en estructuras más detalladas.

Hay una buena razón por la que la invalidación automática se mantiene en &quot;.html&quot; de forma predeterminada. El objetivo es mantener ese cubo lo más pequeño posible. No tire al bebé con el agua del baño simplemente invalidando todo, solo para estar en el lado seguro.

Los recursos independientes deben servirse en la ruta de ese recurso. Eso ayuda mucho a la invalidación. Para ser más sencillos, no cree esquemas de asignación como &quot;resource /a/b/c&quot; se proporciona desde &quot;/x/y/z&quot;. Haga que los componentes funcionen con la configuración de invalidación automática predeterminada de Dispatcher. No intente reparar un componente mal diseñado con invalidación excesiva en Dispatcher.

##### Excepciones a la invalidación automática: invalidación sólo de recursos

La solicitud de invalidación de Dispatcher generalmente se activa desde los sistemas de publicación mediante un agente de replicación.

Si se siente muy seguro de sus dependencias, puede intentar crear su propio agente de replicación invalidante.

Sería un poco más allá de esta guía para entrar en los detalles, pero queremos darle al menos algunas sugerencias.

1. Realmente sabes lo que estás haciendo. Conseguir la invalidación correcta es realmente difícil. Esa es una razón por la que la invalidación automática es tan rigurosa; para evitar entregar contenido antiguo.

2. Si su agente envía un encabezado HTTP `CQ-Action-Scope: ResourceOnly`, significa que esta única solicitud de invalidación no almacena en déclencheur una invalidación automática. Este fragmento de código ( [https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle](https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle)) puede ser un buen punto de partida para su propio agente de replicación.

3. `ResourceOnly`, solo evita la invalidación automática. Para realizar la resolución de dependencias y las invalidaciones necesarias, debe almacenar en déclencheur las solicitudes de invalidación usted mismo. Es posible que desee consultar las reglas de vaciado de Dispatcher del paquete ([https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html)) para obtener inspiración sobre cómo podría suceder esto.

No se recomienda crear un esquema de resolución de dependencias. Hay demasiado esfuerzo y poca ganancia - y como se dijo antes, hay demasiado que se equivocará.

En su lugar, lo que debe hacer es averiguar qué recursos no dependen de otros recursos y se pueden invalidar sin invalidación automática. Sin embargo, no tiene que utilizar un agente de replicación personalizado para ese caso. Simplemente cree una regla personalizada en la configuración de Dispatcher que excluya estos recursos de la invalidación automática.

Dijimos que la navegación principal o los teasers son una fuente para las dependencias. Bueno: si carga la navegación y los teasers de forma asíncrona o los incluye con un script SSI en Apache, no tendrá que rastrear esa dependencia. Más adelante en este documento analizaremos en detalle la carga asíncrona de componentes cuando hablemos de &quot;Sling Dynamic Includes&quot;.

Lo mismo se aplica a las ventanas emergentes o al contenido que se carga en una caja de luz. Estas piezas también rara vez tienen navegaciones (es decir, &quot;dependencias&quot;) y pueden invalidarse como un solo recurso.

## Creación de componentes teniendo en cuenta Dispatcher

### Aplicación de la mecánica de Dispatcher en un ejemplo real

En el último capítulo explicamos cómo funciona la mecánica básica de Dispatcher, cómo funciona en general y cuáles son las limitaciones.

Ahora queremos aplicar estas mecánicas a un tipo de componentes que muy probablemente encontrará en los requisitos de su proyecto. Elegimos el componente deliberadamente para demostrar problemas que también enfrentará tarde o temprano. No teman - no todos los componentes necesitan esa cantidad de consideración que vamos a presentar. Pero si ve la necesidad de crear un componente de este tipo, es muy consciente de las consecuencias y sabe cómo manejarlas.

### Patrón del componente de bobinado (Anti)

#### El componente Imagen interactiva

Ilustremos un patrón común (o antipatrón) de un componente con binarios interconectados. Crearemos un componente &quot;respi&quot;: para &quot;imagen adaptable&quot;. Este componente debe poder adaptar la imagen visualizada al dispositivo en el que se muestra. En escritorios y tabletas muestra la resolución completa de la imagen, en teléfonos una versión más pequeña con un recorte estrecho - o tal vez incluso un motivo completamente diferente (esto se llama &quot;dirección de arte&quot; en el mundo interactivo).

Los recursos se cargan en el área DAM de AEM y solo se hace referencia a _ellos_ en el componente de imagen adaptable.

El componente de respi se encarga de procesar el marcado y de entregar los datos de imagen binarios.

La forma en que lo implementamos aquí es un patrón común que hemos visto en muchos proyectos e incluso uno de los componentes principales de AEM se basa en ese patrón. Por lo tanto, es muy probable que usted como desarrollador pueda adaptar ese patrón. Tiene sus puntos dulces en términos de encapsulación, pero requiere mucho esfuerzo para prepararlo para Dispatcher. Discutiremos varias opciones sobre cómo mitigar el problema más adelante.

Llamamos al patrón usado aquí &quot;Patrón de cola&quot;, porque el problema se remonta a los primeros días del Comunicado 3, donde había un método &quot;bobina&quot; que se podía llamar en un recurso para transmitir sus datos binarios sin procesar en la respuesta.

El término original &quot;puesta en cola&quot; hace referencia a periféricos compartidos sin conexión lentos, como impresoras, por lo que no se aplica aquí correctamente. Pero nos gusta el término de todos modos, porque rara vez en el mundo en línea, por lo tanto, distinguible. Y cada patrón debería tener un nombre distinguible de todos modos, ¿verdad? Depende de ti decidir si esto es un patrón o un anti-patrón.

#### Implementación

Así se implementa nuestro componente de imagen adaptable:

El componente tiene dos partes: la primera parte procesa el marcado HTML de la imagen y la segunda parte pone en cola los datos binarios de la imagen a la que se hace referencia. Como se trata de un sitio web moderno con un diseño interactivo, no se está procesando una etiqueta `<img src"…">` simple, sino un conjunto de imágenes en la etiqueta `<picture/>`. Para cada dispositivo, cargamos dos imágenes diferentes en DAM y las referenciamos desde nuestro componente de imagen.

El componente tiene tres scripts de renderización (implementados en JSP, HTL o como servlet), cada uno de los cuales se dirige con un selector dedicado:

1. `/respi.jsp`: sin selector para procesar el marcado de HTML
2. `/respi.img.java` para procesar la versión de escritorio
3. `/respi.img.mobile.java` para procesar la versión móvil.


El componente se coloca en el parsys de la página principal. A continuación, se ilustra la estructura resultante en CRX.

![Estructura de recursos de la imagen adaptable en CRX](assets/chapter-1/responsive-image-crx.png)

*Estructura de recursos de la imagen adaptable en CRX*

<br> 

El marcado de los componentes se representa de esta manera,

```plain
  #GET /content/home.html

  <html>

  …

  <div class="responsive-image>

  <picture>
    <source src="/content/home/jcr:content/par/respi.img.mobile.jpg" …/>
    <source src="/content/home/jcr:content/par/respi.img.jpg …/>

    …

  </picture>
  </div>
  …
```

y... hemos terminado con nuestro componente bien encapsulado.

#### Componente de imagen interactivo en acción

Ahora un usuario solicita la página y los recursos a través de Dispatcher. Esto da como resultado archivos en el sistema de archivos de Dispatcher como se ilustra a continuación,

![Estructura en caché del componente de imagen adaptable encapsulado](assets/chapter-1/cached-structure-encapsulated-image-comonent.png)

*Estructura en caché del componente de imagen adaptable encapsulado*

<br> 

Considere que un usuario carga y activa una nueva versión de las dos imágenes de flores en el DAM. AEM enviará la solicitud de invalidación correspondiente a

`/content/dam/flower.jpg`

y

`/content/dam/flower-mobile.jpg`

a Dispatcher. Sin embargo, estas solicitudes son en vano. El contenido se ha almacenado en caché como archivos debajo de la subestructura del componente. Estos archivos ya están obsoletos, pero se proporcionan bajo petición.

![Discrepancia de estructura que lleva a contenido obsoleto](assets/chapter-1/structure-mismatch.png)

*Discrepancia de estructura que lleva a contenido obsoleto*

<br> 

Hay otra advertencia para este enfoque. Considere que utiliza el mismo flower.jpg en varias páginas. A continuación, tendrá el mismo recurso en la caché en varias direcciones URL o archivos,

```
/content/home/products/jcr:content/par/respi.img.jpg

/content/home/offers/jcr:content/par/respi.img.jpg

/content/home/specials/jcr:content/par/respi.img.jpg

…
```

Cada vez que se solicita una página nueva y sin almacenar en caché, los recursos se recuperan de AEM en distintas direcciones URL. Ningún almacenamiento en caché de Dispatcher ni de explorador puede acelerar el envío.

#### Donde brilla el patrón de cola de impresión

Hay una excepción natural, donde este patrón incluso en su forma simple es útil: Si el binario se almacena en el propio componente, y no en el DAM. Sin embargo, esto solo es útil para imágenes utilizadas una vez en el sitio web y no almacenar recursos en el DAM significa que le cuesta mucho administrar sus recursos. Imagine que se agota su licencia de uso de un recurso concreto. ¿Cómo puede saber qué componentes ha utilizado el recurso?

¿Lo ves? La &quot;M&quot; en DAM significa &quot;Administración&quot;, como en la Administración de activos digitales. No quiere revelar esa característica de forma gratuita.

#### Conclusión

Desde la perspectiva de un desarrollador de AEM, el patrón parecía súper elegante. Pero con la Dispatcher en la ecuación, podrían estar de acuerdo, en que el enfoque ingenuo podría no ser suficiente.

Por ahora, dejamos que usted decida si esto es un patrón o un anti-patrón. ¿Y tal vez ya tiene algunas buenas ideas en mente sobre cómo mitigar los problemas explicados anteriormente? Bien. Entonces debería estar ansioso por ver cómo otros proyectos han resuelto estos problemas.

### Solución de problemas comunes de Dispatcher

#### Información general

Hablemos de cómo se podría haber implementado un poco más compatible con la caché. Hay varias opciones. A veces no se puede elegir la mejor solución. Tal vez entra en un proyecto que ya está en ejecución y tiene un presupuesto limitado para solucionar el &quot;problema de caché&quot; que tiene a mano y no lo suficiente para hacer una refactorización completa. O se enfrenta a un problema, que es más complejo que el componente de imagen de ejemplo.

Describiremos los principios y las advertencias en las siguientes secciones.

De nuevo, esto se basa en la experiencia de la vida real. Ya hemos visto todos esos patrones en la naturaleza, así que no es un ejercicio académico. Es por eso que les mostramos algunos antipatrones, así que tienen la oportunidad de aprender de los errores que otros ya han cometido.

#### Asesino de caché

>[!WARNING]
>
>Esto es un anti-patrón. No lo use. Nunca.

¿Alguna vez ha visto parámetros de consulta como `?ck=398547283745`? Se llaman &quot;cache-killer&quot; (&quot;ck&quot;). La idea es que, si agrega cualquier parámetro de consulta, el recurso no se almacene en caché. Además, si agrega un número aleatorio como valor del parámetro (como &quot;398547283745&quot;), la URL se convierte en única y se asegura de que ninguna otra caché entre el sistema de AEM y la pantalla pueda almacenar en caché. Normalmente, los sospechosos intermedios serían una caché de &quot;barniz&quot; delante de la Dispatcher, una CDN o incluso la caché del explorador. De nuevo: no hagas eso. Desea que los recursos se almacenen en caché el mayor tiempo posible. El caché es tu amigo. No mates a tus amigos.

#### Invalidación automática

>[!WARNING]
>
>Esto es un anti-patrón. Evite utilizarlo para recursos digitales. Intente mantener únicamente la configuración predeterminada de Dispatcher, que > invalida automáticamente los archivos &quot;.html&quot;

A corto plazo, puede agregar &quot;.jpg&quot; y &quot;.png&quot; a la configuración de invalidación automática en Dispatcher. Esto significa que, cada vez que se produce una invalidación, es necesario volver a procesar todos los archivos &quot;.jpg&quot;, &quot;.png&quot; y &quot;.html&quot;.

Este patrón es muy fácil de implementar si los propietarios de negocios se quejan de que sus cambios no se materializan en el sitio activo con la suficiente rapidez. Pero esto sólo le puede dar tiempo para llegar a una solución más sofisticada.

Asegúrese de comprender los enormes impactos en el rendimiento. Esto ralentizará significativamente el sitio web e incluso podría afectar a la estabilidad, si el sitio es un sitio web de alta carga con cambios frecuentes, como un portal de noticias.

#### Huella digital de URL

La huella digital de una URL parece un asesino en caché. Pero no lo es. No es un número aleatorio, sino un valor que caracteriza el contenido del recurso. Puede ser un hash del contenido del recurso o, más simple aún, una marca de tiempo cuando el recurso se cargó, editó o actualizó.

Una marca de tiempo Unix es suficientemente buena para una implementación en el mundo real. Para mejorar la legibilidad, se utiliza un formato más legible en este tutorial: `2018 31.12 23:59 or fp-2018-31-12-23-59`.

La huella digital no debe utilizarse como parámetro de consulta, como las direcciones URL con parámetros de consulta   no se puede almacenar en caché. Puede utilizar un selector o el sufijo para la huella digital.

Supongamos que el archivo `/content/dam/flower.jpg` tiene una fecha `jcr:lastModified` del 31 de diciembre de 2018, 23:59. La dirección URL con la huella digital es `/content/home/jcr:content/par/respi.fp-2018-31-12-23-59.jpg`.

Esta dirección URL permanece estable, siempre y cuando no se cambie el archivo de recursos al que se hace referencia (`flower.jpg`). Por lo tanto, se puede almacenar en caché durante un tiempo indefinido y no es un asesino en caché.

Tenga en cuenta que esta URL debe crearla y servirla el componente de imagen adaptable. No se trata de una funcionalidad predeterminada de AEM.

Ese es el concepto básico. Sin embargo, hay algunos detalles que podrían pasarse por alto fácilmente.

En nuestro ejemplo, el componente se procesó y se almacenó en caché a las 23:59. Ahora la imagen ha cambiado, digamos que a las 00:00.  El componente _generaría_ una nueva dirección URL con huellas digitales en su marcado.

Puede que pienses que _debería_... pero no es así. Como solo se ha cambiado el binario de la imagen y no se ha tocado la página incluida, no es necesario volver a procesar el marcado de HTML. Por lo tanto, el Dispatcher sirve la página con la huella digital antigua, y por lo tanto la versión antigua de la imagen.

![El componente de imagen es más reciente que la imagen a la que se hace referencia, no se procesó ninguna huella digital nueva.](assets/chapter-1/recent-image-component.png)

*El componente de imagen es más reciente que la imagen a la que se hace referencia, no se procesó ninguna huella digital nueva.*

<br> 

Ahora, si vuelve a activar la página de inicio (o cualquier otra página de ese sitio) en la que se actualiza el archivo de estado, Dispatcher considera que el archivo home.html está obsoleto y lo vuelve a procesar con una nueva huella digital en el componente de imagen.

Pero no activamos la página principal, ¿verdad? ¿Y por qué deberíamos activar una página que no tocamos de todos modos? Además, tal vez no tengamos suficientes derechos para activar páginas o el flujo de trabajo de aprobación es tan largo y lleva tanto tiempo que simplemente no podemos hacerlo con tan poco tiempo de antelación. Entonces, ¿qué hacer?

#### Herramienta del administrador diferido: Niveles de archivo de estado decrecientes

>[!WARNING]
>
>Esto es un anti-patrón. Utilícelo solo a corto plazo para ganar tiempo e idear una solución más sofisticada.

El administrador diferido generalmente &quot;_establece la invalidación automática en jpgs y el nivel de archivo de estado en cero, lo que siempre ayuda con los problemas de almacenamiento en caché de todo tipo_&quot;. Encontrarás ese consejo en foros de tecnología, y ayuda con tu problema de invalidación.

Hasta ahora no hemos discutido el nivel del archivo de estado. Básicamente, la invalidación automática solo funciona para archivos del mismo subárbol. Sin embargo, el problema es que las páginas y los recursos generalmente no residen en el mismo subdirectorio. Las páginas están por debajo de `/content/mysite`, mientras que los recursos viven por debajo de `/content/dam`.

El &quot;nivel de archivo de estado&quot; define a qué profundidad se encuentran los nodos raíz de los subárboles. En el ejemplo por encima del nivel sería &quot;2&quot; (1=/content, 2=/mysite,dam)

La idea de &quot;reducir&quot; el nivel del archivo de estado a 0 básicamente es definir todo el árbol /content como el único subárbol para hacer que las páginas y los recursos vivan en el mismo dominio de invalidación automática. Así que solo tendríamos un árbol grande a nivel (en el docroot &quot;/&quot;). Sin embargo, al hacerlo, se invalidan automáticamente todos los sitios del servidor cada vez que se publica algo, incluso en sitios completamente no relacionados. Confíe en nosotros: Esta es una mala idea a largo plazo, ya que degradará seriamente su tasa general de aciertos de caché. Todo lo que puede hacer es esperar que los servidores de AEM tengan suficiente potencia de fuego para ejecutarse sin caché.

Un poco más tarde comprenderá todas las ventajas de contar con niveles de archivo de estado más profundos.

#### Implementación de un agente de invalidación personalizado

De todos modos: tenemos que decirle a la Dispatcher de alguna manera, para invalidar las HTML-Pages si un &quot;.jpg&quot; o &quot;.png&quot; cambió para permitir volver a procesar con una nueva URL.

Lo que hemos visto en proyectos es, por ejemplo, agentes de replicación especiales en el sistema de publicación que envían solicitudes de invalidación para un sitio cada vez que se publica una imagen de ese sitio.

Aquí resulta muy útil si puede derivar la ruta del sitio de la ruta del recurso mediante la convención de nomenclatura.

En términos generales, es aconsejable hacer coincidir los sitios y las rutas de recursos de esta manera:

**Ejemplo**

```
/content/dam/site-a
/content/dam/site-b

/content/site-a
/content/site-b
```

De este modo, su agente de vaciado de Dispatcher personalizado podría enviar fácilmente una solicitud de invalidación a /content/site-a cuando encuentre un cambio en `/content/dam/site-a`.

En realidad, no importa qué ruta le diga a la Dispatcher que invalide, siempre y cuando esté en el mismo sitio, en el mismo &quot;subárbol&quot;. Ni siquiera tiene que utilizar una ruta de recursos real. También puede ser &quot;virtual&quot;:

```
GET /dispatcher-invalidate
Invalidate-path /content/mysite/dummy
```

![](assets/chapter-1/resource-path.png)

1. Se activa una escucha en el sistema de publicación cuando cambia un archivo en DAM

2. El oyente envía una solicitud de invalidación a Dispatcher. Debido a la invalidación automática, no importa qué ruta enviemos en la invalidación automática, a menos que esté en la página principal del sitio o más exactamente en el nivel del archivo de estado del sitio.

3. Se actualiza el archivo de estado.

4. La próxima vez que se solicite la página principal, se vuelve a procesar. La nueva huella/ fecha se toma de la propiedad lastModified de la imagen como selector adicional

5. Esto crea implícitamente una referencia a una nueva imagen

6. Si la imagen se solicita, se creará una nueva representación y se almacenará en Dispatcher


#### La necesidad de limpiar

¡Uf! Terminado. ¡Hurra!

Bueno... aún no.

La ruta,

`/content/mysite/home/jcr:content/par/respi.img.fp-2018-31-12-23-59.jpg`

no está relacionado con ninguno de los recursos invalidados. ¿Recuerdas? Solo invalidamos un recurso &quot;ficticio&quot; y nos basamos en la invalidación automática para considerar que &quot;home&quot; no era válido. Es posible que la imagen en sí nunca se elimine _físicamente_. Por lo tanto, la caché crecerá y crecerá y crecerá. Cuando las imágenes se cambian y activan, obtienen nuevos nombres de archivo en el sistema de archivos de Dispatcher.

Hay tres problemas con no eliminar físicamente los archivos en caché y mantenerlos indefinidamente:

1. Está desperdiciando capacidad de almacenamiento, obviamente. Concedido - el almacenamiento se ha vuelto más barato y más barato en los últimos años. Sin embargo, las resoluciones de imagen y el tamaño de los archivos también han aumentado en los últimos años, con la llegada de pantallas similares a la retina que necesitan imágenes nítidas como el cristal.

2. A pesar de que los discos duros se han vuelto más baratos, el &quot;almacenamiento&quot; podría no haberse vuelto más barato. Hemos observado una tendencia a no tener almacenamiento (barato) en disco duro, sino almacenamiento virtual alquilado en un NAS por su proveedor de centros de datos. Este tipo de almacenamiento es un poco más fiable y escalable, pero también un poco más caro. Es posible que no quiera desperdiciarla almacenando basura obsoleta. Esto no solo se relaciona con el almacenamiento principal, sino que también con las copias de seguridad. Si tiene una solución de copia de seguridad lista para usar, es posible que no pueda excluir los directorios de caché. Al final, también realiza una copia de seguridad de los datos de basura.

3. Peor aún: es posible que haya comprado licencias de uso para determinadas imágenes solo durante un tiempo limitado, siempre y cuando las necesite. Ahora, si aún almacena la imagen después de que una licencia haya caducado, podría verse como una infracción de copyright. Es posible que ya no utilice la imagen en sus páginas web, pero Google seguirá encontrándola.

Así que finalmente, se te ocurrirá un trabajo de limpieza para limpiar todos los archivos anteriores a... digamos una semana para mantener este tipo de basura bajo control.

#### Abuso de las huellas digitales de URL para ataques de denegación de servicio

Pero espere, hay otro defecto en esta solución:

Estamos abusando de un selector como parámetro: fp-2018-31-12-23-59 se genera dinámicamente como una especie de &quot;cache-killer&quot;. Pero tal vez un chico aburrido (o un rastreador de motor de búsqueda que se ha vuelto loco) comienza a solicitar las páginas:

```
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-00.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-01.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-02.jpg

…
```

Cada solicitud evita la Dispatcher, lo que provoca la carga en una instancia de publicación. Y, lo que es peor, cree un archivo acorde en la Dispatcher.

Así que... en lugar de usar la huella digital como un simple asesino en caché, tendría que comprobar la fecha jcr:lastModified de la imagen y devolver un 404 si no es la fecha esperada. Eso toma un poco de tiempo y ciclos de CPU en el sistema de publicación... que es lo que quería evitar en primer lugar.

#### Advertencias sobre las huellas digitales de URL en versiones de alta frecuencia

Puede utilizar el esquema de huella digital no solo para los recursos que provienen de DAM, sino también para archivos JS y CSS y recursos relacionados.

[Clientlibs con versiones](https://adobe-consulting-services.github.io/acs-aem-commons/features/versioned-clientlibs/index.html) es un módulo que usa este método.

Pero aquí podría enfrentarse a otra advertencia que tenga huellas digitales de URL: vincula la URL con el contenido. No puede cambiar el contenido sin cambiar también la dirección URL (es decir, actualizar la fecha de modificación). Para eso están diseñadas las huellas digitales en primer lugar. Pero tenga en cuenta que está implementando una nueva versión, con nuevos archivos CSS y JS y, por lo tanto, nuevas direcciones URL con nuevas huellas digitales. Todas las páginas de HTML siguen teniendo referencias a las antiguas direcciones URL con huellas digitales. Por lo tanto, para que la nueva versión funcione de forma coherente, debe invalidar todas las páginas de HTML a la vez para forzar una nueva renderización con referencias a los archivos de nueva huella digital. Si tiene varios sitios que dependen de las mismas bibliotecas, puede significar una cantidad considerable de reprocesamiento, y aquí no puede aprovechar `statfiles`. Por lo tanto, asegúrese de ver los picos de carga en los sistemas de publicación después de un despliegue. Puede considerar una implementación azul-verde con calentamiento de caché o tal vez una caché basada en TTL delante de su Dispatcher... las posibilidades son infinitas.

#### Un descanso corto

Vaya, hay muchos detalles que hay que tener en cuenta, ¿no? Y se niega a ser entendido, probado y depurado fácilmente. Y todo por una solución aparentemente elegante. Es cierto que es elegante, pero solo desde una perspectiva de AEM. Junto con el Dispatcher se vuelve desagradable.

Y aún así - no resuelve una advertencia básica, si una imagen se utiliza varias veces en diferentes páginas, se almacenan en caché en esas páginas. No hay mucha sinergia de almacenamiento en caché allí.

En general, la huella digital de URL es una buena herramienta para tener en su kit de herramientas, pero debe aplicarla con cuidado, ya que puede causar nuevos problemas al resolver solo unos pocos existentes.

Así que... ese fue un capítulo largo. Pero hemos visto este patrón tan a menudo, que sentimos que es necesario darle toda la imagen con todos los pros y los contras. Las huellas digitales de URL resuelven algunos de los problemas inherentes al patrón de cola de impresión, pero el esfuerzo por implementar es bastante alto y también necesita considerar otras soluciones (más fáciles). Nuestro consejo es comprobar siempre si puede basar sus URL en las rutas de recursos servidas y no tener un componente intermedio. Llegaremos a esto en el siguiente capítulo.

##### Resolución de dependencias de tiempo de ejecución

La resolución de dependencias en tiempo de ejecución es un concepto que hemos estado considerando en un proyecto. Pero pensándolo bien, se hizo bastante complejo y decidimos no implementarlo.

Esta es la idea básica:

Dispatcher no conoce las dependencias de los recursos. Es sólo un montón de archivos individuales con poca semántica.

AEM también sabe poco acerca de las dependencias. Carece de una semántica adecuada o de un &quot;rastreador de dependencias&quot;.

AEM tiene en cuenta algunas de las referencias. Utiliza este conocimiento para advertirle cuando intenta eliminar o mover una página o recurso al que se hace referencia. Lo hace consultando la búsqueda interna al eliminar un recurso. Las referencias de contenido tienen una forma muy particular. Son expresiones de ruta que comienzan por &quot;/content&quot;. Por lo tanto, se pueden indexar fácilmente con texto completo y consultarlas cuando sea necesario.

En nuestro caso, necesitaríamos un agente de replicación personalizado en el sistema de publicación, que ponga en déclencheur una búsqueda de una ruta específica cuando esa ruta haya cambiado.

Digamos que...

`/content/dam/flower.jpg`

Ha cambiado en Publicar. El agente activaría una búsqueda de &quot;/content/dam/flower.jpg&quot; y encontraría todas las páginas que hacen referencia a esas imágenes.

Entonces podría enviar una serie de solicitudes de invalidación a Dispatcher. Uno por cada página que contenga el recurso.

En teoría, eso debería funcionar. Pero solo para dependencias de primer nivel. No le gustaría aplicar ese esquema para dependencias de varios niveles, por ejemplo, cuando utiliza la imagen en un fragmento de experiencia que se utiliza en una página. En realidad, creemos que ese enfoque es demasiado complejo -y podría haber problemas en tiempo de ejecución. Y, por lo general, el mejor consejo es no hacer computación costosa en los controladores de eventos. Y sobre todo la búsqueda puede llegar a ser bastante caro.

##### Conclusión

Esperamos haber analizado el patrón de cola de impresión lo suficientemente a fondo como para ayudarle a decidir cuándo utilizarlo y no en su implementación.

## Evitar problemas de Dispatcher

### URL basadas en recursos

Una forma mucho más elegante de resolver el problema de la dependencia es no tener dependencias en absoluto. Evite las dependencias artificiales que se producen al utilizar un recurso para simplemente representar a otro, como hicimos en el último ejemplo. Intente ver los recursos como entidades &quot;solitarias&quot; con la mayor frecuencia posible.

Nuestro ejemplo se resuelve fácilmente:

![Poner en cola la imagen con un servlet enlazado a la imagen, no al componente.](assets/chapter-1/spooling-image.png)

*Poner en cola la imagen con un servlet enlazado a la imagen, no al componente.*

<br> 

Utilizamos los recursos originales de las rutas de recursos para procesar los datos. Si necesitamos procesar la imagen original tal cual, solo podemos utilizar el procesador predeterminado de AEM para los recursos.

Si necesitamos realizar algún procesamiento especial para un componente específico, registraríamos un servlet dedicado en esa ruta y un selector para realizar la transformación en nombre del componente. Lo hicimos aquí ejemplar con el &quot;.respi&quot;. selector. Es aconsejable realizar un seguimiento de los nombres de los selectores que se usan en el espacio de URL global (como `/content/dam`) y tener una buena convención de nombres para evitar conflictos de nombres.

Por cierto: no vemos ningún problema con la coherencia del código. El servlet se puede definir en el mismo paquete Java que el modelo de componentes sling.

Incluso podemos usar selectores adicionales en el espacio global como,

`/content/dam/flower.respi.thumbnail.jpg`

Tranquilo, ¿verdad? Entonces, ¿por qué la gente viene con patrones complicados como el Spooler?

Bueno, podríamos resolver el problema evitando la referencia de contenido interna porque el componente externo agregaba poco valor o información a la representación del recurso interno, que podría codificarse fácilmente en un conjunto de selectores estáticos que controlan la representación de un recurso solitario.

Sin embargo, hay una clase de casos que no se pueden resolver fácilmente con una dirección URL basada en recursos. Este tipo de casos se denomina &quot;Parámetro de inyección de componentes&quot; y se analizan en el capítulo siguiente.

### Parámetro de inyección de componentes

#### Información general

El Cola del último capítulo era solo un envoltorio delgado alrededor de un recurso. Causó más problemas que ayudar a resolver el problema.

Podríamos sustituir fácilmente ese envoltorio utilizando un selector simple y añadir un servlet adecuado para servir esas solicitudes.

Pero ¿qué sucede si el componente &quot;respi&quot; es más que solo un proxy. ¿Qué sucede si el componente contribuye de forma genuina a la renderización del componente?

Vamos a introducir una pequeña extensión de nuestro componente &quot;respi&quot;, que es un poco un cambio de juego. Una vez más, primero presentaremos algunas soluciones ingenuas para abordar los nuevos desafíos y mostrar dónde se quedan cortas.

#### El Componente Respi2

El componente respi2 es un componente que muestra una imagen adaptable, como lo es el componente respi. Pero tiene un ligero complemento,

![estructura de CRX: el componente respi2 agrega una propiedad de calidad a la entrega](assets/chapter-1/respi2.png)

*estructura de CRX: el componente respi2 agrega una propiedad de calidad a la entrega*

<br> 

Las imágenes son JPEG y se pueden comprimir. Cuando comprime una imagen JPEG, intercambia la calidad por el tamaño del archivo. La compresión se define como un parámetro numérico de &quot;calidad&quot; que varía de &quot;1&quot; a &quot;100&quot;. &quot;1&quot; significa &quot;calidad pequeña pero mala&quot;, &quot;100&quot; significa &quot;excelente calidad pero archivos grandes&quot;. Entonces, ¿cuál es el valor perfecto?

Como en todas las cosas de TI, la respuesta es: &quot;Depende&quot;.

Aquí depende del motivo. Los motivos con bordes de alto contraste como los motivos que incluyen texto escrito, fotos de edificios, ilustraciones, bocetos o fotos de cajas de productos (con contornos nítidos y texto escrito en ella) generalmente entran dentro de esa categoría. Los motivos con transiciones de color y contraste más suaves, como paisajes o retratos, se pueden comprimir un poco más sin perder calidad visible. Las fotografías de la naturaleza generalmente entran en esa categoría.

Además, según dónde se utilice la imagen, es posible que desee utilizar un parámetro diferente. Una miniatura pequeña en un teaser puede soportar una mejor compresión que la misma imagen utilizada en un banner a pantalla completa. Esto significa que el parámetro de calidad no es innato de la imagen, sino de la imagen y el contexto. Y al gusto del autor.

En resumen: no hay un escenario perfecto para todas las imágenes. No hay una talla única para todos. Es mejor que el autor decida. Modificará el parámetro &quot;calidad&quot; como propiedad del componente hasta que esté satisfecho con la calidad y no vaya más allá para no sacrificar ancho de banda.

Ahora tenemos un archivo binario en DAM y un componente, que proporciona una propiedad de calidad. ¿Qué aspecto debería tener la dirección URL? ¿Qué componente es responsable de la bobina?

#### Enfoque 1 ingenuo: pasar propiedades como parámetros de consulta

>[!WARNING]
>
>Esto es un anti-patrón. No lo use.

En el último capítulo, la URL de imagen procesada por el componente tenía este aspecto:

`/content/dam/flower.respi.jpg`

Lo único que falta es el valor de la calidad. El componente sabe qué propiedad ha introducido el autor... Se podría pasar fácilmente al servlet de procesamiento de imágenes como parámetro de consulta cuando se procese el marcado, como `flower.respi2.jpg?quality=60`:

```plain
  <div class="respi2">
  <picture>
    <source src="/content/dam/flower.respi2.jpg?quality=60" …/>
    …
  </picture>
  </div>
  …
```

Esta es una mala idea. ¿Recuerdas? Las solicitudes con parámetros de consulta no se pueden almacenar en caché.

#### Enfoque 2 no utilizado: pasar información adicional como selector

>[!WARNING]
>
>Esto podría convertirse en un antipatrón. Úsalo con cuidado.

![Pasar propiedades de componente como selectores](assets/chapter-1/passing-component-properties.png)

*Pasar propiedades de componente como selectores*

<br> 

Es una ligera variación de la última URL. Solo esta vez utilizamos un selector para pasar la propiedad al servlet, de modo que el resultado se pueda almacenar en caché:

`/content/dam/flower.respi.q-60.jpg`

Esto es mucho mejor, pero ¿recuerdas a ese desagradable guionista del último capítulo que busca esos patrones? Él vería lo lejos que puede llegar con los valores de bucle sobre:

```plain
  /content/dam/flower.respi.q-60.jpg
  /content/dam/flower.respi.q-61.jpg
  /content/dam/flower.respi.q-62.jpg
  /content/dam/flower.respi.q-63.jpg
  …
```

De nuevo, esto evita la caché y crea una carga en el sistema de publicación. Por lo tanto, podría ser una mala idea. Puede mitigar esto filtrando solo un pequeño subconjunto de parámetros. Desea permitir solamente `q-20, q-40, q-60, q-80, q-100`.

#### Filtrado de solicitudes no válidas al utilizar selectores

Reducir el número de selectores fue un buen comienzo. Como regla general, siempre debe limitar el número de parámetros válidos a un mínimo absoluto. Si lo hace de forma inteligente, incluso puede aprovechar un cortafuegos de aplicaciones web fuera de AEM mediante un conjunto estático de filtros sin tener conocimientos profundos del sistema AEM subyacente para proteger sus sistemas:

```
Allow: /content/dam/(-\_/a-z0-9)+/(-\_a-z0-9)+
       \.respi\.q-(20|40|60|80|100)\.jpg
```

Si no dispone de un cortafuegos de aplicación web, debe filtrarlo en Dispatcher o en AEM. Si lo hace en AEM, asegúrese de que

1. El filtro se implementa de forma muy eficiente, sin tener que acceder demasiado a la CRX y perder memoria y tiempo.

2. El filtro responde a un mensaje de error &quot;404 - Not found&quot;

Recalquemos el último punto de nuevo. La conversación HTTP tendría este aspecto:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 404 – Not found
  << empty response body >>
```

También hemos visto implementaciones que filtraron parámetros no válidos, pero devolvieron una representación de reserva válida cuando se utiliza un parámetro no válido. Supongamos que solo permitimos parámetros de 20 a 100. Los valores intermedios se asignan a los válidos. Por lo tanto,

`q-41, q-42, q-43, …`

siempre respondería a la misma imagen que tendría q-40:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 200 – OK
  << flower.jpg with quality = 40 >>
```

Ese enfoque no está ayudando en absoluto. Estas solicitudes son en realidad solicitudes válidas.  Consumen potencia de procesamiento y ocupan espacio en el directorio de caché de Dispatcher.

Mejor es devolver un `301 – Moved permanently`:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 301 – Moved permanently
  Location: /content/dam/flower.respi.q-40.jpg
```

Aquí AEM le está diciendo al navegador. &quot;No tengo `q-41`. Pero hey - puedes preguntarme acerca de `q-40` &quot;.

Esto agrega un bucle de solicitud-respuesta adicional a la conversación, lo que supone un poco de sobrecarga, pero es más barato que realizar el procesamiento completo en `q-41`. Y puede aprovechar el archivo que ya se encuentra en la caché en `q-40`. Sin embargo, debe comprender que las respuestas 302 no se almacenan en caché en Dispatcher, estamos hablando de lógica que se ejecuta en AEM. Una y otra vez. Así que será mejor que lo hagas delgado y rápido.

Personalmente, el 404 es el que más responde. Hace que sea muy obvio lo que está pasando. Y ayuda a detectar errores en el sitio web cuando se analizan archivos de registro. 301s puede ser intencionado, donde 404 siempre debe ser analizado y eliminado.

## Seguridad - Excursión

### Filtrado de solicitudes

#### Dónde filtrar mejor

Al final del último capítulo señalamos la necesidad de filtrar el tráfico entrante para los selectores conocidos. Esto deja la pregunta: ¿Dónde debería filtrar realmente las solicitudes?

Bueno, depende. Cuanto antes, mejor.

#### Cortafuegos de aplicaciones web

Si dispone de un dispositivo Web Application Firewall o de &quot;WAF&quot; diseñado para Web Security, debe aprovechar estas funciones. Sin embargo, es posible que descubra que el WAF lo gestionan personas con conocimientos limitados de su aplicación de contenido y que filtran solicitudes válidas o dejan pasar demasiadas solicitudes perjudiciales. Tal vez descubra que las personas que gestionan WAF están asignadas a un departamento diferente con diferentes turnos y programas de versiones, la comunicación puede no ser tan estrecha como con sus compañeros directos y no siempre se obtienen los cambios a tiempo, lo que significa que, en última instancia, su desarrollo y velocidad de contenido se ven afectados.

Podría terminar con unas cuantas reglas generales o incluso una lista de bloqueados, que según su instinto, podría ser más estricta.

#### Filtrado de publicación y Dispatcher

El siguiente paso es añadir reglas de filtrado de URL en el núcleo de Apache y/o en Dispatcher.

Aquí solo tiene acceso a las direcciones URL. Se limitan a filtros basados en patrones. Si necesita configurar un filtrado más basado en contenido (como permitir archivos solo con una marca de tiempo correcta) o quiere que parte del filtrado se controle en su Author, terminará escribiendo algo como un filtro servlet personalizado.

#### Monitorización y depuración

En la práctica tendrá algo de seguridad en cada nivel. Pero asegúrese de tener los medios para averiguar a qué nivel se filtra una solicitud. Asegúrese de que tiene acceso directo al sistema de publicación, a Dispatcher y a los archivos de registro de WAF para averiguar qué filtro de la cadena está bloqueando las solicitudes.

### Proliferación de selectores y selectores

El enfoque que utiliza &quot;selector-parameters&quot; en el último capítulo es rápido y sencillo y puede acelerar el tiempo de desarrollo de nuevos componentes, pero tiene límites.

Configurar una propiedad &quot;quality&quot; es solo un ejemplo sencillo. Pero digamos que el servlet también espera que los parámetros de &quot;anchura&quot; sean más versátiles.

Puede reducir el número de direcciones URL válidas reduciendo el número de posibles valores de selector. Puede hacer lo mismo con la anchura también:

calidad = q-20, q-40, q-60, q-80, q-100

anchura = w-100, w-200, w-400, w-800, w-1000, w-1200

Sin embargo, todas las combinaciones ahora son direcciones URL válidas:

```
/content/dam/flower.respi.q-40.w-200.jpg
/content/dam/flower.respi.q-60.w-400.jpg
…
```

Ahora ya tenemos 5x6=30 URL válidas para un recurso. Cada propiedad adicional aumenta la complejidad. Y podría haber propiedades que no se pueden reducir a una cantidad razonable de valores.

Entonces, también este enfoque tiene límites.

#### Exposición involuntaria de una API

¿Qué está pasando aquí? Si miramos con cuidado, vemos que gradualmente estamos pasando de un sitio web estático a uno altamente dinámico. Y, sin darnos cuenta, estamos mostrando una API de renderización de imágenes en el navegador del cliente que en realidad estaba destinado a ser utilizado por los autores solamente.

El autor debe configurar la calidad y el tamaño de una imagen para poder editar la página. Tener las mismas capacidades expuestas por un servlet puede verse como una función o como un vector para un ataque de denegación de servicio. Lo que realmente es, depende del contexto. ¿Qué tan importante es el sitio web? ¿Cuánta carga hay en los servidores? ¿Cuánto espacio libre queda? ¿Cuánto presupuesto tiene para la implementación? Tienes que equilibrar estos factores. Debe tener en cuenta los pros y los contras.

## El patrón de cola de impresión - Revisado y rehabilitado

### Cómo evita el administrador de trabajos de cola exponer la API

Desacreditamos el patrón de Spooler en el último capítulo. Es el momento de rehabilitarlo.

![](assets/chapter-1/spooler-pattern.png)

El Patrón de cola de impresión evita el problema de exponer una API que hemos analizado en el último capítulo. Las propiedades se almacenan y encapsulan en el componente. Todo lo que necesitamos para acceder a estas propiedades es la ruta al componente. No es necesario utilizar la URL como vehículo para transmitir los parámetros entre el marcado y el procesamiento binario:

1. El cliente procesa el marcado de HTML cuando se solicita el componente dentro del bucle de solicitud principal

2. La ruta del componente sirve como referencia inversa desde el marcado al componente

3. El explorador utiliza esta referencia inversa para solicitar el binario

4. A medida que la solicitud llega al componente, tenemos todas las propiedades en nuestra mano para cambiar el tamaño, comprimir y cargar los datos binarios

5. La imagen se transmite mediante el componente al explorador del cliente

El Patrón de Spooler no es tan malo después de todo, por eso es tan popular. Si solo fuera poco engorroso cuando se trata de invalidar la caché...

### El Spooler Invertido - ¿Lo mejor de ambos mundos?

Eso nos lleva a la pregunta. ¿Por qué no podemos obtener lo mejor de ambos mundos? ¿La buena encapsulación del patrón de cola de impresión y las buenas propiedades de almacenamiento en caché de una URL basada en recursos?

Tenemos que admitir, que no hemos visto eso en un proyecto real en vivo. Pero vamos a atrevernos a un pequeño experimento mental aquí de todos modos - como punto de partida para su propia solución.

Llamaremos a este patrón el _Cola invertida_. El administrador de trabajos de impresión invertido debe basarse en el recurso de imágenes para tener todas las propiedades de invalidación de caché.

Pero no debe exponer ningún parámetro. Todas las propiedades deben encapsularse en el componente. Pero podemos exponer la ruta de los componentes, como una referencia opaca a las propiedades.

Esto conduce a una dirección URL con el siguiente formato:

`/content/dam/flower.respi3.content-mysite-home-jcrcontent-par-respi.jpg`

`/content/dam/flower` es la ruta al recurso de la imagen

`.respi3` es un selector para seleccionar el servlet correcto para entregar la imagen

`.content-mysite-home-jcrcontent-par-respi` es un selector adicional. Codifica la ruta al componente que almacena la propiedad necesaria para la transformación de la imagen. Los selectores están limitados a un intervalo de caracteres menor que las rutas. El esquema de codificación aquí es simplemente ejemplar. Sustituye &quot;/&quot; por &quot;-&quot;. No tiene en cuenta que la ruta en sí también puede contener &quot;-&quot;. En un ejemplo real se recomendaría un esquema de codificación más sofisticado. Base64 debe estar bien. Pero hace que la depuración sea un poco más difícil.

`.jpg` es el sufijo de archivos

### Conclusión

Vaya... la discusión del spooler se hizo más larga y complicada de lo esperado. Te debemos una excusa. Pero sentimos que es necesario presentarles una multitud de aspectos -buenos y malos- para que puedan desarrollar una cierta intuición sobre lo que funciona bien en Dispatcher-land y lo que no.

## Archivo de estado y Nivel de archivo de estado

### Conceptos básicos

#### Introducción

Ya hemos mencionado brevemente el _archivo de estado_ antes. Está relacionado con la invalidación automática:

Todos los archivos de caché del sistema de archivos de Dispatcher configurados para invalidarse automáticamente se consideran no válidos si la fecha de última modificación es anterior a la fecha de última modificación de `statfile's`.

>[!NOTE]
>
>La fecha de última modificación de la que hablamos es la del archivo en caché, es decir, la fecha en la que el archivo fue solicitado desde el navegador del cliente y finalmente creado en el sistema de archivos. No es la fecha `jcr:lastModified` del recurso.

La fecha de la última modificación del archivo de estado (`.stat`) es la fecha en la que se recibió la solicitud de invalidación de AEM en Dispatcher.

Si tiene más de una Dispatcher, esto puede producir efectos extraños. Su explorador puede tener una versión más reciente que Dispatchers (si tiene más de un Dispatcher). O un Dispatcher podría pensar que la versión del explorador emitida por el otro Dispatcher está obsoleta y envía una copia nueva innecesariamente. Estos efectos no tienen un impacto significativo en el rendimiento o en los requisitos funcionales. Y se nivelarán con el tiempo, cuando el navegador tenga la última versión. Sin embargo, puede resultar un poco confuso cuando optimiza y depura el comportamiento de almacenamiento en caché del explorador. Así que ten cuidado.

#### Configuración de dominios de invalidación con /statfileslevel

Cuando introdujimos la invalidación automática y el archivo de estado dijimos que *todos los* archivos se consideran no válidos cuando hay algún cambio y que todos los archivos son interdependientes de todos modos.

Eso no es del todo exacto. Normalmente, todos los archivos que comparten una raíz de navegación principal común son interdependientes. Sin embargo, una instancia de AEM puede alojar varios sitios web: *independientes*. No compartir una navegación común - de hecho, no compartir nada.

¿No sería un desperdicio en invalidar el Sitio B porque hay un cambio en el Sitio A? Sí, lo es. Y no tiene por qué ser así.

Dispatcher proporciona un medio sencillo para separar los sitios entre sí: `statfiles-level`.

Es un número que define a partir de qué nivel del sistema de archivos, dos subárboles se consideran &quot;independientes&quot;.

Veamos el caso predeterminado en el que el nivel de archivo de estado es 0.

![/statfileslevel &quot;0&quot;: _The_ _.stat_ _se crea en docroot. El dominio de invalidación abarca toda la instalación, incluidos todos los sitios &#x200B;](assets/chapter-1/statfile-level-0.png)

`/statfileslevel "0":`: el archivo `.stat` se crea en docroot. El dominio de invalidación abarca toda la instalación, incluidos todos los sitios.

Independientemente del archivo que se invalide, el archivo `.stat` en la parte superior del docroot de Dispatchers siempre se actualiza. Por lo tanto, al invalidar `/content/site-b/home`, también se invalidarán todos los archivos de `/content/site-a`, ya que ahora son más antiguos que el archivo `.stat` del docroot. Claramente no es lo que necesita, cuando invalida `site-b`.

En este ejemplo, preferiría establecer `statfileslevel` en `1`.

Ahora, si publica y, por lo tanto, invalida `/content/site-b/home` o cualquier otro recurso por debajo de `/content/site-b`, el archivo `.stat` se creará en `/content/site-b/`.

El contenido por debajo de `/content/site-a/` no se ve afectado. Este contenido se compararía con un archivo de `.stat` en `/content/site-a/`. Hemos creado dos dominios de invalidación independientes.

![Un statfileslevel &quot;1&quot; crea dominios de invalidación diferentes](assets/chapter-1/statfiles-level-1.png)

*Un statfileslevel &quot;1&quot; crea dominios de invalidación diferentes*

<br> 

Las instalaciones grandes suelen estar estructuradas un poco más complejas y profundas. Un esquema común es estructurar los sitios por marca, país e idioma. En ese caso, puede establecer el nivel de archivos de estado aún más alto. _1_ crearía dominios de invalidación por marca, _2_ por país y _3_ por idioma.

### Necesidad de una estructura homogénea del sitio

El nivel de archivos de estado se aplica por igual a todos los sitios de la configuración. Por lo tanto, es necesario tener todos los sitios que sigan la misma estructura y que comiencen en el mismo nivel.

Considere que tiene algunas marcas en su portafolio que se venden solo en unos pocos mercados pequeños, mientras que otras se venden en todo el mundo. Los mercados pequeños tienen un solo idioma local, mientras que en el mercado global hay países donde se habla más de un idioma:

```plain
  /content/tiny-local-brand/finland/home
  /content/tiny-local-brand/finland/products
  /content/tiny-local-brand/finland/about
                              ^
                          /statfileslevel "2"
  …

  /content/tiny-local-brand/norway
  …

  /content/shiny-global-brand/canada/en
  /content/shiny-global-brand/canada/fr
  /content/shiny-global-brand/switzerland/fr
  /content/shiny-global-brand/switzerland/de
  /content/shiny-global-brand/switzerland/it
                                          ^
                                /statfileslevel "3"
  ..
```

El primero requeriría un `statfileslevel` de _2_, mientras que el segundo requiere _3_.

No es una situación ideal. Si lo establece en _3_, la invalidación automática no funcionaría en los sitios más pequeños entre las subramas `/home`, `/products` y `/about`.

Si se establece en _2_, significa que en los sitios más grandes declarará que `/canada/en` y `/canada/fr` son dependientes, lo cual podría no ser así. Por lo tanto, cada invalidación en `/en` también invalidaría `/fr`. Esto llevará a una tasa de visitas a la caché ligeramente disminuida, pero sigue siendo mejor que enviar contenido en caché antiguo.

La mejor solución, por supuesto, es hacer que las raíces de todos los sitios sean igualmente profundas:

```
/content/tiny-local-brand/finland/fi/home
/content/tiny-local-brand/finland/fi/products
/content/tiny-local-brand/finland/fi/about
…
/content/tiny-local-brand/norway/no/home
                                 ^
                        /statfileslevel "3"
```

### Vinculación entre sitios

Ahora, ¿cuál es el nivel correcto? Depende del número de dependencias que tenga entre los sitios. Las inclusiones que resuelva para procesar una página se consideran &quot;dependencias fijas&quot;. Demostramos una _inclusión_ de este tipo cuando presentamos el componente _Teaser_ al principio de esta guía.

_Los hipervínculos_ son una forma más suave de dependencias. Es muy probable que tenga hipervínculos dentro de un sitio web... y no es improbable que tenga vínculos entre sus sitios web. Los hipervínculos simples no suelen crear dependencias entre sitios web. Solo piensa en un enlace externo que configuraste desde tu sitio a Facebook... No tendrías que procesar tu página si algo cambia en Facebook y viceversa, ¿verdad?

Se produce una dependencia al leer contenido del recurso vinculado (por ejemplo, el título de navegación). Estas dependencias se pueden evitar si se basan únicamente en títulos de navegación introducidos localmente y no se dibujan desde la página de destino (como se haría con los vínculos externos).

#### Una Dependencia Inesperada

Sin embargo, podría haber una parte de su configuración, donde los sitios -supuestamente independientes- se juntan. Veamos un escenario del mundo real que encontramos en uno de nuestros proyectos.

El cliente tenía una estructura del sitio como la esbozada en el último capítulo:

```
/content/brand/country/language
```

Por ejemplo,

```
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de

/content/shiny-brand/france/fr

/content/shiny-brand/germany/de
```

Cada país tenía su propio dominio,

```
www.shiny-brand.ch

www.shiny-brand.fr

www.shiny-brand.de
```

No había vínculos navegables entre los sitios de idiomas ni inclusiones aparentes, por lo que establecemos statfileslevel en 3.

Básicamente, todos los sitios ofrecían el mismo contenido. La única diferencia importante era el idioma.

Los motores de búsqueda como Google consideran que tener el mismo contenido en distintas URL es &quot;engañoso&quot;. Es posible que un usuario quiera obtener una clasificación más alta o una lista con más frecuencia creando granjas que sirvan contenido idéntico. Los motores de búsqueda reconocen estos intentos y en realidad clasifican las páginas por debajo que simplemente reciclan el contenido.

Puede evitar que se le reduzca la clasificación haciendo que sea transparente, que en realidad tenga más de una página con el mismo contenido y que no esté intentando &quot;jugar&quot; con el sistema (consulte [&quot;Informar a Google sobre las versiones localizadas de su página&quot;](https://support.google.com/webmasters/answer/189077?hl=en)) configurando las etiquetas `<link rel="alternate">` en cada página relacionada en la sección del encabezado de cada página:

```
# URL: www.shiny-brand.fr/fr/home/produits.html

<head>

  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch" 
        href="http://www.shiny-brand.ch/de/home/produkte.html">
  <link rel="alternate" 
        hreflang="de-de" 
        href="http://www.shiny-brand.de/de/home/produkte.html">

</head>

----

# URL www.shiny-brand.de/de/home/produkte.html

<head>

  <link rel="alternate" 
        hreflang="fr-fr" 
        href="http://www.shiny-brand.fr/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch"
         href="http://www.shiny-brand.ch/de/home/produits.html">

</head>
```

![Intervincular todo](assets/chapter-1/inter-linking-all.png)

*Intervincular todo*

<br> 

Algunos expertos en SEO incluso argumentan que esto podría transferir reputación o &quot;link-juice&quot; de un sitio web de alto rango en un idioma al mismo sitio web en un idioma diferente.

Este esquema creó no solo una serie de vínculos, sino también algunos problemas. El número de vínculos necesarios para _p_ en _n_ idiomas es _p x (n<sup>2</sup>-n)_: cada página vincula a otra página (_n x n_) excepto a sí misma (_-n_). Este esquema se aplica a cada página. Si tenemos un sitio pequeño en 4 idiomas con 20 páginas, cada uno equivale a _240_ vínculos.

Primero, no desea que un editor tenga que mantener manualmente estos vínculos: el sistema debe generarlos automáticamente.

Segundo, deben ser precisos. Siempre que el sistema detecta un nuevo &quot;relativo&quot;, quieres enlazarlo desde todas las demás páginas con el mismo contenido (pero en un idioma diferente).

En nuestro proyecto, las nuevas páginas relativas aparecían con frecuencia. Pero no se materializaron como enlaces &quot;alternativos&quot;. Por ejemplo, cuando la página `de-de/produkte` se publicó en el sitio web alemán, no era visible de inmediato en los demás sitios.

La razón era que en nuestra configuración los sitios se suponía que eran independientes. Así que un cambio en el sitio web alemán no déclencheur una anulación en el sitio web francés.

Ya conoce una solución para resolver ese problema. Simplemente reduzca el nivel de archivos de estado a 2 para ampliar el dominio de invalidación. Por supuesto, esto también reduce la proporción de visitas en caché, especialmente cuando se publican, y por lo tanto las invalidaciones se producen con más frecuencia.

En nuestro caso fue aún más complicado:

A pesar de que teníamos el mismo contenido, las marcas reales no eran diferentes en cada país.

`shiny-brand` se llamó a `marque-brillant` en Francia y a `blitzmarke` en Alemania:

```
/content/marque-brillant/france/fr
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de
/content/blitzmarke/germany/de
…
```

Eso habría significado establecer el nivel `statfiles` en 1, lo que habría resultado en un dominio de invalidación demasiado grande.

Reestructurar el sitio lo habría arreglado. Combinar todas las marcas en una raíz común. Pero no teníamos la capacidad en ese entonces, y eso nos habría dado sólo un nivel 2.

Decidimos quedarnos con el nivel 3 y pagamos el precio de no tener siempre enlaces &quot;alternativos&quot; actualizados. Para mitigarlo, teníamos un trabajo cron &quot;segador&quot; ejecutándose en el Dispatcher que limpiaba archivos anteriores a 1 semana de todos modos. Finalmente, todas las páginas fueron renderizadas de todos modos en algún momento. Sin embargo, ese es un equilibrio que debe decidirse individualmente en cada proyecto.

## Conclusión

Hemos visto algunos principios básicos sobre el funcionamiento de Dispatcher en general y le hemos dado algunos ejemplos en los que podría tener que realizar un poco más de esfuerzo de implementación para hacerlo correctamente y en los que podría querer realizar concesiones.

No hemos entrado en detalles sobre cómo se configura en Dispatcher. Queremos que entienda los conceptos básicos y los problemas primero, sin perderle a la consola demasiado pronto. Y, el trabajo de configuración real está bien documentado: si comprende los conceptos básicos, debe saber para qué se utilizan los distintos conmutadores.

## Sugerencias y trucos para Dispatcher

Concluiremos la primera parte de este libro con una colección aleatoria de pistas y trucos que podrían ser útiles en una situación u otra. Como hicimos antes, no estamos presentando la solución, sino la idea para que tenga la oportunidad de entender la idea y el concepto y el vínculo a artículos que describen la configuración real con más detalle.

### Corregir intervalos de invalidación

Si instala un autor y una publicación de AEM de forma predeterminada, la topología es un poco extraña. El autor envía el contenido a los sistemas de publicación y la solicitud de invalidación a las instancias de Dispatcher al mismo tiempo. Como tanto los sistemas de publicación como el Dispatcher están disociados del autor por las colas, el tiempo puede ser un poco desafortunado. Dispatcher puede recibir la solicitud de invalidación del autor antes de actualizar el contenido en el sistema de publicación.

Si un cliente solicita ese contenido mientras tanto, Dispatcher solicitará y almacenará contenido antiguo.

Una configuración más confiable está enviando la solicitud de invalidación desde los sistemas de publicación _después_ de que hayan recibido el contenido. El artículo &quot;[Invalidación de la caché de Dispatcher desde una instancia de publicación](https://helpx.adobe.com/es/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)&quot; describe los detalles.

**Referencias**

[helpx.adobe.com - Invalidando la caché de Dispatcher desde una instancia de publicación](https://helpx.adobe.com/es/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)

### Encabezado HTTP y almacenamiento en caché de encabezado

En los viejos tiempos, el Dispatcher simplemente almacenaba archivos simples en el sistema de archivos. Si necesitaba que se le enviaran encabezados HTTP al cliente, lo hizo configurando Apache según la poca información que tenía del archivo o la ubicación. Esto resultaba especialmente molesto cuando se implementaba una aplicación web en AEM que dependía en gran medida de encabezados HTTP. Todo funcionaba bien en la instancia solo de AEM, pero no cuando utilizaba un Dispatcher.

Normalmente empezó a volver a aplicar los encabezados que faltaban a los recursos en el servidor Apache con `mod_headers` utilizando información que podía derivar por la ruta de acceso y el sufijo de los recursos. Pero eso no siempre fue suficiente.

Fue especialmente molesto que, incluso con Dispatcher, la primera respuesta _sin almacenar en caché_ al explorador provino del sistema de publicación con una gama completa de encabezados, mientras que las respuestas posteriores fueron generadas por Dispatcher con un conjunto limitado de encabezados.

A partir de Dispatcher 4.1.11, Dispatcher puede almacenar los encabezados generados por los sistemas de publicación.

Esto evita la duplicación de lógica de encabezado en Dispatcher y libera todo el poder expresivo de HTTP y AEM.

**Referencias**

* [helpx.adobe.com - Almacenando en caché los encabezados de respuesta](https://helpx.adobe.com/experience-manager/kb/dispatcher-cache-response-headers.html)

### Excepciones de almacenamiento en caché individuales

Es posible que desee almacenar en caché todas las páginas e imágenes en general, pero hacer una excepción en algunas circunstancias. Por ejemplo, desea almacenar en caché imágenes PNG, pero no imágenes PNG que muestren un captcha (que se supone que debe cambiar en cada solicitud). Puede que Dispatcher no reconozca un captcha como captcha... pero AEM sí lo reconoce. Puede solicitar a Dispatcher que no almacene en caché esa solicitud única enviando un encabezado correspondiente junto con la respuesta:

```plain
  response.setHeader("Dispatcher", "no-cache");

  response.setHeader("Cache-Control: no-cache");

  response.setHeader("Cache-Control: private");

  response.setHeader("Pragma: no-cache");
```

Cache-Control y Pragma son encabezados HTTP oficiales, que se propagan e interpretan en capas de almacenamiento en caché superiores, como una CDN. El encabezado `Dispatcher` es solo una sugerencia para que Dispatcher no almacene en caché. Se puede utilizar para indicar a la Dispatcher que no almacene en caché, mientras se permite a las capas de almacenamiento en caché superiores que lo hagan. En realidad, es difícil encontrar un caso en el que eso pueda ser útil. Pero estamos seguros de que hay algunos, en alguna parte.

**Referencias**

* [Dispatcher - Sin caché](https://helpx.adobe.com/experience-manager/kb/DispatcherNoCache.html)

### Almacenamiento en caché del explorador

La respuesta http más rápida es la respuesta proporcionada por el propio explorador. Donde la solicitud y la respuesta no tienen que viajar a través de una red a un servidor web con una carga alta.

Puede ayudar al explorador a decidir cuándo solicitar al servidor una nueva versión del archivo estableciendo una fecha de caducidad en un recurso.

Normalmente, esto se hace de forma estática utilizando `mod_expires` de Apache o almacenando el encabezado Cache-Control y Expires que provienen de AEM si necesita un control más individual.

Un documento almacenado en caché en el explorador puede tener tres niveles de actualización.

1. _Nuevo garantizado_: el explorador puede utilizar el documento almacenado en caché.

2. _Potencialmente obsoleto_: el explorador debe preguntar primero al servidor si el documento almacenado en caché sigue estando actualizado,

3. _Antiguo_: el explorador debe solicitar una nueva versión al servidor.

La primera está garantizada por la fecha de caducidad establecida por el servidor. Si un recurso no ha caducado, no es necesario volver a preguntar al servidor.

Si el documento alcanza su fecha de caducidad, aún puede ser nuevo. La fecha de caducidad se establece cuando se entrega el documento. Sin embargo, a menudo no se sabe con antelación cuándo hay nuevo contenido disponible, por lo que esta es solo una estimación conservadora.

Para determinar si el documento de la caché del explorador sigue siendo el mismo que se entregaría en una nueva solicitud, el explorador puede utilizar la fecha `Last-Modified` del documento. El explorador pregunta al servidor:

&quot;_Tengo una versión del 10 de junio... ¿necesito una actualización?_&quot; Y el servidor podría responder con

&quot;_304 - Su versión aún está actualizada_&quot; sin volver a transmitir el recurso; de lo contrario, el servidor podría responder con

&quot;_200 - aquí hay una versión más reciente_&quot; en el encabezado HTTP y el contenido real más reciente en el cuerpo HTTP.

Para que funcione la segunda parte, asegúrese de transmitir la fecha de `Last-Modified` al explorador para que tenga un punto de referencia en el que solicitar actualizaciones.

Ya hemos explicado anteriormente que cuando Dispatcher genera la fecha `Last-Modified`, puede variar entre distintas solicitudes porque el archivo en caché (y su fecha) se genera cuando el explorador solicita el archivo. Una alternativa sería utilizar &quot;etiquetas electrónicas&quot;: se trata de números que identifican el contenido real (por ejemplo, generando un código hash) en lugar de una fecha.

&quot;[Soporte Etag](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)&quot; del _Paquete ACS Commons_ usa este enfoque. Sin embargo, esto tiene un precio: como la etiqueta electrónica debe enviarse como encabezado, pero el cálculo del código hash requiere leer completamente la respuesta, esta debe almacenarse en el búfer completo en la memoria principal antes de poder enviarse. Esto puede tener un impacto negativo en la latencia cuando el sitio web tiene más probabilidades de tener recursos sin caché y, por supuesto, debe vigilar la memoria consumida por el sistema de AEM.

Si utiliza huellas digitales de URL, puede establecer fechas de caducidad muy largas. Puede almacenar en caché los recursos de huellas digitales para siempre en el explorador. Una nueva versión se marca con una nueva URL y las versiones anteriores nunca tienen que actualizarse.

Utilizamos huellas digitales de URL cuando introdujimos el patrón de cola de impresión. Los archivos estáticos procedentes de `/etc/design` (CSS, JS) cambian con poca frecuencia, por lo que son buenos candidatos para utilizarlos como huellas digitales.

Para los archivos normales, normalmente configuramos un esquema fijo, como volver a comprobar HTML cada 30 minutos, imágenes cada 4 horas, etc.

El almacenamiento en caché del explorador es extremadamente útil en el sistema de creación. Desea almacenar en caché todo lo que pueda en el explorador para mejorar la experiencia de edición. Desafortunadamente, los recursos más caros, las páginas html no se pueden almacenar en caché... se supone que cambian con frecuencia en el autor.

Las bibliotecas de Granite, que conforman la interfaz de usuario de AEM, se pueden almacenar en caché durante bastante tiempo. También puede almacenar en caché los archivos estáticos del sitio (fuentes, CSS y JavaScript) en el explorador. Incluso las imágenes de `/content/dam` se pueden almacenar en caché durante unos 15 minutos, ya que no se modifican con la misma frecuencia con la que se copia texto en las páginas. Las imágenes no se editan interactivamente en AEM. Se editan y aprueban primero, antes de cargarse en AEM. Por lo tanto, puede suponer que no cambian con tanta frecuencia como el texto.

Almacenar en caché archivos de interfaz de usuario, archivos de biblioteca de sitios e imágenes puede acelerar significativamente la recarga de páginas cuando está en modo de edición.



**Referencias**

*[developer.mozilla.org - Almacenamiento en caché](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

* [apache.org - El modo caduca](https://httpd.apache.org/docs/current/mod/mod_expires.html)

* [ACS Commons - Compatibilidad con Etag](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)

### Truncamiento de URL

Los recursos se almacenan en

`/content/brand/country/language/…`

Pero, por supuesto, esta no es la dirección URL que desea mostrar al cliente. Por motivos de estética, legibilidad y optimización de los motores de búsqueda (SEO), es posible que desee truncar la parte que ya se representa en el nombre de dominio.

Si tiene un dominio

`www.shiny-brand.fi`

por lo general, no hay necesidad de poner la marca y el país en el camino. En lugar de,

`www.shiny-brand.fi/content/shiny-brand/finland/fi/home.html`

que le gustaría tener,

`www.shiny-brand.fi/home.html`

Debe implementar esa asignación en AEM, ya que AEM necesita saber cómo procesar los vínculos según ese formato truncado.

Pero no confíe solo en AEM. Si lo hace, tendría rutas de acceso como `/home.html` en el directorio raíz de la caché. Ahora, ¿es esa la &quot;casa&quot; para el sitio web finlandés o alemán o canadiense? Y si hay un archivo `/home.html` en Dispatcher, ¿cómo sabe Dispatcher que esto debe invalidarse cuando llega una solicitud de invalidación de `/content/brand/fi/fi/home`?

Hemos visto un proyecto que tenía docroot separados para cada dominio. Fue una pesadilla depurar y mantener, y en realidad nunca lo vimos funcionar a la perfección.

Podríamos resolver los problemas reestructurando la caché. Teníamos un único docroot para todos los dominios, y las solicitudes de invalidación se pudieron administrar 1:1, ya que todos los archivos del servidor empezaron con `/content`.

La parte truncada también fue muy fácil.  AEM generó vínculos truncados debido a una configuración acorde en `/etc/map`.

Ahora, cuando una solicitud `/home.html` llega a Dispatcher, lo primero que sucede es aplicar una regla de reescritura que expande internamente la ruta.

Esa regla se configuraba de forma estática en cada configuración de vhost. En pocas palabras, las reglas se veían así,

```plain
  # vhost www.shiny-brand.fi

  RewriteRule "^(.\*\.html)" "/content/shiny-brand/finland/fi/$1"
```

En el sistema de archivos ahora tenemos rutas simples basadas en `/content`, que se encontrarían también en Author y Publish, lo que ayudó mucho a depurar. Por no hablar de la invalidación correcta - que ya no era un problema.

Tenga en cuenta que solo lo hicimos para direcciones URL &quot;visibles&quot;, direcciones URL que se muestran en la ranura de dirección URL del explorador. Las URL para imágenes, por ejemplo, seguían siendo URL puras de &quot;/content&quot;. Creemos que embellecer la URL &quot;principal&quot; es suficiente en términos de optimización del motor de búsqueda.

Tener un docroot común también tenía otra buena característica. Cuando algo salía mal en Dispatcher, podíamos limpiar toda la caché ejecutando,

`rm -rf /cache/dispatcher/*`

(algo que puede no querer hacer en picos de carga altos).

**Referencias**

* [apache.org - Reescritura de modo](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)

* [helpx.adobe.com - Asignación de recursos](https://helpx.adobe.com/es/experience-manager/6-4/sites/deploying/using/resource-mapping.html)

### Gestión de errores

En las clases de AEM, aprenderá a programar un controlador de error en Sling. Esto no es tan diferente de escribir una plantilla habitual. Simplemente escribe una plantilla en JSP o HTL, ¿verdad?

Sí, pero solo esta es la parte de AEM. Recuerde: Dispatcher no almacena en caché las respuestas de `404 – not found` o `500 – internal server error`.

Si está procesando estas páginas dinámicamente en cada solicitud (fallida), tendrá una carga alta innecesaria en los sistemas de publicación.

Lo que nos ha resultado útil es no procesar la página de error completa cuando se produce un error, sino solo una versión súper simplificada y pequeña, incluso estática, de esa página, sin adornos ni lógica.

Esto, por supuesto, no es lo que vio el cliente. En Dispatcher, registramos `ErrorDocuments` de la siguiente manera:

```
ErrorDocument 404 "/content/shiny-brand/fi/fi/edocs/error-404.html"
ErrorDocument 500 "/content/shiny-brand/fi/fi/edocs/error-500.html"
```

Ahora el sistema AEM podría notificar al Dispatcher que algo estaba mal, y el Dispatcher podría entregar una versión brillante y hermosa del documento de error.

Hay que señalar dos cosas aquí.

Primero, `error-404.html` siempre es la misma página. Por lo tanto, no hay ningún mensaje individual como &quot;Su búsqueda de &quot;_produkten_&quot; no arrojó un resultado&quot;. Podríamos vivir fácilmente con eso.

Segundo... bueno, si vemos un error interno del servidor - o peor aún, nos encontramos con una interrupción del sistema de AEM, no hay forma de pedir a AEM que procese una página de error, ¿verdad? La solicitud posterior necesaria, tal como se define en la directiva `ErrorDocument`, también daría error. Hemos solucionado ese problema ejecutando un trabajo cron que periódicamente extraería las páginas de error de sus ubicaciones definidas a través de `wget` y las almacenaría en ubicaciones de archivos estáticos definidas en la directiva `ErrorDocuments`.

**Referencias**

* [apache.org - Documentos de error personalizados](https://httpd.apache.org/docs/2.4/custom-error.html)

### Almacenar contenido protegido en caché

Dispatcher no comprueba los permisos cuando envía un recurso de forma predeterminada. Se implementa de esta manera a propósito: para acelerar su sitio web público. Si desea proteger algunos recursos mediante un inicio de sesión, básicamente tiene tres opciones:

1. Proteja el recurso antes de que la solicitud llegue a la caché; es decir, por una puerta de enlace de inicio de sesión único (SSO) delante de Dispatcher o como módulo en el servidor Apache

2. Excluya los recursos confidenciales de la caché y, por lo tanto, envíelos siempre en directo desde el sistema de publicación.

3. Utilice el almacenamiento en caché con permisos confidenciales en Dispatcher.

Y, por supuesto, se puede aplicar su propia combinación de los tres enfoques.

**Opción 1**. Su organización podría aplicar una puerta de enlace &quot;SSO&quot; de todas formas. Si el esquema de acceso es muy básico, es posible que no necesite información de AEM para decidir si conceder o denegar el acceso a un recurso.

>[!NOTE]
>
>Este patrón requiere una _puerta de enlace_ que _intercepta_ cada solicitud y realiza la _autorización_ real: conceder o denegar solicitudes a Dispatcher. Si su sistema SSO es un _autenticador_, eso solo establece la identidad de un usuario, debe implementar la Opción 3. Si lee términos como &quot;SAML&quot; u &quot;OAauth&quot; en el manual de su sistema SSO, ese es un indicador sólido de que debe implementar la Opción 3.


**Opción 2**. &quot;No almacenar en caché&quot; generalmente es una mala idea. Si va en esa dirección, asegúrese de que la cantidad de tráfico y el número de recursos confidenciales que se excluyen sean pequeños. O asegúrese de tener instalada alguna caché en memoria en el sistema de publicación, de que los sistemas de publicación pueden manejar la carga resultante, más información en la Parte III de esta serie.

**Opción 3**. &quot;Almacenamiento en caché con permisos confidenciales&quot; es un enfoque interesante. Dispatcher está almacenando en caché un recurso, pero antes de enviarlo, pregunta al sistema de AEM si puede hacerlo. Esto crea una solicitud adicional de Dispatcher a la publicación, pero normalmente evita que el sistema de publicación vuelva a procesar una página si ya se ha almacenado en caché. Sin embargo, este enfoque requiere cierta implementación personalizada. Encuentre detalles aquí en el artículo [Almacenamiento en caché con permisos confidenciales](https://helpx.adobe.com/es/experience-manager/dispatcher/using/permissions-cache.html).

**Referencias**

* [helpx.adobe.com - Almacenamiento en caché con permisos confidenciales](https://helpx.adobe.com/es/experience-manager/dispatcher/using/permissions-cache.html)

### Configuración del período de gracia

Si invalida con frecuencia en una sucesión corta, por ejemplo, mediante la activación de un árbol o simplemente por necesidad de mantener el contenido actualizado, puede ocurrir que esté vaciando constantemente la caché y que los visitantes casi siempre estén alcanzando una caché vacía.

El diagrama siguiente ilustra un posible tiempo al acceder a una sola página.  Por supuesto, el problema empeora cuando el número de páginas diferentes solicitadas aumenta.

![Activaciones frecuentes que llevan a una caché no válida la mayor parte del tiempo](assets/chapter-1/frequent-activations.png)

*Activaciones frecuentes que llevan a una caché no válida la mayor parte del tiempo*

<br> 

Para mitigar el problema de esta &quot;tormenta de invalidación de caché&quot; como a veces se la llama, puede ser menos riguroso con la interpretación de `statfile`.

Puede configurar Dispatcher para que use `grace period` para la invalidación automática. Esto agregaría internamente algún tiempo adicional a la fecha de modificación de `statfiles`.

Digamos que su `statfile` tiene una hora de modificación de hoy 12:00 y su `gracePeriod` está establecido en 2 minutos. A continuación, todos los archivos invalidados automáticamente se considerarán válidos a las 12:01 y a las 12:02. Se vuelven a procesar después de 12:02.

La configuración de referencia propone un(a) `gracePeriod` de dos minutos por una buena razón. Podrías pensar: &quot;¿Dos minutos? Eso es casi nada. Puedo esperar fácilmente 10 minutos para que el contenido aparezca...&quot;.  Por lo tanto, podría verse tentado a establecer un periodo más largo (por ejemplo, 10 minutos), suponiendo que el contenido se muestre al menos después de estos 10 minutos.

>[!WARNING]
>
>Así no es como `gracePeriod` está funcionando. El período de gracia es _no_ el tiempo después del cual se garantiza la invalidación de un documento, pero no se produce ninguna invalidación en un período de tiempo. Cada invalidación subsiguiente que cae dentro de este marco _prolonga_ el lapso de tiempo, lo cual puede ser indefinidamente largo.

Vamos a ilustrar cómo está funcionando `gracePeriod` con un ejemplo:

Supongamos que gestiona un sitio multimedia y que el personal de edición le ofrece actualizaciones de contenido periódicas cada 5 minutos. Considere la posibilidad de establecer gracePeriod en 5 minutos.

Empezaremos con un ejemplo rápido a las 12:00.

12:00 - El archivo de estado está establecido en 12:00. Todos los archivos en caché se consideran válidos hasta el 12:05.

12:01: se produce una invalidación. Esto prolonga el tiempo de la parrilla a 12:06

12:05 - Otro editor publica este artículo - prolongando el tiempo de gracia por otro gracePeriod a 12:10.

Y así sucesivamente... el contenido nunca se invalida. Cada invalidación *en* gracePeriod prolonga efectivamente el tiempo de gracia. El `gracePeriod` está diseñado para capear la tormenta de invalidación... pero debes salir a la lluvia con el tiempo... así que mantén el `gracePeriod` considerablemente corto para evitar esconderte en el refugio para siempre.

#### Un período de gracia determinista

Nos gustaría introducir otra idea de cómo podría capear una tormenta de invalidación. Es sólo una idea. No lo hemos probado en producción, pero encontramos el concepto lo suficientemente interesante como para compartir la idea con ustedes.

`gracePeriod` puede llegar a ser impredeciblemente largo, si el intervalo de replicación regular es más corto que `gracePeriod`.

La idea alternativa es la siguiente: invalidar solo en intervalos de tiempo fijos. El tiempo intermedio siempre significa ofrecer contenido antiguo. La invalidación se producirá con el tiempo, pero se recopilarán varias invalidaciones para una invalidación &quot;masiva&quot;, de modo que Dispatcher tenga la oportunidad de servir contenido en caché mientras tanto y dar al sistema de publicación un poco de aire para respirar.

La implementación tendría este aspecto:

Utiliza una &quot;Secuencia de comandos de invalidación personalizada&quot; (consulte la referencia) que se ejecutaría después de producirse la invalidación. Este script leería la fecha de la última modificación de `statfile's` y la redondearía hasta la siguiente parada de intervalo. El comando de shell de Unix `touch --time`, le permite especificar una hora.

Por ejemplo, si establece el período de gracia en 30 segundos, Dispatcher redondeará la última fecha de modificación del archivo de estado a los siguientes 30 segundos. Las solicitudes de invalidación que se producen entre una y otra simplemente establecen los mismos 30 segundos completos siguientes.

![Si se pospone la invalidación hasta los siguientes 30 segundos completos, aumenta la tasa de aciertos.](assets/chapter-1/postponing-the-invalidation.png)

*Si se pospone la invalidación hasta los siguientes 30 segundos completos, aumenta la tasa de aciertos.*

<br> 

Las visitas de caché que se producen entre la solicitud de invalidación y la siguiente ranura de ronda de 30 segundos se consideran obsoletas. Se ha producido una actualización en la publicación, pero el Dispatcher sigue ofreciendo contenido antiguo.

Este enfoque podría ayudar a definir periodos de gracia más largos sin tener que temer que las solicitudes posteriores prolonguen el periodo de forma indeterminista. Aunque como dijimos antes - es solo una idea y no tuvimos la oportunidad de probarla.

**Referencias**

[helpx.adobe.com - Configuración de Dispatcher](https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher-configuration.html)

### Recuperación automática de datos

Su sitio tiene un patrón de acceso muy particular. Tiene una alta carga de tráfico entrante y la mayor parte del tráfico se concentra en una pequeña fracción de sus páginas. La página de inicio, las páginas de aterrizaje de la campaña y las páginas de detalles del producto más destacadas reciben el 90 % del tráfico. O si opera un sitio nuevo, los artículos más recientes tienen números de tráfico más altos en comparación con los más antiguos.

Es muy probable que estas páginas se almacenen en caché en Dispatcher, ya que se solicitan con tanta frecuencia.

Se envía una solicitud de invalidación arbitraria a Dispatcher, lo que provoca que todas las páginas, incluida la más popular, se invaliden.

Posteriormente, como estas páginas son tan populares, hay nuevas solicitudes entrantes de diferentes navegadores. Veamos la página principal como ejemplo.

Como ahora la caché no es válida, todas las solicitudes a la página principal que llegan al mismo tiempo se reenvían al sistema de publicación y generan una carga alta.

![Solicitudes paralelas al mismo recurso en caché vacía: las solicitudes se reenvían a Publish](assets/chapter-1/parallel-requests.png)

*Solicitudes paralelas al mismo recurso en caché vacía: las solicitudes se reenvían a Publish*

Con la recuperación automática, puede mitigar eso hasta cierto punto. La mayoría de las páginas invalidadas siguen almacenadas físicamente en Dispatcher después de la invalidación automática. Solo se consideran _obsoletos_. La _recuperación automática_ significa que aún servirá estas páginas obsoletas durante unos segundos mientras inicia _una única solicitud_ al sistema de publicación para recuperar el contenido obsoleto:

![Entrega de contenido obsoleto mientras se recupera en segundo plano](assets/chapter-1/fetching-background.png)

*Entrega de contenido obsoleto mientras se recupera en segundo plano*

<br> 

Para habilitar la recuperación, debe indicar a Dispatcher qué recursos recuperar después de una invalidación automática. Recuerde que cualquier página que active también invalida automáticamente todas las demás páginas, incluidas las populares.

Recuperar significa decir a la Dispatcher en cada solicitud de invalidación (!) que desea recuperar las más populares y cuáles son las más populares.

Esto se logra colocando una lista de URL de recursos (URL reales, no solo rutas) en el cuerpo de las solicitudes de invalidación:

```
POST /dispatcher/invalidate.cache HTTP/1.1

CQ-Action: Activate
CQ-Handle: /content/my-brand/home/path/to/some/resource
Content-Type: Text/Plain
Content-Length: 207

/content/my-brand/home.html
/content/my-brand/campaigns/landing-page-1.html
/content/my-brand/campaigns/landing-page-2.html
/content/my-brand/products/product-1.html
/content/my-brand/products/product-2.html
```

Cuando Dispatcher déclencheur vea una solicitud de este tipo, se invalidará automáticamente como de costumbre y se pondrán en cola inmediatamente las solicitudes para recuperar contenido nuevo del sistema de publicación.

Como ahora estamos utilizando un cuerpo de solicitud, también necesitamos establecer el tipo de contenido y la longitud del contenido según el estándar HTTP.

Dispatcher también marca internamente las direcciones URL correspondientes para saber que puede entregar estos recursos directamente aunque se consideren no válidos mediante la invalidación automática.

Todas las direcciones URL enumeradas se solicitan una por una. Por lo tanto, no es necesario preocuparse por crear una carga demasiado alta en los sistemas de publicación. Pero tampoco le gustaría incluir demasiadas direcciones URL en esa lista. Al final, la cola debe procesarse finalmente en un tiempo limitado para no mostrar contenido antiguo durante demasiado tiempo. Incluya las 10 páginas a las que accede con más frecuencia.

Si busca en el directorio de caché de Dispatcher, verá los archivos temporales marcados con marcas de tiempo. Estos son los archivos que se están cargando en segundo plano.

**Referencias**

[helpx.adobe.com - Invalidar páginas en la caché de AEM](https://helpx.adobe.com/es/experience-manager/dispatcher/using/page-invalidate.html)

### Blindaje del sistema de publicación

Dispatcher proporciona un poco de seguridad adicional al proteger el sistema de publicación de solicitudes que solo se dirigen a fines de mantenimiento. Por ejemplo, no desea exponer al público las direcciones URL de `/crx/de` o `/system/console`.

No es perjudicial tener instalado en el sistema un cortafuegos de aplicaciones web (WAF). Pero eso añade un número significativo a su presupuesto y no todos los proyectos se encuentran en una situación en la que pueden permitirse y, no olvidemos, operar y mantener un WAF.

Lo que vemos con frecuencia es un conjunto de reglas de reescritura de Apache en la configuración de Dispatcher que impiden el acceso a los recursos más vulnerables.

Pero también puede considerar un enfoque diferente:

Según la configuración de Dispatcher, el módulo de Dispatcher está enlazado a un directorio determinado:

```
<Directory />
  SetHandler dispatcher-handler
  …
</Directory>
```

Pero, ¿por qué enlazar el controlador a todo docroot, cuando necesita filtrar posteriormente?

En primer lugar, puede reducir el enlace del controlador. `SetHandler` solo enlaza un controlador a un directorio, puede enlazar el controlador a una dirección URL o a un patrón de URL:

```
<LocationMatch "^(/content|/etc/design|/dispatcher/invalidate.cache)/.\*">
  SetHandler dispatcher-handler
</LocationMatch>

<LocationMatch "^/dispatcher/invalidate.cache">
  SetHandler dispatcher-handler
</LocationMatch>

…
```

Si lo hace, no olvide enlazar siempre el controlador de Dispatcher a la URL de invalidación de Dispatcher; de lo contrario, no podrá enviar solicitudes de invalidación de AEM a Dispatcher.

Otra alternativa para usar Dispatcher como filtro es configurar directivas de filtro en `dispatcher.any`

```
/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }
```

No estamos exigiendo el uso de una directiva sobre la otra, sino que recomendamos una combinación adecuada de todas las directivas.

Sin embargo, le proponemos que considere la posibilidad de reducir el espacio de URL lo más pronto posible en la cadena, tanto como sea necesario, y que lo haga de la manera más sencilla posible. No obstante, tenga en cuenta que estas técnicas no sustituyen a WAF en sitios web muy confidenciales. Algunas personas llaman a estas técnicas &quot;cortafuegos de hombre pobre&quot; - por una razón.

**Referencias**

[apache.org- directiva sethandler](https://httpd.apache.org/docs/2.4/mod/core.html#sethandler)

[helpx.adobe.com - Configurando el acceso al filtro de contenido](https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher-configuration.html#ConfiguringAccesstoContentfilter)

### Filtrado mediante expresiones regulares y globs

En los primeros días solo podía usar &quot;globs&quot;: marcadores de posición simples para definir filtros en la configuración de Dispatcher.

Por suerte, esto ha cambiado en las versiones posteriores de Dispatcher. Ahora también puede utilizar expresiones regulares POSIX y puede acceder a varias partes de una solicitud para definir un filtro. Para alguien que acaba de empezar con el Dispatcher que podría darse por sentado. Pero si estás acostumbrado a tener globos solamente, es una sorpresa y fácilmente se puede pasar por alto. Además, la sintaxis de globs y regex es demasiado similar. Vamos a comparar dos versiones que hacen lo mismo:

```
# Version A

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }

# Version B

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url '/content.\*'  }
```

¿Ves la diferencia?

La versión B usa comillas simples `'` para marcar un _patrón de expresión regular_. &quot;Cualquier carácter&quot; se expresa mediante `.*`.

_Patrones globbing_, en cambio usa comillas dobles `"` y solamente puedes usar marcadores de posición simples como `*`.

Si conoce esa diferencia, es trivial, pero si no, puede confundir fácilmente las citas y pasar una tarde soleada depurando su configuración. Ahora estás advertido.

&quot;Reconozco `'/url'` en la configuración... Pero, ¿qué es ese(a) `'/glob'` en el filtro que puede preguntar?

Esa directiva representa toda la cadena de solicitud, incluidos el método y la ruta de acceso. Podría representar...

`"GET /content/foo/bar.html HTTP/1.1"`

esta es la cadena con la que se compararía su patrón. Los principiantes tienden a olvidar la primera parte, la `method` (GET, POST, ...). Entonces, un patrón

`/0002  { /glob "/content/\*" /type "allow" }`

Siempre fallaría, ya que &quot;/content&quot; no coincide con &quot;GET ..&quot; de la solicitud.

Así que cuando quieras usar Globs,

`/0002  { /glob "GET /content/\*" /type "allow" }`

sería correcto.

Para una regla de denegación inicial, como

`/0001  { /glob "\*" /type "deny" }`

esto está bien. Sin embargo, para las siguientes permitidas, es mejor y más claro, más expresivo y mucho más seguro utilizar las partes individuales de una solicitud:

```
/method
/url
/path
/selector
/extension
/suffix
```

Así es:

```
/005  {

  /type "allow"
  /method "GET"
  /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

Tenga en cuenta que puede mezclar expresiones regex y glob en una regla.

Una última palabra sobre los &quot;números de línea&quot; como `/005` delante de cada definición,

¡No tienen ningún significado! Puede elegir denominadores arbitrarios para las reglas. El uso de números no requiere mucho esfuerzo para pensar en un esquema, pero tenga en cuenta que el orden importa.

Si tiene cientos de reglas como estas:

```
/001
/002
/003
…
/100
…
```

y desea insertar uno entre /001 y /002 ¿qué sucede con los números subsiguientes? ¿Estás aumentando su número? ¿Está insertando números intermedios?

```
/001
/001a
/002
/003
…
/100
…
```

¿O qué pasa si cambia para reordenar /003 y /001, va a cambiar los nombres y sus identidades o es usted

```
/003
/002
/001
…
/100
…
```

La numeración, aunque parezca una elección simple en primer lugar, alcanza sus límites a largo plazo. Seamos honestos, elegir números como identificadores es un mal estilo de programación de todos modos.

Nos gustaría proponer un enfoque diferente: lo más probable es que no se le ocurran identificadores significativos para cada regla de filtro individual. Pero probablemente sirven para un propósito mayor, por lo que pueden agruparse de algunas maneras según ese propósito. Por ejemplo, &quot;configuración básica&quot;, &quot;excepciones específicas de la aplicación&quot;, &quot;excepciones globales&quot; y &quot;seguridad&quot;.

A continuación, puede asignar un nombre a las reglas y agruparlas en consecuencia, y proporcionar al lector de la configuración (su querido colega) cierta orientación en el archivo:

```plain
  # basic setup:

  /filter {

    # basic setup

    /basic_01  { /glob "\*"             /type "deny"  }
    /basic_02  { /glob "/content/\*"    /type "allow" }
    /basic_03  { /glob "/etc/design/\*" /type "allow" }

    /basic_04  { /extension '(json|xml)'  /type "deny"  }
    …


    # login

    /login_01 { /glob "/api/myapp/login/\*" /type "allow" }
    /login_02 { … }

    # global exceptions

    /global_01 { /method "POST" /url '.\*contact-form.html' }
```


Lo más probable es que añada una nueva regla a uno de los grupos o que incluso cree un nuevo grupo. En ese caso, el número de elementos para cambiar el nombre o la numeración se limita a ese grupo.

>[!WARNING]
>
>Configuraciones más sofisticadas dividen las reglas de filtrado en varios archivos, incluidos en el archivo de configuración principal de `dispatcher.any`. Sin embargo, un nuevo archivo no introduce una nueva área de nombres. Por lo tanto, si tiene una regla &quot;001&quot; en un archivo y &quot;001&quot; en otro, obtendrá un error. Aún más razones para encontrar nombres semánticamente fuertes.

**Referencias**

[helpx.adobe.com - Diseñando patrones para propiedades glob](https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher-configuration.html#DesigningPatternsforglobProperties)

### Especificación de protocolo

El último consejo no es real, pero sentimos que valía la pena compartirlo con usted de todos modos.

AEM y Dispatcher funcionan en la mayoría de los casos de forma predeterminada. Por lo tanto, no encontrará una especificación de protocolo Dispatcher completa acerca del protocolo de invalidación para crear su propia aplicación sobre. La información es pública, pero un poco dispersa sobre una serie de recursos.

Tratamos de llenar el vacío hasta cierto punto aquí. Este es el aspecto de una solicitud de invalidación:

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: <action>
CQ-Handle: <path-pattern>
[CQ-Action-Scope]
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]

<newline>

<refetch-url-1>
<refetch-url-2>

…

<refetch-url-n>
```

`POST /dispatcher/invalidate.cache HTTP/1.1`: la primera línea es la dirección URL del extremo de control de Dispatcher y es probable que no la cambie.

`CQ-Action: <action>` - Qué debería suceder. `<action>` puede ser:

* `Activate:` elimina `/path-pattern.*`
* `Deactive:` eliminar `/path-pattern.*`
Y eliminar `/path-pattern/*`
* `Delete:`   eliminar `/path-pattern.*`
Y eliminar `/path-pattern/*`
* `Test:`   Devolver &quot;ok&quot; pero no hacer nada

`CQ-Handle: <path-pattern>`: ruta de acceso de recurso de contenido que se va a invalidar. Tenga en cuenta que `<path-pattern>` es en realidad una &quot;ruta de acceso&quot; y no un &quot;patrón&quot;.

`CQ-Action-Scope: ResourceOnly` - Opcional: si se establece este encabezado, no se tocará el archivo `.stat`.

```
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]
```

Establezca estos encabezados si define una lista de direcciones URL de recuperación automática. `<bytes in request body>` es el número de caracteres del cuerpo HTTP

`<newline>`: si tiene un cuerpo de solicitud, debe estar separado del encabezado por una fila vacía.

```
<refetch-url-1>
<refetch-url-2>
…
<refetch-url-n>
```

Enumerar las direcciones URL que desea recuperar inmediatamente después de la invalidación.

## Recursos adicionales

Información general e introducción al almacenamiento en caché de Dispatcher: [https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher.html](https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher.html)

Documentación de Dispatcher con todas las directivas explicadas: [https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher-configuration.html](https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher-configuration.html)

Algunas preguntas más frecuentes: [https://helpx.adobe.com/es/experience-manager/using/dispatcher-faq.html](https://helpx.adobe.com/es/experience-manager/using/dispatcher-faq.html)

Grabación de un seminario web acerca de la optimización de Dispatcher: muy recomendado: [https://my.adobeconnect.com/p7th2gf8k43?proto=true](https://my.adobeconnect.com/p7th2gf8k43?proto=true)

Presentación &quot;El poder subestimado de la invalidación de contenido&quot;, conferencia &quot;adaptTo()&quot; en Potsdam 2018 [https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html](https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html)

Invalidar páginas en la caché de AEM: [https://helpx.adobe.com/es/experience-manager/dispatcher/using/page-invalidate.html](https://helpx.adobe.com/es/experience-manager/dispatcher/using/page-invalidate.html)

## Siguiente paso

* [2 - Patrón de infraestructura](chapter-2.md)
