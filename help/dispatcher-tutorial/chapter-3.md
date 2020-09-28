---
title: Capítulo 3 - Temas avanzados de almacenamiento en caché
seo-title: Desmitificación de la caché de despachantes de AEM - Capítulo 3 - Temas de almacenamiento en caché avanzados
description: El capítulo 3 del tutorial Desmitificado de la caché del despachante de AEM describe cómo superar las limitaciones descritas en el capítulo 2.
seo-description: El capítulo 3 del tutorial Desmitificado de la caché del despachante de AEM describe cómo superar las limitaciones descritas en el capítulo 2.
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '6187'
ht-degree: 0%

---


# Capítulo 3 - Temas avanzados de almacenamiento en caché

*&quot;Hay sólo dos cosas difíciles en Ciencias de la Computación: invalidación de caché y asignación de nombres&quot;.*

— PHIL KARLTON

## Información general

Esta es la tercera parte de una serie de tres partes a la caché en AEM. Las dos primeras partes se centraban en el almacenamiento en caché http sin formato en el despachante y en las limitaciones que existían. Esta parte analiza algunas ideas sobre cómo superar estas limitaciones.

## Almacenamiento en caché en general

[El capítulo 1](chapter-1.md) y el [capítulo 2](chapter-2.md) de esta serie se centraron principalmente en el despachante. Hemos explicado los conceptos básicos, las limitaciones y dónde debe realizar ciertas compensaciones.

La complejidad y complejidad del almacenamiento en caché no son problemas exclusivos del despachante. El almacenamiento en caché es difícil en general.

Tener al despachante como la única herramienta en la caja de herramientas sería una verdadera limitación.

En este capítulo queremos ampliar nuestra vista sobre el almacenamiento en caché y desarrollar algunas ideas sobre cómo superar algunas de las deficiencias del despachante. No hay una panacea - tendrás que hacer concesiones en tu proyecto. Recuerde que con el almacenamiento en caché y la precisión de invalidación siempre viene la complejidad, y con la complejidad viene la posibilidad de errores.

Tendrá que hacer concesiones en estas áreas,

* Rendimiento y latencia
* Consumo de recursos / Carga de CPU / Uso de disco
* Precisión / Moneda / Estancia / Seguridad
* Simplicidad / Complejidad / Costo / Sostenibilidad / Pronunciación de errores

Estas dimensiones están interrelacionadas en un sistema bastante complejo. No hay un simple &quot;if-this-then-that&quot;. Hacer que un sistema sea más simple puede hacerlo más rápido o más lento. Puede reducir los costes de desarrollo, pero aumentar los costes en el servicio de asistencia, por ejemplo, si los clientes ven contenido antiguo o se quejan de un sitio web lento. Todos estos factores deben considerarse y equilibrarse entre sí. Pero a estas alturas ya deberían tener una buena idea, que no hay una panacea o una sola &quot;mejor práctica&quot; -sólo un montón de malas prácticas y unas pocas buenas.

## Almacenamiento en caché encadenado

### Información general

#### Flujo de datos

El envío de una página desde un servidor al explorador de un cliente cruza una gran cantidad de sistemas y subsistemas. Si se mira con cuidado, hay una cantidad de datos de saltos que deben tomarse del origen al drenaje, cada uno de los cuales es un candidato potencial para el almacenamiento en caché.

![Flujo de datos de una aplicación típica de CMS](assets/chapter-3/data-flow-typical-cms-app.png)

*Flujo de datos de una aplicación típica de CMS*

<br> 

Vamos a inicio nuestro viaje con un fragmento de datos que se encuentra en un disco duro y que necesita mostrarse en un navegador.

#### Hardware y sistema operativo

En primer lugar, la unidad de disco duro (HDD) tiene una caché integrada en el hardware. En segundo lugar, el sistema operativo que monta el disco duro utiliza memoria libre para almacenar en caché los bloques a los que se accede con frecuencia a fin de acelerar el acceso.

#### Repositorio de contenido

El siguiente nivel es CRX u Oak, la base de datos de documento utilizada por AEM. CRX y Oak dividen los datos en segmentos que pueden almacenarse en caché en la memoria, así como evitar un acceso más lento al disco duro.

#### Datos de terceros

La mayoría de las instalaciones Web más grandes tienen datos de terceros también; datos procedentes de un sistema de información del producto, un sistema de gestión de la relación con el cliente, una base de datos heredada o cualquier otro servicio Web arbitrario. No es necesario extraer estos datos de la fuente en ningún momento, especialmente cuando se sabe que cambian con poca frecuencia. Por lo tanto, se puede almacenar en caché si no se sincroniza en la base de datos de CRX.

#### Nivel comercial: aplicación/modelo

Normalmente, las secuencias de comandos de plantilla no representan el contenido sin procesar procedente de CRX mediante la API de JCR. Lo más probable es que tenga una capa comercial entre esas combinaciones, calcule o transforme datos en un objeto de dominio comercial. Adivinen qué: si estas operaciones son costosas, debe considerar almacenarlas en caché.

#### Fragmentos de marcado

El modelo es ahora la base para el procesamiento de la marca de un componente. ¿Por qué no almacenar también en caché el modelo procesado?

#### Dispatcher, CDN y otros proxies

