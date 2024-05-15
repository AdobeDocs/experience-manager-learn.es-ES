---
title: 'Capítulo 3: Temas avanzados del almacenamiento en caché de Dispatcher'
description: AEM Esta es la parte 3 de una serie de tres partes para almacenar en caché en el almacenamiento en caché de la. Dónde las dos primeras partes se centraron en el almacenamiento en caché de http sin formato en Dispatcher y qué limitaciones hay. En esta parte se analizan algunas ideas sobre cómo superar estas limitaciones.
feature: Dispatcher
topic: Architecture
role: Architect
level: Intermediate
doc-type: Tutorial
exl-id: 7c7df08d-02a7-4548-96c0-98e27bcbc49b
duration: 1353
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '6172'
ht-degree: 0%

---

# Capítulo 3: Temas avanzados de almacenamiento en caché

*&quot;Solo hay dos cosas difíciles en la informática: invalidar la caché y nombrar cosas&quot;.*

— PHIL KARLTON

## Información general

AEM Esta es la parte 3 de una serie de tres partes para almacenar en caché en el almacenamiento en caché de la base de datos de. Dónde las dos primeras partes se centraron en el almacenamiento en caché de http sin formato en Dispatcher y qué limitaciones hay. En esta parte se analizan algunas ideas sobre cómo superar estas limitaciones.

## Almacenamiento en caché en general

[Capítulo 1](chapter-1.md) y [Capítulo 2](chapter-2.md) La mayoría de esta serie se centró principalmente en Dispatcher. Hemos explicado los conceptos básicos, las limitaciones y dónde debe realizar determinadas compensaciones.

La complejidad y las complejidades del almacenamiento en caché no son problemas únicos de Dispatcher. El almacenamiento en caché es difícil en general.

Tener Dispatcher como la única herramienta en su caja de herramientas sería en realidad una limitación.

En este capítulo queremos ampliar aún más nuestra vista sobre el almacenamiento en caché y desarrollar algunas ideas para superar algunas de las deficiencias de Dispatcher. No hay una solución mágica: tendrá que hacer concesiones en su proyecto. Recuerde que con el almacenamiento en caché y la precisión de invalidación siempre viene la complejidad, y con la complejidad viene la posibilidad de errores.

Tendrá que hacer concesiones en estas áreas,

* Rendimiento y latencia
* Consumo de recursos / Carga de CPU / Uso de disco
* Precisión / Moneda / Robustez / Seguridad
* Simplicidad/complejidad/coste/mantenimiento/propensión a errores

Estas dimensiones están vinculadas entre sí en un sistema bastante complejo. No hay un simple si-este-entonces-eso. Simplificar un sistema puede hacerlo más rápido o más lento. Puede reducir los costes de desarrollo, pero aumentar los costes en el servicio de asistencia, por ejemplo, si los clientes ven contenido antiguo o se quejan de un sitio web lento. Todos estos factores deben ser considerados y equilibrados entre sí. Pero a estas alturas ya deberían tener una buena idea, que no hay una bala de plata o una sola &quot;práctica recomendada&quot; - sólo un montón de malas prácticas y unas pocas buenas.

## Almacenamiento en caché encadenado

### Información general

#### Flujo de datos

Al enviar una página desde un servidor al explorador de un cliente, se atraviesan una multitud de sistemas y subsistemas. Si observa con atención, hay una serie de saltos que los datos deben llevar del origen al drenaje, cada uno de los cuales es un candidato potencial para el almacenamiento en caché.

![Flujo de datos de una aplicación CMS típica](assets/chapter-3/data-flow-typical-cms-app.png)

*Flujo de datos de una aplicación CMS típica*

<br> 

Comencemos nuestro recorrido con un fragmento de datos que se encuentra en un disco duro y que debe mostrarse en un explorador.

#### Hardware y sistema operativo

En primer lugar, la propia unidad de disco duro (HDD) tiene una caché integrada en el hardware. Segundo, el sistema operativo que monta el disco duro, utiliza memoria libre para almacenar en caché los bloques a los que se accede con frecuencia para acelerar el acceso.

#### Repositorio de contenido

AEM El siguiente nivel es CRX u Oak: la base de datos de documentos utilizada por los usuarios de la base de datos de documentos de la base de datos de. CRX y Oak dividen los datos en segmentos que se pueden almacenar en caché también para evitar un acceso más lento al disco duro.

#### Datos de terceros

La mayoría de las instalaciones web más grandes también tienen datos de terceros; datos procedentes de un sistema de información de productos, un sistema de gestión de relaciones con el cliente, una base de datos heredada o cualquier otro servicio web arbitrario. No es necesario extraer estos datos de la fuente cada vez que se necesitan, especialmente cuando se sabe que cambian con poca frecuencia. Por lo tanto, se puede almacenar en caché si no se sincroniza en la base de datos CRX.

#### Nivel empresarial: aplicación/modelo

Normalmente, los scripts de plantilla no representan el contenido sin procesar proveniente de CRX a través de la API JCR. Lo más probable es que tenga una capa empresarial intermedia que combine, calcule o transforme datos en un objeto de dominio empresarial. Adivina qué: si estas operaciones son costosas, debe considerar almacenarlas en caché.

#### Fragmentos de marcado

El modelo ahora es la base para la renderización del marcado de un componente. ¿Por qué no almacenar también en caché el modelo procesado?

#### Dispatcher, CDN y otros proxies

