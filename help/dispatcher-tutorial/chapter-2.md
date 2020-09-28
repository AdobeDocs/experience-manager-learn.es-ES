---
title: Capítulo 2 - Definición de modelos de fragmentos de contenido de Evento
seo-title: Introducción a Servicios de contenido de AEM - Capítulo 2 - Definición de modelos de fragmentos de contenido de Evento
description: El capítulo 2 del tutorial sin encabezado de AEM cubre la habilitación y definición de modelos de fragmentos de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para crear Eventos.
seo-description: El capítulo 2 del tutorial sin encabezado de AEM cubre la habilitación y definición de modelos de fragmentos de contenido utilizados para definir una estructura de datos normalizada y una interfaz de creación para crear Eventos.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1917'
ht-degree: 0%

---


# Capítulo 2 - Infraestructura

## Configuración de una infraestructura de almacenamiento en caché

Hemos introducido la topología básica de un sistema de publicación y un despachante en el capítulo 1 de esta serie. Un conjunto de servidores Publish y Dispatcher se puede configurar en muchas variaciones, según la carga esperada, la topología de los centros de datos y las propiedades de failover deseadas.

Esbozaremos las topologías más comunes y describiremos las ventajas y dónde se quedan cortos. La lista - por supuesto - nunca puede ser integral. El único límite es tu imaginación.

### La configuración &quot;heredada&quot;

En los primeros días, el número de visitantes potenciales era pequeño, el hardware era caro y los servidores web no eran tan críticos para el negocio como hoy en día. Una configuración común consistía en que un despachante actuara como equilibrador de carga y caché frente a dos o más sistemas de publicación. El servidor Apache en el centro del Dispatcher era muy estable y, en la mayoría de los entornos, lo suficientemente capaz como para satisfacer una cantidad decente de solicitudes.

![Configuración del despachante &quot;heredado&quot; - No muy común según los estándares actuales](assets/chapter-2/legacy-dispatcher-setup.png)

*Configuración del despachante &quot;heredado&quot; - No muy común según los estándares actuales*

<br> 

Aquí es donde el despachante recibió su nombre de: Básicamente enviaba solicitudes. Esta configuración ya no es muy común, ya que no puede satisfacer las mayores exigencias de rendimiento y estabilidad requeridas en la actualidad.

### Configuración de varias piernas

Hoy en día es más común una topología ligeramente diferente. Una topología con varios patas tendría un despachante por servidor de publicación. Un equilibrador de carga dedicado (hardware) se sienta delante de la infraestructura de AEM que envía las solicitudes a estas dos (o más) patas:

![Configuración moderna de Dispatcher &quot;estándar&quot;: fácil de manejar y mantener](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Configuración moderna de Dispatcher &quot;estándar&quot;: fácil de manejar y mantener*

<br> 

Estas son las razones de este tipo de configuración,

1. Los sitios Web, en promedio, ofrecen mucho más tráfico que en el pasado. Por lo tanto, es necesario ampliar la &quot;infraestructura Apache&quot;.

2. La configuración de &quot;Legacy&quot; no proporcionó redundancia en el nivel de Dispatcher. Si Apache Server se apagaba, todo el sitio web estaba inaccesible.

3. Los servidores Apache son baratos. Se basan en código abierto y, dado que tiene un centro de datos virtual, se pueden aprovisionar muy rápido.

4. Esta configuración proporciona una manera sencilla de realizar una actualización &quot;móvil&quot; o &quot;escalonada&quot;. Basta con cerrar Dispatcher 1 al instalar un nuevo paquete de software en Publish 1. Una vez finalizada la instalación y que haya probado suficientemente el humo, haga clic en Publicar 1 desde la red interna, limpie la caché en Dispatcher 1 y vuelva a inicio mientras desinstala Dispatcher 2 para el mantenimiento de Publicar 2.

5. La invalidación de caché se vuelve muy fácil y determinante en esta configuración. Dado que solo un sistema de publicación está conectado a un despachante, solo hay un despachante para invalidar. El orden y el momento de la invalidación son triviales.

### Configuración de &quot;Escalar hacia fuera&quot;

Los servidores Apache son baratos y fáciles de aprovisionar, por qué no empujar la ampliación de ese nivel un poco más. ¿Por qué no hay dos o más despachantes delante de cada servidor de publicación?

![Configuración de &quot;Escalar&quot;: tiene algunas áreas de aplicación pero también limitaciones y advertencias](assets/chapter-2/scale-out-setup.png)

*Configuración de &quot;Escalar&quot;: tiene algunas áreas de aplicación pero también limitaciones y advertencias*

<br> 

¡Puedes hacer eso! Y hay muchos escenarios de aplicación válidos para esa configuración. Pero también hay algunas limitaciones y complejidades que debe tener en cuenta.

#### Invalidación

Cada sistema de publicación está conectado a una multitud de despachantes, cada uno de los cuales debe invalidarse cuando el contenido se ha cambiado.

#### Mantenimiento

Huelga decir que la configuración inicial de los sistemas Dispatcher y Publish es un poco más compleja. Pero también tenga en cuenta que el esfuerzo de una versión &quot;móvil&quot; también es un poco mayor. Los sistemas AEM pueden y deben actualizarse durante la ejecución. Pero es prudente no hacer eso mientras están sirviendo solicitudes activamente. Normalmente, solo se desea actualizar una parte de los sistemas de publicación, mientras que los demás siguen ofreciendo tráfico y, después de realizar la prueba, se cambian a la otra parte. Si tiene suerte y puede acceder al equilibrador de carga en el proceso de implementación, puede desactivar el enrutamiento para los servidores que están en mantenimiento aquí. Si está en un equilibrador de carga compartido sin acceso directo, preferiría cerrar los despachantes de la publicación que desea actualizar. Cuanto más haya, más tendrá que cerrar. Si hay un gran número y está planeando actualizaciones frecuentes, se aconseja cierta automatización. Si no dispone de herramientas de automatización, la reducción de escala es una mala idea de todos modos.

En un proyecto anterior utilizamos un truco diferente para eliminar un sistema de publicación del equilibrio de carga sin tener acceso directo al propio equilibrador de carga.

El equilibrador de carga generalmente &quot;pings&quot;, una página en particular para ver si el servidor está activo y en ejecución. Una opción trivial es generalmente hacer ping en la página principal. Pero si desea utilizar el ping para indicar al equilibrador de carga que no equilibre el tráfico, elegirá otra cosa. Puede crear una plantilla o un servlet dedicado que se pueda configurar para responder con `"up"` o `"down"` (en el cuerpo o como código de respuesta http). La respuesta de esa página, por supuesto, no debe almacenarse en caché en el despachante, por lo que siempre se obtiene directamente del sistema de publicación. Ahora, si configura el equilibrador de carga para que compruebe esta plantilla o servlet, puede dejar que Publish &quot;pretenda&quot; que esté inactivo. No formaría parte del equilibrio de carga y se puede actualizar.

#### Distribución mundial

&quot;Distribución mundial&quot; es una configuración de &quot;Escalar&quot; en la que tiene varios despachantes frente a cada sistema de publicación; ahora se distribuye por todo el mundo para estar más cerca del cliente y proporcionar un mejor rendimiento. Por supuesto, en ese escenario no tiene un equilibrador de carga central sino un esquema de equilibrio de carga basado en DNS y geo-IP.

>[!NOTE]
>
>En realidad, está creando una especie de red de distribución de contenido (CDN) con ese enfoque, por lo que debería considerar la posibilidad de comprar una solución CDN lista para usar en lugar de crearla usted mismo. Crear y mantener una CDN personalizada no es una tarea trivial.

#### Escala horizontal

Incluso en un centro de datos local, una topología de &quot;Escalar fuera&quot; con varios despachantes frente a cada sistema de publicación tiene algunas ventajas. Si ve cuellos de botella de rendimiento en los servidores Apache debido al tráfico elevado (y a una buena tasa de visitas en caché) y ya no puede aumentar el tamaño del hardware (al agregar CPU, RAM y discos más rápidos), puede aumentar el rendimiento agregando despachantes. Esto se denomina &quot;escala horizontal&quot;. Sin embargo, esto tiene límites, especialmente cuando se invalida el tráfico con frecuencia. Describiremos el efecto en la siguiente sección.

#### Límites de la topología de escalado de salida

Añadir servidores proxy normalmente incrementará el rendimiento. Sin embargo, existen situaciones en las que la adición de servidores puede reducir el rendimiento. ¿Cómo? Considere que tiene un portal de noticias, donde presenta nuevos artículos y páginas cada minuto. Un despachante invalida por &quot;auto-invalidación&quot;: Cada vez que se publica una página, se invalidan todas las páginas de la caché del mismo sitio. Esta es una característica útil - hemos cubierto esto en el [Capítulo 1](chapter-1.md) de esta serie - pero también significa que cuando usted tiene cambios frecuentes en su sitio web usted está invalidando la caché con bastante frecuencia. Si solo tiene una instancia de Dispatcher por publicación, el primer visitante que solicita una página, se desencadena un nuevo almacenamiento en caché de dicha página. El segundo visitante ya obtiene la versión en caché.

Si tiene dos despachantes, el segundo visitante tiene un 50 % de probabilidad de que la página no se almacene en caché y, a continuación, experimentará una latencia mayor cuando se vuelva a procesar esa página. Tener incluso más despachantes por publicación empeora las cosas. Lo que sucede es que el servidor de publicación recibe más carga porque tiene que volver a procesar la página para cada despachante por separado.

![Se ha reducido el rendimiento en un escenario de escala con frecuentes vaciados de caché.](assets/chapter-2/decreased-performance.png)

*Se ha reducido el rendimiento en un escenario de escala con frecuentes vaciados de caché.*

<br> 

#### Mitigación de problemas de sobreescala

Puede considerar usar un almacenamiento compartido central para todos los despachantes o sincronizar los file systems de los servidores Apache para mitigar los problemas. Sólo podemos ofrecer una experiencia de primera mano limitada, pero estamos preparados para que esto sume la complejidad del sistema y pueda introducir una nueva clase de errores.

Hemos tenido algunos experimentos con NFS - pero NFS presenta enormes problemas de rendimiento debido al bloqueo de contenido. Esto en realidad disminuyó el rendimiento general.

**Conclusión** : Compartir un sistema de archivos común entre varios despachantes NO es un método recomendado.

Si se enfrentan a problemas de rendimiento, aumente la escala de Publicar y Distribuidores de forma equitativa para evitar la carga máxima en las instancias de Publisher. No existe una regla de oro sobre la proporción Publicar/Dispatcher; depende en gran medida de la distribución de las solicitudes y la frecuencia de las publicaciones y las invalidaciones de caché.

Si también le preocupa la latencia que experimenta un visitante, considere el uso de una red de envío de contenido, la recuperación de caché, el calentamiento de caché preventivo, la configuración de un tiempo de gracia como se describe en el [Capítulo 1](chapter-1.md) de esta serie o consulte algunas ideas avanzadas de la [Parte 3](chapter-3.md).

### La configuración de &quot;Conexión cruzada&quot;

Otra configuración que hemos visto de vez en cuando es la configuración &quot;interconectada&quot;: Las instancias de Publicar no tienen despachantes dedicados pero todos los despachantes están conectados a todos los sistemas de Publicar.

![Topología interconectada: Mayor redundancia y mayor complejidad](assets/chapter-2/cross-connected-setup.png)

<br> 

*Topología interconectada: Mayor redundancia y mayor complejidad.*

A primera vista, esto ofrece más redundancia para un presupuesto relativamente pequeño. Cuando uno de los servidores Apache está inactivo, aún puede tener dos sistemas de publicación que hagan el procesamiento. Además, si uno de los sistemas de publicación se bloquea, aún tiene dos despachantes que sirven la carga en caché.

Sin embargo, esto tiene un precio.

Primero, sacar una pierna para el mantenimiento es bastante complicado. En realidad, para esto se diseñó este sistema; para ser más resiliente y seguir funcionando por todos los medios posibles. Hemos visto complicados planes de mantenimiento sobre cómo lidiar con esto. Vuelva a configurar el Dispatcher 2 primero, quitando la conexión cruzada. Reiniciando Dispatcher 2. Apagar Dispatcher 1, actualizar Publish 1, ..., etc. Debe considerar cuidadosamente si eso escala hasta más de dos patas. Llegarán a la conclusión de que en realidad aumenta la complejidad, los costos y es una fuente formidable de errores humanos. Sería mejor automatizar esto. Así que mejor compruebe, si realmente tiene los recursos humanos para incluir esta tarea de automatización en su programación de proyectos. Si bien puede ahorrar algunos costes de hardware con esto, puede gastar dobles en el personal de TI.

En segundo lugar, es posible que se esté ejecutando alguna aplicación de usuario en la AEM que requiera un inicio de sesión. Las sesiones adhesivas se utilizan para garantizar que un usuario siempre se proporciona desde la misma instancia de AEM para que pueda mantener el estado de la sesión en esa instancia. Al tener esta configuración interconectada, debe asegurarse de que las sesiones adhesivas funcionen correctamente en el equilibrador de carga y en los despachantes. No es imposible, pero hay que estar atento a eso y añadir algunas horas de configuración y prueba adicionales, que, de nuevo, podrían igualar los ahorros que se habían planeado ahorrando hardware.

### Conclusión

No se recomienda utilizar este esquema de conexión cruzada como opción predeterminada. Sin embargo, si decide utilizarla, querrá evaluar cuidadosamente los riesgos y los costes ocultos y planear incluir la automatización de la configuración como parte de su proyecto.

## Etapa siguiente

* [3 - Temas avanzados de almacenamiento en caché](chapter-3.md)