Desactivado dirige la página HTML representada al despachante. Ya hemos discutido que el propósito principal del despachante es almacenar en caché las páginas HTML y otros recursos web (a pesar de su nombre). Antes de que los recursos lleguen al navegador, puede pasar un proxy inverso, que puede almacenar en caché y un CDN, que también se utiliza para almacenar en caché. El cliente puede sentarse en una oficina, que otorga acceso a la web solamente a través de un proxy, y ese proxy podría decidir almacenar en caché también para ahorrar tráfico.

#### Caché del explorador

Por último, pero no menos importante: el navegador también se almacena en caché. Se trata de un recurso que se pasa por alto fácilmente. Pero es la memoria caché más cercana y rápida que tiene en la cadena de almacenamiento en caché. Desafortunadamente, no se comparte entre los usuarios, sino entre diferentes solicitudes de un usuario.

### Dónde almacenar en caché y por qué

Esa es una larga cadena de cachés potenciales. Y todos hemos enfrentado problemas donde hemos visto contenido anticuado. Pero teniendo en cuenta cuántas etapas hay, es un milagro que la mayor parte del tiempo esté funcionando.

Pero, ¿dónde en esa cadena tiene sentido guardar en caché? ¿Al principio? ¿Al final? ¿En todas partes? Depende... y depende de un gran número de factores. Incluso dos recursos del mismo sitio web podrían desear una respuesta diferente a esa pregunta.

Para darles una idea aproximada de los factores que pueden tener en cuenta,

**Tiempo de vida** : si los objetos tienen un corto tiempo de vida inherente (los datos de tráfico pueden tener un tiempo de vida más corto que los datos meteorológicos), puede que no valga la pena almacenarlos en caché.

**Costo de producción:** cuán costoso (en cuanto a ciclos de CPU y E/S) es la reproducción y el envío de un objeto. Si el almacenamiento en caché es barato puede no ser necesario.

**Tamaño** : los objetos grandes requieren más recursos para almacenarse en caché. Esto podría ser un factor limitante y debe equilibrarse con el beneficio.

**Frecuencia** de acceso: si raramente se accede a los objetos, el almacenamiento en caché podría no ser efectivo. Se quedarían obsoletos o se invalidarían antes de acceder a ellos por segunda vez desde la caché. Estos elementos simplemente bloquearían los recursos de memoria.

**Acceso** compartido: los datos que utiliza más de una entidad deben almacenarse en la caché más arriba de la cadena. En realidad, la cadena de almacenamiento en caché no es una cadena, sino un árbol. Más de un modelo puede utilizar un fragmento de datos del repositorio. A su vez, más de una secuencia de comandos de procesamiento puede utilizar estos modelos para generar fragmentos HTML. Estos fragmentos se incluyen en varias páginas que se distribuyen a varios usuarios con sus cachés privadas en el explorador. Así que &quot;compartir&quot; no significa compartir solo entre personas, sino entre piezas de software. Si desea encontrar una caché &quot;compartida&quot; potencial, simplemente rastree el árbol a la raíz y busque un antecesor común - es ahí donde debe almacenar en caché.

**Distribución** geoespacial: Si sus usuarios se distribuyen por todo el mundo, el uso de una red distribuida de cachés puede ayudar a reducir la latencia.

**Ancho de banda y latencia** de la red: Hablando de latencia, ¿quiénes son sus clientes y qué tipo de red utilizan? ¿Tal vez sus clientes son clientes móviles en un país subdesarrollado que utilizan una conexión 3G de teléfonos inteligentes de generaciones anteriores? Considere la posibilidad de crear objetos más pequeños y almacenarlos en caché en las memorias caché del explorador.

Esta lista por lejos no es exhaustiva, pero creemos que ya entienden la idea.

### Reglas básicas para el almacenamiento en caché encadenado

De nuevo: el almacenamiento en caché es difícil. Compartamos algunas reglas básicas, que hemos extraído de proyectos anteriores que pueden ayudarle a evitar problemas en su proyecto.

#### Evitar el almacenamiento en caché de Dobles

Cada una de las capas introducidas en el último capítulo proporciona algún valor en la cadena de almacenamiento en caché. Al guardar los ciclos de computación o al acercar los datos al consumidor. No está mal almacenar en caché un fragmento de datos en múltiples etapas de la cadena, pero siempre hay que tener en cuenta cuáles son los beneficios y los costos de la siguiente etapa. El almacenamiento en caché de una página completa en el sistema de publicación generalmente no proporciona ningún beneficio, ya que esto se hace en el despachante.

#### Mezcla de estrategias de invalidación

Existen tres estrategias básicas de invalidación:

* **TTL, Tiempo de vivir:** Un objeto caduca después de una cantidad de tiempo determinada (por ejemplo, &quot;dentro de 2 horas&quot;)
* **Fecha de caducidad:** El objeto caduca en el momento definido en el futuro (por ejemplo, &quot;5:00 PM el 10 de junio de 2019&quot;)
* **Basado en evento:** El objeto se invalida explícitamente por un evento que se produjo en la plataforma (por ejemplo, cuando se cambia y se activa una página)

Ahora puede usar diferentes estrategias en diferentes capas de caché, pero hay algunas &quot;tóxicas&quot;.

#### Invalidación basada en eventos

![Invalidez pura basada en el Evento](assets/chapter-3/event-based-invalidation.png)

*Invalidez pura basada en el Evento: Invalidar desde la caché interna a la capa externa*

<br> 

La invalidación pura basada en el evento es la más fácil de comprender, la más fácil de lograr teóricamente correcta y la más precisa.