Desactivado va la página HTML procesada a Dispatcher. Ya hemos hablado de que el propósito principal de Dispatcher es almacenar en caché las páginas de los HTML y otros recursos web (a pesar de su nombre). Antes de que los recursos lleguen al explorador, puede pasar un proxy inverso, que puede almacenar en caché y una CDN, que también se utiliza para el almacenamiento en caché. El cliente puede sentarse en una oficina que concede acceso a la web solo a través de un proxy, y ese proxy puede decidir almacenar en caché también para ahorrar tráfico.

#### Caché del explorador

Por último, pero no menos importante, el explorador también almacena en caché. Este es un recurso fácilmente ignorado. Sin embargo, es la caché más cercana y rápida que tiene en la cadena de almacenamiento en caché. Lamentablemente, no se comparte entre usuarios, pero entre distintas solicitudes de un usuario.

### Dónde almacenar en caché y por qué

Esa es una larga cadena de cachés potenciales. Y todos hemos enfrentado problemas donde hemos visto contenido obsoleto. Pero teniendo en cuenta cuántas etapas hay, es un milagro que la mayor parte del tiempo esté funcionando.

Pero, ¿en qué parte de esa cadena tiene sentido almacenar en caché? ¿Al principio? ¿Al final? ¿En todas partes? Depende... y depende de un gran número de factores. Incluso dos recursos en el mismo sitio web podrían desear una respuesta diferente a esa pregunta.

Para darle una idea aproximada de qué factores podría tener en cuenta,

**Tiempo de vida** : Si los objetos tienen un corto tiempo de vida inherente (los datos de tráfico pueden tener un tiempo de vida más corto que los datos meteorológicos), es posible que no valga la pena almacenarlos en caché.

**Coste de producción -** Cuánto cuesta (en términos de ciclos de CPU y E/S) la reproducción y entrega de un objeto. Si es barato, el almacenamiento en caché podría no ser necesario.

**Tamaño** : Los objetos grandes requieren más recursos para almacenarse en caché. Ese podría ser un factor limitante y debe sopesarse en relación con los beneficios.

**Frecuencia de acceso** : Si no se accede a los objetos con frecuencia, el almacenamiento en caché podría no ser eficaz. Simplemente quedarían obsoletos o se invalidarían antes de que se acceda a ellos por segunda vez desde la caché. Estos elementos simplemente bloquearían los recursos de memoria.

**Acceso compartido** : los datos que utilizan más de una entidad deben almacenarse en caché más adelante en la cadena. En realidad, la cadena de almacenamiento en caché no es una cadena, sino un árbol. Más de un modelo podría utilizar un fragmento de datos del repositorio. Estos modelos, a su vez, los puede utilizar más de un script de procesamiento para generar fragmentos de HTML. Estos fragmentos se incluyen en varias páginas que se distribuyen a varios usuarios con sus cachés privadas en el explorador. Así que &quot;compartir&quot; no significa compartir entre personas solamente, sino entre piezas de software. Si desea encontrar una caché &quot;compartida&quot; potencial, solo tiene que rastrear el árbol hasta la raíz y encontrar un antecesor común, ahí es donde debe almacenar en caché.

**Distribución geoespacial** : Si los usuarios están distribuidos por todo el mundo, el uso de una red distribuida de cachés puede ayudar a reducir la latencia.

**Ancho de banda y latencia de red** - Hablando de latencia, ¿quiénes son sus clientes y qué tipo de red utilizan? ¿Tal vez sus clientes son clientes móviles en un país subdesarrollado que utilizan una conexión 3G de teléfonos inteligentes de generación más antigua? Considere la posibilidad de crear objetos más pequeños y almacenarlos en caché en las cachés del explorador.

Esta lista no es muy completa, pero creemos que ya entendieron la idea.

### Reglas básicas para el almacenamiento en caché encadenado

De nuevo: el almacenamiento en caché es difícil. Compartimos algunas reglas básicas que hemos extraído de proyectos anteriores y que pueden ayudarle a evitar problemas en su proyecto.

#### Evitar el almacenamiento en caché doble

Cada una de las capas introducidas en el último capítulo proporciona algún valor en la cadena de almacenamiento en caché. Ahorrando ciclos informáticos o acercando los datos al consumidor. No es incorrecto almacenar en caché un fragmento de datos en varias etapas de la cadena, pero siempre debe tener en cuenta cuáles son los beneficios y los costes de la siguiente etapa. Almacenar en caché una página completa en el sistema de publicación no suele proporcionar ningún beneficio, ya que esto ya se hace en Dispatcher.

#### Combinación de estrategias de invalidación

Existen tres estrategias básicas de invalidación:

* **TTL, Tiempo de vida:** Un objeto caduca después de una cantidad de tiempo fija (por ejemplo, &quot;dentro de 2 horas&quot;)
* **Fecha de caducidad:** El objeto caduca a una hora definida en el futuro (por ejemplo, &quot;5:00 p.m. el 10 de junio de 2019&quot;)
* **Basado en evento:** El objeto se invalida explícitamente mediante un evento que se produjo en la plataforma (por ejemplo, cuando se cambia y activa una página)

Ahora puede utilizar diferentes estrategias en diferentes capas de caché, pero hay algunas &quot;tóxicas&quot;.

#### Invalidación basada en evento

![Invalidación basada en evento puro](assets/chapter-3/event-based-invalidation.png)