En pocas palabras, las cachés se invalidan una por una después de que el objeto haya cambiado.

Solo tiene que tener en cuenta una regla:

Siempre invalide desde el interior a la caché externa. Si primero invalidó una caché externa, podría volver a almacenar en caché el contenido antiguo de una caché interna. No haga suposiciones sobre el momento en que una caché vuelve a estar fresca; asegúrese de que. Lo mejor es activar la invalidación de la caché externa _después_ de invalidar la interna.

Esa es la teoría. Pero en la práctica hay una serie de problemas. Los eventos deben distribuirse, potencialmente a través de una red. En la práctica, esto hace que sea el plan de invalidación más difícil de aplicar.

#### Automático - Curación

Con la invalidación basada en eventos, debe tener un plan de contingencia. ¿Qué sucede si se pierde un evento de invalidación? Una estrategia simple podría ser invalidar o purgar después de una determinada cantidad de tiempo. Por lo tanto, es posible que haya perdido ese evento y ahora ofrezca contenido antiguo. Pero los objetos también tienen un TTL implícito de varias horas (días) solamente. Finalmente el sistema se auto-cura.

#### Invalidez pura basada en TTL

![invalidación no sincronizada basada en TTL](assets/chapter-3/ttl-based-invalidation.png)

*invalidación no sincronizada basada en TTL*

<br> 

Ese también es un esquema bastante común. Se apilan varias capas de cachés, cada una con derecho a servir un objeto durante un determinado período de tiempo.

Es fácil de implementar. Desafortunadamente, es difícil predecir la duración efectiva de una porción de datos.

![Cubierta exterior que prolonga el recorrido de vida de un objeto interior](assets/chapter-3/outer-cache.png)

*Caché exterior que prolonga la duración de un objeto interior*

<br> 

Considere la ilustración anterior. Cada capa de almacenamiento en caché introduce una TTL de 2 minutos. Ahora - el TTL general también debe 2 minutos, ¿verdad? No del todo. Si la capa exterior captura el objeto justo antes de que se quede anticuado, la capa exterior prolonga el tiempo de vida efectivo del objeto. En ese caso, el tiempo de vida efectivo puede ser entre 2 y 4 minutos. Considere que usted está de acuerdo con su departamento de negocios, un día es tolerable - y usted tiene cuatro capas de cachés. El TTL real en cada capa no debe ser superior a seis horas... aumentando la velocidad de pérdida de caché...

No estamos diciendo que sea un mal plan. Deberías conocer sus límites. Y es una buena y fácil estrategia para el inicio. Sólo si el tráfico del sitio aumenta, puede considerar una estrategia más precisa.

*Sincronización de la hora de invalidación estableciendo una fecha específica*

#### Invalidación basada en fecha de caducidad

Se obtiene un tiempo de vida efectivo más predecible, si se establece una fecha específica en el objeto interno y se propaga a las cachés externas.

![Sincronización de fechas de caducidad](assets/chapter-3/synchronize-expiration-dates.png)

*Sincronización de fechas de caducidad*

<br> 

Sin embargo, no todas las cachés pueden propagar las fechas. Y puede resultar desagradable cuando la caché externa agrega dos objetos internos con fechas de caducidad diferentes.

#### Mezcla de invalidación basada en Evento y TTL

![Mezcla de estrategias basadas en evento y TTL](assets/chapter-3/mixing-event-ttl-strategies.png)

*Mezcla de estrategias basadas en evento y TTL*

<br> 

También un esquema común en el mundo AEM es utilizar la invalidación basada en el evento en las memorias caché interiores (por ejemplo, en las memorias caché en las que los eventos se pueden procesar en tiempo casi real) y las memorias caché basadas en TTL en el exterior -donde tal vez no tenga acceso a la invalidación explícita.

En el mundo AEM, tendría una memoria caché en memoria para objetos empresariales y fragmentos HTML en los sistemas de publicación, que se invalida cuando cambian los recursos subyacentes y se propaga este evento de cambio al despachante, que también funciona en evento. Delante de esto, tendría, por ejemplo, una CDN basada en TTL.

Tener una capa de almacenamiento en caché (corto) basado en TTL delante de un despachante podría suavizar un pico que normalmente se produciría después de una invalidación automática.

#### Mezcla de la invalidación basada en Evento y TTL

![Mezcla de la Invalidación basada en evento y TTL](assets/chapter-3/toxic.png)

*Tóxico: Mezcla de la Invalidación basada en evento y TTL*

<br> 

Esta combinación es tóxica. No coloque nunca la caché basada en evento después de una caché basada en TTL o Caducidad. ¿Recuerdan ese efecto derrame que tuvimos en la estrategia de &quot;TTL puro&quot;? Aquí se puede observar el mismo efecto. Sólo que el evento de invalidación de la caché externa ya se ha producido podría no volver a suceder - siempre, esto puede expandir la duración del objeto almacenado en caché hasta el infinito.

![Combinado basado en TTL y basado en evento: Desbordamiento hasta infinito](assets/chapter-3/infinity.png)

*Combinado basado en TTL y basado en evento: Desbordamiento hasta infinito*

<br> 

## Almacenamiento en caché parcial y almacenamiento en caché en memoria

Puede enlazar a la etapa del proceso de procesamiento para agregar capas de almacenamiento en caché. Desde obtener objetos de transferencia de datos remota o crear objetos comerciales locales hasta almacenar en caché el marcado procesado de un solo componente. Dejaremos implementaciones concretas en un tutorial posterior. Pero quizá planee ya haber implementado algunas de estas capas de almacenamiento en caché. Así que lo menos que podemos hacer aquí es introducir los principios básicos - y las gotchas.

### Palabras de advertencia

#### Respetar Control de acceso

Las técnicas descritas aquí son bastante poderosas y _obligatorias_ en cada una de las herramientas del desarrollador de AEM. Pero no te emociones demasiado, úsalos sabiamente. Almacenar un objeto en una caché y compartirlo con otros usuarios en solicitudes de seguimiento significa en realidad eludir el control de acceso. Normalmente, esto no es un problema en los sitios web públicos, pero puede serlo cuando un usuario necesita iniciar sesión antes de obtener acceso.

Considere almacenar el código HTML del menú principal del sitio en una caché en memoria para compartirlo entre varias páginas. En realidad, este es un ejemplo perfecto para almacenar HTML procesado parcialmente, ya que la creación de una navegación suele ser costosa, ya que requiere recorrer muchas páginas.

No comparte la misma estructura de menú entre todas las páginas, sino también con todos los usuarios, lo que la hace aún más eficaz. Pero espera... pero tal vez haya algunos elementos en el menú que están reservados solo para un grupo determinado de usuarios. En ese caso, el almacenamiento en caché puede resultar un poco más complejo.

#### Sólo Caché de Objetos Comerciales Personalizados

Si hay alguna - ese es el consejo más importante, podemos ofrecerle:

>[!WARNING]
>
>Sólo los objetos de caché que son suyos, que son inmutables, que usted mismo creó, que son superficiales y no tienen referencia saliente.

¿Qué significa eso?

1. No se sabe sobre el ciclo de vida previsto de los objetos de otras personas. Considere la posibilidad de consultar una referencia a un objeto de solicitud y decida almacenarla en caché. Ahora, la solicitud ha finalizado y el contenedor servlet desea reciclar ese objeto para la siguiente solicitud entrante. En ese caso, otra persona está cambiando el contenido sobre el que creía tener control exclusivo. No lo descartes - Hemos visto que algo así sucede en un proyecto. Los clientes estaban viendo los datos de otros clientes en lugar de los suyos.

2. Siempre que un objeto esté referenciado por una cadena de otras referencias, no se podrá eliminar del montón. Si se mantiene un objeto supuestamente pequeño en la memoria caché que hace referencia, digamos una representación de 4 MB de una imagen, se tendrá una buena oportunidad de tener problemas con la pérdida de memoria. Se supone que los cachés se basan en referencias débiles. Pero las referencias débiles no funcionan como cabría esperar. Esa es la mejor manera absoluta de producir una pérdida de memoria y terminar con un error de memoria insuficiente. Y - no sabes cuál es el tamaño de la memoria retenida de los objetos extranjeros, ¿verdad?

3. Especialmente en Sling, se puede adaptar (casi) cada objeto al otro. Considere colocar un recurso en la caché. La siguiente solicitud (con diferentes derechos de acceso), captura ese recurso y lo adapta a resourceResolver o a una sesión para acceder a otros recursos a los que no tendría acceso.

4. Aunque cree un &quot;envoltorio&quot; delgado alrededor de un recurso de AEM, no debe almacenarlo en caché, incluso si es el suyo y el inmutable. El objeto envuelto sería una referencia (que prohibimos antes) y si nos enfocamos, eso básicamente crea los mismos problemas que se describen en el último elemento.