*Invalidación basada en evento puro: invalidar desde la caché interna a la capa exterior*

<br> 

La invalidación pura basada en eventos es la más fácil de comprender, la más fácil de hacer correctamente en teoría y la más precisa.

En pocas palabras, las cachés se invalidan una por una después de que el objeto haya cambiado.

Solo tiene que tener en cuenta una regla:

Invalidar siempre desde la caché interna a la caché externa. Si invalidó primero una caché externa, podría volver a almacenar en caché el contenido obsoleto de una caché interna. No realice ninguna suposición en el momento en que una caché vuelva a estar fresca: asegúrese de que lo está. Lo mejor es activar la invalidación de la caché externa _después_ invalidando la interna.

Ahora, esa es la teoría. Pero en la práctica hay una serie de problemas. Los eventos deben distribuirse, posiblemente a través de una red. En la práctica, esto hace que sea el esquema de invalidación más difícil de implementar.

#### Automático: Curación

Con la invalidación basada en eventos, debe tener un plan de contingencia. ¿Qué sucede si se pasa por alto un evento de invalidación? Una estrategia sencilla podría ser invalidar o purgar después de una cierta cantidad de tiempo. Por lo tanto, es posible que se haya perdido ese evento y ahora ofrezca contenido obsoleto. Sin embargo, los objetos también tienen un TTL implícito de varias horas (días) únicamente. Así que eventualmente el sistema se cura a sí mismo.

#### Invalidación pura basada en TTL

![Invalidación basada en TTL no sincronizada](assets/chapter-3/ttl-based-invalidation.png)

*Invalidación basada en TTL no sincronizada*

<br> 

Ese también es un esquema bastante común. Se apilan varias capas de cachés, cada una con derecho a servir un objeto durante una cierta cantidad de tiempo.

Es fácil de implementar. Desafortunadamente, es difícil predecir la vida útil efectiva de un fragmento de datos.

![Casualidad externa que prolonga la duración de un objeto interior](assets/chapter-3/outer-cache.png)

*Caché externa que prolonga la vida útil de un objeto interno*

<br> 

Consideremos la ilustración anterior. Cada capa de almacenamiento en caché introduce un TTL de 2 minutos. Ahora - el TTL general debe 2 min también, ¿verdad? No del todo. Si la capa exterior recupera el objeto justo antes de que quede obsoleto, la capa exterior en realidad prolonga el tiempo de vida efectivo del objeto. El tiempo de vida efectivo puede estar entre 2 y 4 minutos en ese caso. Considere que estuvo de acuerdo con su departamento comercial, un día es tolerable y tiene cuatro capas de cachés. El TTL real en cada capa no debe ser superior a seis horas... aumentando la tasa de fallos de caché...

No estamos diciendo que sea un mal plan. Deberías saber sus límites. Y es una buena y fácil estrategia para empezar. Solo si el tráfico del sitio aumenta, puede considerar una estrategia más precisa.

*Sincronizar la hora de invalidación estableciendo una fecha específica*

#### Invalidación basada en fecha de vencimiento

Se obtiene una vida útil efectiva más predecible, si se configura una fecha específica en el objeto interno y se propaga a las cachés externas.

![Sincronización de fechas de caducidad](assets/chapter-3/synchronize-expiration-dates.png)

*Sincronización de fechas de caducidad*

<br> 

Sin embargo, no todas las cachés pueden propagar las fechas. Y puede volverse desagradable, cuando la caché externa agrega dos objetos internos con diferentes fechas de caducidad.

#### Combinación de invalidación basada en eventos y en TTL

![Combinación de estrategias basadas en eventos y en TTL](assets/chapter-3/mixing-event-ttl-strategies.png)

*Combinación de estrategias basadas en eventos y en TTL*

<br> 

AEM También un esquema común en el mundo de la es utilizar la invalidación basada en eventos en las cachés internas (por ejemplo, cachés en memoria donde los eventos se pueden procesar en tiempo casi real) y cachés basadas en TTL en el exterior, donde tal vez no tenga acceso a la invalidación explícita.

AEM En el mundo de los recursos, tendría una caché en memoria para objetos empresariales y fragmentos de HTML en los sistemas de publicación, que se invalida cuando cambian los recursos subyacentes y propaga este evento de cambio al distribuidor, que también funciona en función de eventos. Por delante tendría, por ejemplo, una CDN basada en TTL.

Tener una capa de almacenamiento en caché (corto) basado en TTL delante de Dispatcher podría suavizar eficazmente un pico que generalmente se produciría después de una invalidación automática.

#### Combinación de TTL e invalidación basada en eventos

![Combinación de TTL e invalidación basada en eventos](assets/chapter-3/toxic.png)

*Tóxico: mezcla de TTL y invalidación basada en eventos*

<br> 

Esta combinación es tóxica. Nunca coloque una caché basada en eventos después de un TTL o una caché basada en caducidad. ¿Recuerdan el efecto derrame que tuvimos en la estrategia de &quot;TTL puro&quot;? El mismo efecto se puede observar aquí. Solo que el evento de invalidación de la caché externa ya se ha producido puede que no vuelva a ocurrir, nunca más, esto puede expandir la vida útil del objeto en caché hasta el infinito.

![Combinación basada en TTL y basada en eventos: desbordamiento hacia el infinito](assets/chapter-3/infinity.png)

*Combinación basada en TTL y basada en eventos: desbordamiento hacia el infinito*

<br> 