5. Si desea almacenar en caché sus propios objetos copiando datos primitivos en sus propios objetos shallo. Es posible que desee vincular sus propios objetos mediante referencias; por ejemplo, puede que desee almacenar en caché un árbol de objetos. Esto está bien, pero sólo los objetos de caché que acaba de crear en la misma solicitud, y ningún objeto que se haya solicitado desde otro lugar (incluso si es el espacio de nombre del objeto &#39;suyo&#39;). _La clave es copiar objetos_ . Y asegúrese de purgar toda la estructura de objetos vinculados a la vez y evitar las referencias entrantes y salientes a la estructura.

6. Sí y mantener los objetos inmutables. Propiedades privadas, solo y sin definidores.

Son muchas reglas, pero vale la pena seguirlas. Incluso si usted tiene experiencia y es súper inteligente y tiene todo bajo control. El joven colega en su proyecto acaba de graduarse de la universidad. Él no sabe de todos estos escollos. Si no hay escollos, no hay nada que evitar. Manténgalo simple y comprensible.

### Herramientas y bibliotecas

Esta serie trata de comprender los conceptos y darle la posibilidad de construir una arquitectura que se ajuste mejor a su caso de uso.

No estamos promoviendo ninguna herramienta en particular. Pero dales pistas cómo evaluarlas. Por ejemplo, AEM tiene una caché integrada simple con un TTL fijo desde la versión 6.0. ¿Lo usas? Probablemente no en la publicación donde sigue una caché basada en evento en la cadena (sugerencia: El despachante). Pero podría ser por una opción decente para un Autor. También hay una caché HTTP por Adobe ACS commons que vale la pena considerar.

O puede crear su propio, basado en un marco de almacenamiento en caché maduro como [Ehcache](https://www.ehcache.org). Se puede utilizar para almacenar en caché objetos Java y marcas procesadas (`String` objetos).

En algunos casos sencillos, también puede llevarse bien con el uso de mapas hash concurrentes -aquí verá rápidamente límites- en la herramienta o en sus habilidades. La simultaneidad es tan difícil de dominar como la nominación y el almacenamiento en caché.

#### Referencias

* [Caché http de ACS Commons ](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Entorno de almacenamiento en caché Ehcache](https://www.ehcache.org)

### Términos básicos

No vamos a entrar en la teoría del almacenamiento en caché demasiado profundo aquí, pero nos sentimos obligados a proporcionar algunas palabras de moda, para que usted tenga un buen inicio de salto.

#### Desalojo de caché

Hablamos de invalidación y purgar mucho. _El desalojo_ de caché está relacionado con estos términos: Después de desalojar una entrada, ya no está disponible. Pero el desalojo no ocurre cuando una entrada está desactualizada, sino cuando la caché está llena. Los elementos nuevos o &quot;más importantes&quot; sacan de la caché a los más antiguos o a los menos importantes. Las entradas que tendrá que sacrificar es una decisión caso por caso. Es posible que quiera desalojar a los más antiguos o a los que se han utilizado muy raramente o a los que se ha accedido por última vez durante mucho tiempo.

#### Almacenamiento en caché preventivo

El almacenamiento en caché preventivo significa volver a crear la entrada con contenido nuevo en el momento en que se invalida o se considere obsoleta. Por supuesto, esto solo se puede hacer con unos pocos recursos, a los que se accede con frecuencia e inmediatamente. De lo contrario, perdería recursos al crear entradas de caché que nunca se soliciten. Al crear previamente entradas de caché, se podría reducir la latencia de la primera solicitud a un recurso después de la invalidación de caché.

#### Calentamiento de caché

El calentamiento de la caché está estrechamente relacionado con el almacenamiento en caché preventivo. Aunque no usarías ese término para un sistema activo. Y tiene menos tiempo que lo primero. No se vuelve a almacenar en caché inmediatamente después de la invalidación, pero se llena gradualmente la caché cuando el tiempo lo permite.

Por ejemplo, se extrae un tramo Publicar/Dispatcher del equilibrador de carga para actualizarlo. Antes de reintegrarla, rastrea automáticamente las páginas a las que se accede con mayor frecuencia para volver a colocarlas en la caché. Cuando la caché está &quot;caliente&quot;, rellena adecuadamente la pata de nuevo en el equilibrador de carga.

O tal vez reintegren la pierna a la vez, pero limiten el tráfico a esa pierna para que tenga la oportunidad de calentar sus cachés con uso regular.

O quizá también desee almacenar en caché algunas páginas a las que se accede con menos frecuencia en momentos en los que el sistema está inactivo para reducir la latencia cuando se accede a ellas en realidad mediante solicitudes reales.

#### Identidad del objeto de caché, carga útil, dependencia de invalidación y TTL

En general, un objeto en caché o una &quot;entrada&quot; tiene cinco propiedades principales,

#### Clave

Esta es la propiedad mediante la cual se identifica y se establece un objeto. Para recuperar la carga útil o para depurarla de la caché. El despachante, por ejemplo, utiliza la dirección URL de una página como clave. Tenga en cuenta que el despachante no utiliza las rutas de páginas. Esto no es suficiente para distinguir diferentes representaciones. Otras memorias caché pueden utilizar claves diferentes. Veremos algunos ejemplos más adelante.

#### Valor / Carga útil

Ese es el cofre del tesoro del objeto, los datos que desea recuperar. En el caso del despachante es el contenido de los archivos. Pero también puede ser un árbol de objetos Java.

#### TTL

Ya hemos cubierto el TTL. Tiempo después del cual una entrada se considera obsoleta y no debe entregarse más.

#### Dependencia

Esto se refiere a la invalidación basada en el evento. ¿En qué datos originales depende ese objeto? En la Parte I, ya dijimos, que un seguimiento de dependencia verdadero y preciso es demasiado complejo. Pero con nuestro conocimiento del sistema puedes aproximar las dependencias con un modelo más simple. Se invalidan suficientes objetos para purgar contenido antiguo... y tal vez inadvertidamente más de lo que se necesitaría. Pero aún así tratamos de mantenernos por debajo de &quot;purgar todo&quot;.

Qué objetos dependen de los que otros sean genuinos en cada aplicación individual. Más adelante le daremos algunos ejemplos de cómo implementar una estrategia de dependencia.

### Almacenamiento en caché de fragmentos HTML

![Reutilización de un fragmento procesado en páginas diferentes](assets/chapter-3/re-using-rendered-fragment.png)

*Reutilización de un fragmento procesado en páginas diferentes*

<br> 

El almacenamiento en caché de fragmentos HTML es una poderosa herramienta. La idea es almacenar en caché el código HTML generado por un componente en una memoria caché. Pueden preguntar, ¿por qué debería hacer eso? Estoy almacenando en caché el marcado de toda la página en el despachante de todos modos, incluido el marcado de ese componente. Estamos de acuerdo. Lo hace, pero una vez por página. No comparte ese marcado entre las páginas.

Imagine que está representando una navegación en la parte superior de cada página. El marcado tiene el mismo aspecto en cada página. Pero lo está representando una y otra vez para cada página, que no está en el despachante. Y recuerden: Después de la invalidación automática, es necesario volver a procesar todas las páginas. Básicamente, se está ejecutando el mismo código con los mismos resultados cientos de veces.

Por nuestra experiencia, la representación de una navegación superior anidada es una tarea muy costosa. Normalmente, se recorre una buena parte del árbol de documentos para generar los elementos de navegación. Aunque solo necesite el título de navegación y la dirección URL, las páginas deben cargarse en la memoria. Y aquí están obstruyendo recursos preciosos. Una y otra vez.

Pero el componente se comparte entre muchas páginas. Y compartir algo indica usar una caché. Por lo tanto, lo que desea hacer es comprobar si el componente de navegación ya se ha procesado y almacenado en caché y, en lugar de volver a procesar, emitir el valor de caché.

Hay dos maravillosas variedades de ese esquema que se pueden perder fácilmente:

1. Está almacenando una cadena Java en la caché. Una cadena no tiene referencias salientes y es inmutable. Así que, considerando las advertencias de arriba - esto es súper seguro.

2. La invalidación también es muy fácil. Cuando algo cambie su sitio web, quiere invalidar esta entrada de caché. La reconstrucción es relativamente barata, ya que sólo se debe realizar una vez y luego se reutiliza en todos los cientos de páginas.

Esto supone un gran alivio para los servidores de publicación.

### Implementación de cachés de fragmentos

#### Etiquetas personalizadas

En los viejos tiempos, en los que se utilizaba JSP como motor de creación de plantillas, era bastante común utilizar una etiqueta JSP personalizada que se ajustaba alrededor del código de representación de componentes.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

La etiqueta personalizada que capturaría su cuerpo y lo escribiría en la caché o evitaría la ejecución de su cuerpo y, en su lugar, generaría la carga útil de la entrada de caché.

La &quot;Clave&quot; es la ruta de componentes que tendría en la página principal. No utilizamos la ruta del componente en la página actual, ya que esto crearía una entrada de caché por página, lo que contradiría nuestra intención de compartir ese componente. Tampoco estamos utilizando sólo la ruta relativa (`jcr:conten/mainnavigation`) de los componentes, ya que esto nos impediría utilizar diferentes componentes de navegación en distintos sitios.

&quot;Caché&quot; es un indicador en el que se almacena la entrada. Normalmente, hay más de una caché en la que se almacenan los elementos. Cada una de ellas podría comportarse de forma un poco diferente. Así que es bueno diferenciar lo que se almacena - incluso si al final son sólo cadenas.

&quot;Dependencia&quot; de la que depende la entrada de caché. La caché de &quot;navegación principal&quot; puede tener una regla, que si hay algún cambio debajo del nodo &quot;dependencia&quot;, la entrada de acuerdo debe eliminarse. Por lo tanto, la implementación de la memoria caché tendría que registrarse como oyente de evento en el repositorio para estar al tanto de los cambios y luego aplicar las reglas específicas de la memoria caché para averiguar qué es lo que debe invalidarse.

Lo anterior fue sólo un ejemplo. También puede elegir tener un árbol de cachés. Donde el primer nivel se utiliza para separar sitios (o inquilinos) y el segundo nivel se ramifica en tipos de contenido (por ejemplo, &quot;navegación principal&quot;), lo que podría ahorrarle agregar la ruta de páginas de inicio como en el ejemplo anterior.

Por cierto, también puede utilizar este enfoque con componentes basados en HTL más modernos. Luego tendría un envoltorio JSP alrededor de su script HTL.

#### Filtros de componentes

Pero en un enfoque HTL puro, preferiría crear la caché de fragmentos con un filtro de componente Sling. Todavía no hemos visto esto en la naturaleza, pero ese es el enfoque que adoptaríamos sobre ese tema.

#### Sling Dynamic Include

La caché de fragmentos se utiliza si tiene alguna constante (navegación) en el contexto de un entorno que cambia (páginas diferentes).

Pero también puede tener lo contrario, un contexto relativamente constante (una página que rara vez cambia) y algunos fragmentos siempre cambiantes en esa página (por ejemplo, un ticker en vivo).

En este caso, puede dar una oportunidad a [Sling Dynamic Include](https://sling.apache.org/documentation/bundles/dynamic-includes.html) . En esencia, se trata de un filtro de componente que envuelve el componente dinámico y, en lugar de procesar el componente en la página, crea una referencia. Esta referencia puede ser una llamada de Ajax, de modo que el explorador incluya el componente y, por tanto, la página que lo rodea se pueda almacenar en caché de forma estática. O bien, Sling Dynamic Include puede generar una directiva SSI (Server Side Include). Esta directiva se ejecutaría en el servidor Apache. Incluso puede utilizar las directivas ESI - Edge Side Include si aprovecha Varnish o un CDN que admite scripts ESI.

![Diagrama de secuencia de una solicitud que utiliza Sling Dynamic Include](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagrama de secuencia de una solicitud que utiliza Sling Dynamic Include*

<br> 

La documentación de SDI indica que debe deshabilitar el almacenamiento en caché para las direcciones URL que finalizan en &quot;*.nocache.html&quot;, lo que tiene sentido, ya que se trata de componentes dinámicos.

Podría ver otra opción para utilizar SDI: Si _no desactiva_ la caché del despachante para los incluyentes, el despachante actúa como una caché de fragmentos similar a la que describimos en el último capítulo: Las páginas y los fragmentos de componentes de forma equitativa e independiente se almacenan en la caché del despachante y se unen mediante la secuencia de comandos SSI en el servidor Apache cuando se solicita la página. De este modo, puede implementar componentes compartidos como la navegación principal (siempre que utilice la misma URL de componente).

Eso debería funcionar -en teoría. Pero...

Recomendamos no hacer eso: Perdería la capacidad de evitar la caché para los componentes dinámicos reales. SDI se configura globalmente y los cambios que se harían para la &quot;memoria caché de fragmentos de hombre pobre&quot; también se aplicarían a los componentes dinámicos.

Le aconsejamos que estudie detenidamente la documentación de SDI. Existen otras limitaciones, pero la SDI es una herramienta valiosa en algunos casos.

#### Referencias

* [docs.oracle.com - Cómo escribir etiquetas JSP personalizadas](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Creación y uso de filtros de componentes](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - La dinámica de Sling incluye](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Configuración de las inclusiones dinámicas de Sling en AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Almacenamiento en caché de modelo

![Caché basada en modelo: Un objeto comercial con dos representaciones diferentes](assets/chapter-3/model-based-caching.png)

*Caché basada en modelo: Un objeto comercial con dos representaciones diferentes*

<br> 

Volvamos a examinar el caso con la navegación. Suponíamos que cada página requeriría la misma marca de navegación.

Pero tal vez, ese no es el caso. Es posible que desee representar un marcado diferente para el elemento en la navegación que representa la página __ actual.

```
Travel Destinations

<ul class="maninnav">
  <li class="currentPage">Travel Destinations
    <ul>
      <li>Finland
      <li>Canada
      <li>Norway
    </ul>
  <li>News
  <li>About us
<ul>
```

```
News

<ul class="maninnav">
  <li>Travel Destinations
  <li class="currentPage">News
    <ul>
      <li>Winter is coming>
      <li>Calm down in the wild
    </ul>
  <li>About us
<is
```

Estas son dos representaciones completamente diferentes. Aun así, el objeto __ comercial -el árbol de navegación completo- es el mismo.  Aquí, el objeto __ comercial sería un gráfico de objetos que representa los nodos del árbol. Este gráfico se puede almacenar fácilmente en una memoria caché. Recuerde, sin embargo, que este gráfico no debe contener ningún objeto ni hacer referencia a ningún objeto que no haya creado usted mismo, especialmente ahora nodos JCR.

#### Almacenamiento en caché en el explorador

Ya hemos tocado la importancia del almacenamiento en caché en el navegador, y hay muchos buenos tutoriales por ahí. Al final, para el navegador, Dispatcher es sólo un servidor web que sigue el protocolo HTTP.

Sin embargo, a pesar de la teoría, hemos reunido algunos conocimientos que no hemos encontrado en ningún otro lugar y que queremos compartir.

En esencia, el almacenamiento en caché de exploradores se puede aprovechar de dos maneras diferentes:

1. El explorador tiene un recurso almacenado en la caché del cual conoce la fecha de caducidad exacta. En ese caso, no solicita el recurso de nuevo.

2. El explorador tiene un recurso, pero no está seguro de si sigue siendo válido. En ese caso, preguntaría al servidor web (el despachante en nuestro caso). Por favor, denme el recurso si se modificó desde la última vez que lo entregó. Si no ha cambiado, el servidor responde con &quot;304 - no ha cambiado&quot; y sólo se han transmitido los metadatos.

#### Depuración

Si está optimizando la configuración de Dispatcher para el almacenamiento en caché del explorador, resulta extremadamente útil utilizar un servidor proxy de escritorio entre el explorador y el servidor web. Preferimos &quot;Charles Web Debugging Proxy&quot; de Karl von Randow.

Con Charles, puede leer las solicitudes y respuestas, que se transmiten desde y hacia el servidor. Y - puede aprender mucho sobre el protocolo HTTP. Los navegadores modernos también oferta algunas funciones de depuración, pero las características de un proxy de escritorio no tienen precedentes. Puede manipular los datos transferidos, acelerar la transmisión, reproducir solicitudes individuales y mucho más. Y la interfaz de usuario está claramente organizada y es bastante completa.

La prueba más básica es usar el sitio web como un usuario normal -con el proxy en el medio- y comprobar el proxy si el número de solicitudes estáticas (a /etc/...) se está reduciendo con el tiempo, ya que deberían estar en la memoria caché y ya no ser solicitadas.

Encontramos que un proxy podría proporcionar una descripción general más clara, ya que la solicitud en caché no aparece en el registro, mientras que algunos depuradores integrados en el explorador siguen mostrando estas solicitudes con &quot;0 ms&quot; o &quot;desde el disco&quot;. Lo cual es correcto y preciso pero podría nublar tu vista un poco.

A continuación, puede explorar en profundidad y comprobar los encabezados de los archivos transferidos para ver, por ejemplo, si los encabezados http &quot;Caduca&quot; son correctos. Puede reproducir solicitudes con encabezados if-modified-as configurados para ver si el servidor responde correctamente con un código de respuesta 304 o 200. Puede observar el tiempo de las llamadas asincrónicas y también puede probar los supuestos de seguridad en cierto grado. ¿Recuerda que le dijimos que no aceptara todos los selectores que no se esperan explícitamente? Aquí puede jugar con la URL y los parámetros y ver si la aplicación se comporta bien.

Solo hay una cosa que le pedimos que no haga, cuando esté depurando su caché:

No vuelva a cargar las páginas en el explorador.

Una &quot;recarga del explorador&quot;, una recarga __ simple y una recarga __ forzada (&quot;_cambio-recarga_&quot;) no son lo mismo que una solicitud de página normal. Una simple solicitud de recarga establece un encabezado

```
Cache-Control: max-age=0
```

Y una tecla Mayús-Recarga (manteniendo presionada la tecla &quot;Mayús&quot; mientras hace clic en el botón de recarga) generalmente configura un encabezado de solicitud

```
Cache-Control: no-cache
```

Ambos encabezados tienen efectos similares pero ligeramente diferentes, pero lo que es más importante, difieren completamente de una solicitud normal cuando se abre una dirección URL desde la ranura de dirección URL o mediante vínculos en el sitio. La exploración normal no establece los encabezados Cache-Control, pero probablemente un encabezado if-modified-as.

Por lo tanto, si desea depurar el comportamiento de exploración normal, debe hacer exactamente lo siguiente: _Explore con normalidad_. El uso del botón de recarga del navegador es la mejor manera de no ver los errores de configuración de caché en la configuración.

Usa tu proxy Charles para ver de qué estamos hablando. Sí - y mientras lo tiene abierto - puede reproducir las solicitudes justo ahí. No es necesario volver a cargar desde el explorador.

## Prueba de rendimiento

Al utilizar un proxy, se obtiene una idea del comportamiento de temporización de las páginas. Por supuesto, no se trata de una prueba de rendimiento.  Una prueba de rendimiento requeriría que varios clientes solicitaran sus páginas en paralelo.

Un error común, que hemos visto con demasiada frecuencia, es que la prueba de rendimiento solo incluye un número superpequeño de páginas y que estas páginas se envían únicamente desde la caché de Dispatcher.

Si está promocionando su aplicación al sistema activo, la carga es completamente diferente de la que ha probado.

En el sistema activo, el patrón de acceso no es tan pequeño número de páginas distribuidas equitativamente que tiene en las pruebas (página de inicio y pocas páginas de contenido). El número de páginas es mucho mayor y las solicitudes se distribuyen de manera muy desigual. Y - por supuesto - las páginas en vivo no se pueden servir al 100% desde la caché: Hay solicitudes de invalidación procedentes del sistema Publicar que invalidan automáticamente una gran parte de sus preciados recursos.

Ah sí, y cuando esté reconstruyendo la caché de Dispatcher, descubrirá que el sistema de publicación también se comporta de manera muy diferente, dependiendo de si solicita sólo un puñado de páginas o un número mayor. Aunque todas las páginas sean igualmente complejas, su número juega un papel. ¿Recuerdan lo que dijimos sobre el almacenamiento en caché encadenado? Si siempre solicita el mismo número pequeño de páginas, es muy probable que los bloques de acuerdo con los datos sin procesar se encuentren en la caché de los discos duros o que el sistema operativo almacene en caché los bloques. Además, existe una buena posibilidad de que el Repositorio haya almacenado en caché el segmento de acuerdo en su memoria principal. Por lo tanto, la representación es considerablemente más rápida que cuando otras páginas se desalojaban una de otra ahora y después de varias cachés.

El almacenamiento en caché es difícil, al igual que la prueba de un sistema que depende del almacenamiento en caché. Entonces, ¿qué se puede hacer para tener un escenario de la vida real más preciso?

Creemos que tendrá que realizar más de una prueba y que debe proporcionar más de un índice de rendimiento como medida de la calidad de su solución.

Si ya tiene un sitio web existente, mida el número de solicitudes y la forma en que se distribuyen. Intente modelar una prueba que utilice una distribución de solicitudes similar. Añadir un poco de aleatoriedad no podía hacer daño. No es necesario simular un navegador que cargue recursos estáticos como JS y CSS, ya que no importan. Se almacenan en caché en el navegador o en el despachante eventualmente y no suman significativamente a la carga. Pero las imágenes a las que se hace referencia sí importan. Encuentre también su distribución en archivos de registro antiguos y modele un patrón de solicitud similar.

Ahora lleve a cabo una prueba con Dispatcher sin almacenar en caché. Ese es su peor escenario. Averiguar en qué carga máxima su sistema se está volviendo inestable bajo estas peores condiciones. También puede empeorarlo si elimina algunas piernas de Dispatcher/Publish si lo desea.

A continuación, realice la misma prueba con todos los ajustes de caché necesarios para &quot;activar&quot;. Aumente lentamente sus solicitudes paralelas para calentar la caché y ver cuánto puede tomar su sistema en estas mejores condiciones.

Un escenario de caso promedio sería ejecutar la prueba con Dispatcher habilitado pero también con algunas invalidaciones. Puede simular esto tocando los archivos de estado por un cronjob o enviando las solicitudes de invalidación en intervalos irregulares al despachante. No olvide también purgar algunos de los recursos no invalidados automáticamente de vez en cuando.

Puede variar el último escenario aumentando las solicitudes de invalidación y la carga.

Esto es un poco más complejo que una simple prueba de carga lineal, pero le da mucha más confianza en su solución.

Puedes evitar el esfuerzo. Sin embargo, realice una prueba en el peor de los casos en el sistema de publicación con un mayor número de páginas (distribuidas de manera equitativa) para ver los límites del sistema. Asegúrese de interpretar correctamente el número de casos óptimos y de proporcionar a sus sistemas suficiente espacio para la cabeza.