## Almacenamiento en caché parcial y almacenamiento en caché en memoria

Puede conectarse al escenario del proceso de renderización para añadir capas de almacenamiento en caché. Desde obtener objetos de transferencia de datos remotos o crear objetos empresariales locales hasta almacenar en caché el marcado procesado de un solo componente. Dejaremos las implementaciones concretas en un tutorial posterior. Pero tal vez ya tiene pensado implementar algunas de estas capas de almacenamiento en caché. Así que lo menos que podemos hacer aquí es introducir los principios básicos... y los problemas.

### Palabras de advertencia

#### Respetar control de acceso

Las técnicas descritas aquí son bastante potentes y una _imprescindible_ AEM en la caja de herramientas de cada desarrollador de. Pero no te emociones demasiado, úsalos sabiamente. Almacenar un objeto en una caché y compartirlo con otros usuarios en solicitudes de seguimiento significa eludir el control de acceso. Esto no suele ser un problema en los sitios web públicos, pero puede serlo cuando un usuario necesita iniciar sesión antes de obtener acceso.

Considere la posibilidad de almacenar el marcado de HTML de un menú principal de sitios en una memoria caché en memoria para compartirlo entre varias páginas. En realidad, este es un ejemplo perfecto para almacenar HTML procesados parcialmente, ya que la creación de una navegación suele ser costosa, ya que requiere atravesar muchas páginas.

No comparte la misma estructura de menús entre todas las páginas, sino también con todos los usuarios, lo que la hace aún más eficaz. Pero espere... pero tal vez haya algunos elementos en el menú que están reservados únicamente para un determinado grupo de usuarios. En ese caso, el almacenamiento en caché puede ser un poco más complejo.

#### Almacenar sólo en caché objetos de negocio personalizados

Si alguno - ese es el consejo más importante, podemos darle:

>[!WARNING]
>
>Almacene en caché únicamente los objetos que sean suyos, que sean inmutables, que haya creado usted mismo, que sean superficiales y no tengan referencias salientes.

¿Qué significa eso?

1. No se conoce el ciclo de vida previsto de los objetos de otras personas. Considere la posibilidad de obtener una referencia a un objeto de solicitud y decidir almacenarlo en caché. Ahora, la solicitud ha finalizado y el contenedor del servlet desea reciclar ese objeto para la siguiente solicitud entrante. En ese caso, otra persona está cambiando el contenido sobre el que usted pensaba que tenía control exclusivo. No descartes eso: hemos visto que algo así sucede en un proyecto. Los clientes veían datos de otros clientes en lugar de los suyos propios.

2. Mientras una cadena de otras referencias haga referencia a un objeto, este no se puede eliminar del montón. Si retiene un objeto supuestamente pequeño en la caché que hace referencia a, digamos una representación de 4 MB de una imagen, tendrá una buena oportunidad de tener problemas con la pérdida de memoria. Se supone que las cachés deben basarse en referencias débiles. Sin embargo, las referencias débiles no funcionan como cabría esperar. Esa es la mejor manera de producir una fuga de memoria y terminar en un error de falta de memoria. Y... no sabes cuál es el tamaño de la memoria retenida de los objetos extraños, ¿verdad?

3. Especialmente en Sling, puede adaptar (casi) cada objeto entre sí. Considere la posibilidad de colocar un recurso en la caché. La siguiente solicitud (con diferentes derechos de acceso), recupera ese recurso y lo adapta a un resourceResolver o a una sesión para acceder a otros recursos a los que no tendría acceso.

4. AEM Incluso si crea un &quot;envoltorio&quot; delgado alrededor de un recurso a partir de un recurso, no debe almacenarlo en caché, aunque sea su propio recurso e inmutable. El objeto ajustado sería una referencia (que antes prohibimos) y si nos vemos nítidos, eso básicamente crea los mismos problemas que se describen en el último elemento.

5. Si desea almacenar en caché, cree sus propios objetos copiando datos primitivos en sus propios objetos shallo. Es posible que desee vincular entre sus propios objetos mediante referencias; por ejemplo, puede que desee almacenar en caché un árbol de objetos. Está bien, pero solo almacena en caché los objetos que acaba de crear en la misma solicitud, y no los objetos que se solicitaron desde otro lugar (aunque sea el espacio de nombre del objeto). _Copia de objetos_ es la clave. Y asegúrese de purgar toda la estructura de objetos vinculados a la vez y evitar referencias entrantes y salientes a su estructura.

6. Sí, y mantenga sus objetos inmutables. Propiedades privadas, solo y sin establecedores.

Eso es un montón de reglas, pero vale la pena seguirlas. Incluso si tienes experiencia y eres súper inteligente y tienes todo bajo control. El joven colega de su proyecto acaba de graduarse de la universidad. Él no sabe de todas estas trampas. Si no hay escollos, no hay nada que evitar. Hágalo simple y comprensible.

### Herramientas y bibliotecas

Esta serie trata sobre comprender los conceptos y permitirle crear una arquitectura que se ajuste mejor a su caso de uso.

No estamos promoviendo ninguna herramienta en particular. Pero te daré pistas sobre cómo evaluarlas. AEM Por ejemplo, la caché integrada simple de la tiene un TTL fijo desde la versión 6.0. ¿Quieres usarlo? Probablemente no en la publicación, donde sigue una caché basada en eventos en la cadena (sugerencia: Dispatcher). Pero podría ser una elección decente para un autor. También hay una caché HTTP por Adobe ACS commons que podría valer la pena considerar.

O puede crear el suyo propio, basado en un marco de trabajo de almacenamiento en caché maduro como [Ehcache](https://www.ehcache.org). Se puede utilizar para almacenar en caché objetos Java y marcado procesado (`String` objetos).

En algunos casos sencillos, también puede llevarse bien con el uso de mapas hash simultáneos (verá rápidamente límites aquí), ya sea en la herramienta o en sus habilidades. La concurrencia es tan difícil de dominar como la nomenclatura y el almacenamiento en caché.

#### Referencias

* [Caché http de ACS Commons](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Marco de almacenamiento en caché Ehcache](https://www.ehcache.org)

### Términos básicos

No entraremos en la teoría del almacenamiento en caché demasiado profundo aquí, pero nos sentimos obligados a proporcionar algunas palabras de moda, para que tenga un buen comienzo.

#### Desalojo de caché

Hablamos mucho de invalidación y purga. _Desalojo de caché_ está relacionado con estos términos: Después de una entrada, se desaloja y ya no está disponible. Pero la expulsión no se produce cuando una entrada está obsoleta, sino cuando la caché está llena. Los elementos más recientes o &quot;más importantes&quot; eliminan de la caché los más antiguos o menos importantes. Las entradas que tendrá que sacrificar es una decisión caso por caso. Es posible que desee expulsar a los más antiguos o a los que se han utilizado muy pocas veces o a los que se ha accedido por última vez durante mucho tiempo.

#### Almacenamiento en caché preventivo

El almacenamiento en caché preventivo significa volver a crear la entrada con contenido nuevo en el momento en que se invalida o se considera obsoleta. Por supuesto, solo lo haría con unos pocos recursos, a los que seguro se accede con frecuencia e inmediatamente. De lo contrario, desperdiciaría recursos al crear entradas de caché que podrían no solicitarse nunca. Si crea entradas de caché de forma preventiva, puede reducir la latencia de la primera solicitud a un recurso después de la invalidación de la caché.

#### Calentamiento de caché

El calentamiento de la caché está estrechamente relacionado con el almacenamiento en caché preventivo. Aunque no usarías ese término para un sistema en vivo. Y es menos tiempo limitado que el primero. No se vuelve a almacenar en caché inmediatamente después de la invalidación, sino que se rellena gradualmente la caché cuando el tiempo lo permite.

Por ejemplo, se extrae un componente Publicación/Dispatcher del equilibrador de carga para actualizarlo. Antes de volver a integrarlo, rastrea automáticamente las páginas a las que se accede con más frecuencia para volver a introducirlas en la caché. Cuando la caché está &quot;caliente&quot;: se llena adecuadamente, se reintegra la pata en el equilibrador de carga.

O tal vez reintegres la pierna de una vez, pero estrangulas el tráfico a esa pierna para que tenga la oportunidad de calentar sus cachés por el uso regular.

O quizá también quiera almacenar en caché algunas páginas a las que se accede con menos frecuencia en momentos en que el sistema está inactivo para reducir la latencia cuando realmente se accede a ellas mediante solicitudes reales.

#### Identidad de objeto de caché, carga útil, dependencia de invalidación y TTL

En términos generales, un objeto en caché o &quot;entrada&quot; tiene cinco propiedades principales,

#### Clave

Identidad es la propiedad mediante la cual se identifican y se oponen. Para recuperar su carga útil o depurarla de la caché. Por ejemplo, Dispatcher utiliza la dirección URL de una página como clave. Tenga en cuenta que Dispatcher no utiliza las rutas de páginas. Esto no es suficiente para diferenciar las diferentes renderizaciones. Otras cachés pueden utilizar claves diferentes. Más adelante veremos algunos ejemplos.

#### Valor/carga útil

Ese es el cofre del tesoro del objeto, los datos que desea recuperar. En el caso del despachante, es el contenido de los archivos. Pero también puede ser un árbol de objetos de Java.

#### TTL

Ya cubrimos el TTL. El tiempo después del cual una entrada se considera obsoleta y no debe entregarse más.

#### Dependencia

Esto se relaciona con la invalidación basada en eventos. ¿De qué datos originales depende ese objeto? En la Parte I, ya dijimos, que un seguimiento de dependencia verdadero y preciso es demasiado complejo. Pero con nuestro conocimiento del sistema se pueden aproximar las dependencias con un modelo más sencillo. Invalidamos suficientes objetos para purgar contenido obsoleto... y quizás inadvertidamente más de lo que se requeriría. Sin embargo, tratamos de mantener por debajo de &quot;purgar todo&quot;.

Qué objetos dependen de qué otros sean genuinos en cada aplicación. Más adelante le daremos algunos ejemplos sobre cómo implementar una estrategia de dependencia.

### Almacenamiento en caché de fragmentos de HTML

![Reutilización de un fragmento procesado en diferentes páginas](assets/chapter-3/re-using-rendered-fragment.png)

*Reutilización de un fragmento procesado en diferentes páginas*

<br> 

HTML El almacenamiento en caché de fragmentos es una herramienta poderosa. La idea es almacenar en caché el marcado del HTML generado por un componente en una caché en memoria. Pueden preguntar, ¿por qué debería hacer eso? De todos modos, estoy almacenando en caché el marcado de toda la página en Dispatcher, incluido el marcado de ese componente. Estamos de acuerdo. Lo hace, pero una vez por página. No está compartiendo ese marcado entre las páginas.

Imagine que está representando una navegación sobre cada página. El marcado tiene el mismo aspecto en cada página. Pero lo procesa una y otra vez para cada página, que no está en Dispatcher. Recuerde: después de la invalidación automática, todas las páginas deben volver a procesarse. Básicamente, se está ejecutando el mismo código con los mismos resultados cientos de veces.

Por nuestra experiencia, procesar una navegación superior anidada es una tarea muy costosa. Normalmente, se atraviesa una buena parte del árbol del documento para generar los elementos de navegación. Incluso si solo necesita el título de navegación y la dirección URL, las páginas deben cargarse en la memoria. Y aquí están obstruyendo recursos preciosos. Una y otra vez.

Sin embargo, el componente se comparte entre muchas páginas. Y compartir algo es un indicador del uso de una caché. Por lo tanto, lo que debe hacer es comprobar si el componente de navegación ya se ha procesado y almacenado en caché y, en lugar de volver a procesarlo, emitir el valor cachés.

Hay dos maravillosas sutilezas de ese esquema que se pueden pasar por alto fácilmente:

1. Está almacenando en caché una cadena Java. Una cadena no tiene ninguna referencia saliente y es inmutable. Por lo tanto, teniendo en cuenta las advertencias anteriores, esto es muy seguro.

2. La invalidación también es súper fácil. Siempre que algo cambie en su sitio web, quiere invalidar esta entrada de caché. Reconstruir es relativamente barato, ya que necesita ser realizado solo una vez y luego es reutilizado por todos los cientos de páginas.

Esto supone un gran alivio para los servidores de publicación.

### Implementación de cachés de fragmentos

#### Etiquetas personalizadas

En los viejos tiempos, cuando se utilizaba JSP como motor de creación de plantillas, era bastante común utilizar un ajuste de etiqueta JSP personalizado alrededor del código de procesamiento de componentes.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

La etiqueta personalizada que capturaría su cuerpo y lo escribiría en la caché o impediría la ejecución de su cuerpo y emitiría la carga útil de la entrada de caché en su lugar.

La &quot;clave&quot; es la ruta de componentes que tendría en la página principal. No utilizamos la ruta del componente en la página actual, ya que esto crearía una entrada de caché por página, lo que contradiría nuestra intención de compartir ese componente. Tampoco utilizamos solo la ruta relativa de los componentes (`jcr:conten/mainnavigation`), ya que esto nos impediría usar diferentes componentes de navegación en diferentes sitios.

&quot;Caché&quot; es un indicador de dónde almacenar la entrada. Normalmente, hay más de una caché en la que se almacenan los elementos. Cada uno de ellos puede comportarse de forma un poco diferente. Por lo tanto, es bueno diferenciar lo que se almacena, incluso si al final son solo cadenas.

&quot;Dependencia&quot;, de esto depende la entrada de caché. La caché de &quot;navegación principal&quot; puede tener una regla que indica que si hay algún cambio debajo del nodo &quot;dependencia&quot;, se debe purgar la entrada correspondiente. Por lo tanto, la implementación de la caché tendría que registrarse a sí misma como detector de eventos en el repositorio para tener en cuenta los cambios y, a continuación, aplicar las reglas específicas de la caché para averiguar qué debe invalidarse.

Lo anterior fue solo un ejemplo. También puede elegir tener un árbol de cachés. Cuando el primer nivel se utiliza para separar sitios (o inquilinos) y el segundo nivel luego se divide en tipos de contenido (por ejemplo, &quot;navegación principal&quot;), lo que podría ahorrarle agregar la ruta de las páginas principales como en el ejemplo anterior.

Por cierto, también puede utilizar este enfoque con componentes más modernos basados en HTL. Luego tendría un envoltorio JSP alrededor de su script HTL.

#### Filtros de componentes

Sin embargo, en un enfoque HTL puro, preferiría crear la caché de fragmentos con un filtro de componente Sling. Todavía no hemos visto esto en la naturaleza, pero ese es el enfoque que adoptaríamos en ese tema.

#### Sling Dynamic Include

La caché de fragmentos se utiliza si tiene algo constante (la navegación) en el contexto de un entorno cambiante (páginas diferentes).

Pero también puede tener lo contrario, un contexto relativamente constante (una página que rara vez cambia) y algunos fragmentos que cambian constantemente en esa página (por ejemplo, un marcador en vivo).

En este caso, puede dar [Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html) una oportunidad. Básicamente, se trata de un filtro de componentes, que se ajusta al componente dinámico y, en lugar de procesar el componente en la página, crea una referencia. Esta referencia puede ser una llamada de Ajax para que el componente se incluya en el explorador y, por lo tanto, la página circundante se pueda almacenar en caché de forma estática. O, alternativamente, Sling Dynamic Include puede generar una directiva SSI (Server Side Include). Esta directiva se ejecutaría en el servidor Apache. Incluso puede utilizar las directivas ESI - Edge Side Include si utiliza Barnish o una CDN que admita scripts ESI.

![Diagrama de secuencia de una solicitud mediante la inclusión dinámica de Sling](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagrama de secuencia de una solicitud mediante la inclusión dinámica de Sling*

<br> 

La documentación de SDI indica que debe deshabilitar el almacenamiento en caché para las URL que terminan en &quot;*.nocache.html&quot;, lo que tiene sentido, ya que está tratando con componentes dinámicos.

Puede ver otra opción sobre cómo utilizar SDI: Si _no_ Si se deshabilita la caché de Dispatcher para las inclusiones, Dispatcher actuará como una caché de fragmentos similar a la que describimos en el último capítulo: las páginas y los fragmentos de componentes de forma equitativa e independiente se almacenan en la caché de Dispatcher y se vinculan entre sí mediante el script SSI en el servidor Apache cuando se solicita la página. Al hacerlo, puede implementar componentes compartidos como la navegación principal (dado que siempre utiliza la misma URL del componente).

Eso debería funcionar, en teoría. Pero...

Se recomienda no hacerlo: perdería la capacidad de omitir la caché para los componentes dinámicos reales. SDI está configurado globalmente y los cambios que haría para su &quot;pobre-mans-fragment-cache&quot; también se aplicarían a los componentes dinámicos.

Le recomendamos que estudie detenidamente la documentación de SDI. Hay algunas otras limitaciones, pero el SDI es una herramienta valiosa en algunos casos.

#### Referencias

* [docs.oracle.com - Cómo escribir etiquetas JSP personalizadas](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß: Creación y uso de filtros de componentes](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [AEM helpx.adobe.com: Configuración de Sling Dynamic Includes en la](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Almacenamiento en caché de modelo

![Almacenamiento en caché basado en modelos: un objeto comercial con dos procesamientos diferentes](assets/chapter-3/model-based-caching.png)

*Almacenamiento en caché basado en modelos: un objeto comercial con dos procesamientos diferentes*

<br> 

Volvamos a examinar el caso con la navegación. Suponíamos que cada página requeriría el mismo marcado de la navegación.

Pero tal vez, ese no es el caso. Es posible que desee representar un marcado diferente para el elemento en la navegación que representa el _página actual_.

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

Estas son dos representaciones completamente diferentes. Sin embargo, el _objeto comercial_ - el árbol de navegación completo - es el mismo.  El _objeto comercial_  habría un gráfico de objetos que representaría los nodos del árbol. Este gráfico se puede almacenar fácilmente en una caché en memoria. Recuerde, sin embargo, que este gráfico no debe contener ningún objeto ni hacer referencia a ningún objeto que no haya creado usted mismo, especialmente ahora los nodos JCR.

#### Almacenamiento en caché en el explorador

Ya hemos tocado la importancia del almacenamiento en caché en el navegador, y hay muchos buenos tutoriales por ahí. Al final, para el explorador, Dispatcher es solo un servidor web que sigue el protocolo HTTP.

Sin embargo, a pesar de la teoría, hemos reunido algunos fragmentos de conocimiento que no hemos encontrado en ningún otro lugar y que queremos compartir.

En esencia, el almacenamiento en caché del explorador se puede aprovechar de dos formas diferentes,

1. El explorador tiene un recurso en caché del que conoce la fecha de caducidad exacta. En ese caso, no vuelve a solicitar el recurso.

2. El explorador tiene un recurso, pero no está seguro de si sigue siendo válido. En ese caso, se lo pediría al servidor web (Dispatcher en nuestro caso). Indique el recurso si se ha modificado desde la última vez que lo entregó. Si no ha cambiado, el servidor responde con &quot;304 - no cambiado&quot; y solo se transmiten los metadatos.

#### Depuración

Si está optimizando la configuración de Dispatcher para el almacenamiento en caché del explorador, es extremadamente útil utilizar un servidor proxy de escritorio entre el explorador y el servidor web. Preferimos &quot;Charles Web Debugging Proxy&quot; de Karl von Randow.

Con Charles, puede leer las solicitudes y respuestas, que se transmiten al servidor y desde él. Y - usted puede aprender mucho sobre el protocolo HTTP. Los exploradores modernos también ofrecen algunas funciones de depuración, pero las características de un proxy de escritorio no tienen precedentes. Puede manipular los datos transferidos, acelerar la transmisión, reproducir solicitudes únicas y mucho más. Y la interfaz de usuario está claramente organizada y es bastante completa.

La prueba más básica es usar el sitio web como un usuario normal - con el proxy en el medio - y verificar en el proxy si el número de las solicitudes estáticas (a /etc/...) se está haciendo más pequeño con el tiempo - ya que estos deben estar en la caché y no se deben solicitar más.

Encontramos que un proxy puede proporcionar una visión general más clara, ya que las solicitudes en caché no aparecen en el registro, mientras que algunos depuradores integrados en el explorador siguen mostrando estas solicitudes con &quot;0 ms&quot; o &quot;del disco&quot;. Lo cual está bien y es preciso, pero podría nublar tu vista un poco.

A continuación, puede explorar en profundidad y comprobar los encabezados de los archivos transferidos para ver, por ejemplo, si los encabezados http &quot;Caduca&quot; son correctos. Puede reproducir solicitudes con encabezados if-modified-since configurados para ver si el servidor responde correctamente con un código de respuesta 304 o 200. Puede observar el tiempo de las llamadas asincrónicas y también puede probar las suposiciones de seguridad en cierto grado. ¿Recuerdas que te dijimos que no aceptaras todos los selectores que no se esperan explícitamente? Aquí puede jugar con la dirección URL y los parámetros y ver si la aplicación se comporta bien.

Solo hay una cosa que le pedimos que no haga, cuando depure su caché:

No vuelva a cargar las páginas en el explorador.

Una &quot;recarga del explorador&quot;, un _simple-reload_ así como un _recarga forzada_ (&quot;_mayús-recarga_&quot;) no es lo mismo que una solicitud de página normal. Una simple solicitud de recarga establece un encabezado

```
Cache-Control: max-age=0
```

Y una Mayús-Recarga (mantener pulsada la tecla &quot;Mayús&quot; mientras se hace clic en el botón de recarga) suele configurar un encabezado de solicitud

```
Cache-Control: no-cache
```

Ambos encabezados tienen efectos similares pero ligeramente diferentes, pero lo más importante es que difieren completamente de una solicitud normal cuando se abre una dirección URL desde la ranura de la dirección URL o mediante vínculos en el sitio. La exploración normal no establece encabezados Cache-Control, pero probablemente sea un encabezado if-modified-since.

Por lo tanto, si desea depurar el comportamiento de navegación normal, debe hacer exactamente lo siguiente: _Explorar normalmente_. El uso del botón de recarga del navegador es la mejor manera de no ver errores de configuración de caché en la configuración.

Use su Charles Proxy para ver de qué estamos hablando. Sí, y mientras lo tenga abierto, puede reproducir las solicitudes allí mismo. No es necesario volver a cargar desde el explorador.

## Pruebas de rendimiento

Al utilizar un proxy, se tiene una idea del comportamiento temporal de las páginas. Por supuesto, eso no es, ni mucho menos, una prueba de rendimiento.  Una prueba de rendimiento requeriría que varios clientes solicitaran las páginas en paralelo.

Un error común, que hemos visto con demasiada frecuencia, es que la prueba de rendimiento solo incluye un número súper pequeño de páginas y estas páginas se entregan únicamente desde la caché de Dispatcher.

Si está promocionando la aplicación al sistema activo, la carga es completamente diferente de la que ha probado.

En el sistema activo, el patrón de acceso no es un número tan pequeño de páginas distribuidas equitativamente que tiene en las pruebas (página de inicio y pocas páginas de contenido). El número de páginas es mucho mayor y las solicitudes se distribuyen de forma muy desigual. Y, por supuesto, las páginas en directo no se pueden servir al 100 % desde la caché: hay solicitudes de invalidación procedentes del sistema de publicación que invalidan automáticamente una gran parte de sus valiosos recursos.

Ah, sí. Y cuando esté reconstruyendo la caché de Dispatcher, se dará cuenta de que el sistema de publicación también se comporta de forma muy diferente, dependiendo de si solicita solo un puñado de páginas o un número mayor. Incluso si todas las páginas son igualmente complejas: su número juega un papel. ¿Recuerdas lo que dijimos sobre el almacenamiento en caché encadenado? Si siempre solicita el mismo número pequeño de páginas, lo más probable es que los bloques correspondientes con los datos sin procesar estén en la caché de los discos duros o que el sistema operativo almacene en caché los bloques. Además, existe una buena probabilidad de que el repositorio haya almacenado en caché el segmento correspondiente en su memoria principal. Por lo tanto, volver a procesar es significativamente más rápido que cuando otras páginas se expulsaban entre sí de vez en cuando de varias cachés.

El almacenamiento en caché es difícil, al igual que las pruebas de un sistema que depende del almacenamiento en caché. Entonces, ¿qué se puede hacer para tener un escenario más preciso en la vida real?

Creemos que debería realizar más de una prueba y proporcionar más de un índice de rendimiento como medida de la calidad de la solución.

Si ya tiene un sitio web, mida el número de solicitudes y cómo se distribuyen. Intente modelar una prueba que utilice una distribución similar de solicitudes. Añadir algo de aleatoriedad no podría doler. No es necesario simular un explorador que cargue recursos estáticos como JS y CSS, ya que en realidad no importan. Finalmente, se almacenan en caché en el explorador o en Dispatcher y no se añaden a la carga de forma significativa. Pero las imágenes referenciadas sí importan. Encuentre también su distribución en archivos de registro antiguos y modele un patrón de solicitud similar.

Ahora realice una prueba con el Dispatcher sin almacenar en caché. Ese es tu peor escenario. Averigüe a qué carga máxima se está volviendo inestable su sistema en estas peores condiciones. También puede empeorarlo si elimina algunas de las secciones de Dispatcher/Publish si lo desea.

A continuación, realice la misma prueba con todos los ajustes de caché necesarios en &quot;activado&quot;. Aumente sus solicitudes paralelas lentamente para calentar la caché y ver cuánto puede tomar su sistema en estas mejores condiciones.

Un escenario de caso promedio sería ejecutar la prueba con Dispatcher habilitado, pero también con algunas invalidaciones ocurriendo. Puede simular que tocando los archivos de estado mediante un trabajo de crono o enviando las solicitudes de invalidación a intervalos irregulares a Dispatcher. No olvide purgar también algunos de los recursos no invalidados automáticamente de vez en cuando.

Puede variar el último escenario aumentando las solicitudes de invalidación y la carga.

Es un poco más complejo que una prueba de carga lineal, pero ofrece mucha más confianza en su solución.

Puede que te alejes del esfuerzo. Pero por favor realice una prueba en el peor de los casos en el sistema Publish con un mayor número de páginas (distribuidas equitativamente) para ver los límites del sistema. Asegúrese de interpretar correctamente el número del mejor escenario y proporcione a sus sistemas suficiente margen de ampliación.
