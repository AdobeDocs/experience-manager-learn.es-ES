---
title: 'Capítulo 2: Infraestructura de Dispatcher'
description: Comprender la topología de publicación y Dispatcher. Obtenga información acerca de las topologías y configuraciones más comunes.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
doc-type: Tutorial
exl-id: a25b6f74-3686-40a9-a148-4dcafeda032f
duration: 403
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# Capítulo 2 - Infraestructura

## Configuración de una infraestructura de almacenamiento en caché

En el capítulo 1 de esta serie presentamos la topología básica de un sistema de publicación y un despachante. Un conjunto de servidores de publicación y Dispatcher se puede configurar en muchas variaciones, según la carga esperada, la topología de los centros de datos y las propiedades de conmutación por error deseadas.

Esbozaremos las topologías más comunes y describiremos las ventajas y dónde se quedan cortas. La lista, por supuesto, nunca puede ser exhaustiva. El único límite es tu imaginación.

### La configuración &quot;heredada&quot;

En los primeros tiempos, el número de visitantes potenciales era pequeño, el hardware era caro y los servidores web no se veían tan críticos para el negocio como lo son hoy en día. Una configuración común era tener una instancia de Dispatcher que sirviera como equilibrador de carga y caché delante de dos o más sistemas de publicación. El servidor Apache en el núcleo de Dispatcher era muy estable y, en la mayoría de los ajustes, lo suficientemente capaz como para servir una cantidad decente de solicitudes.

![Configuración de Dispatcher &quot;heredada&quot;: no es muy común según los estándares actuales](assets/chapter-2/legacy-dispatcher-setup.png)

*Configuración de Dispatcher &quot;heredada&quot;: no es muy común según los estándares actuales*

<br> 

Aquí es desde donde el despachante recibió su nombre: básicamente enviaba solicitudes. Esta configuración ya no es muy común, ya que no puede satisfacer las mayores exigencias de rendimiento y estabilidad requeridas hoy en día.

### Configuración de varias piernas

Hoy en día, una topología ligeramente diferente es más común. Una topología de varias patas tendría una instancia de Dispatcher por servidor de publicación. AEM Un equilibrador de carga (hardware) específico se encuentra delante de la infraestructura de la que envía las solicitudes a estos dos (o más) componentes:

![Configuración moderna &quot;estándar&quot; de Dispatcher: fácil de manejar y mantener](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Configuración moderna &quot;estándar&quot; de Dispatcher: fácil de manejar y mantener*

<br> 

Estas son las razones para este tipo de configuración,

1. Los sitios web en promedio sirven mucho más tráfico que en el pasado. Por lo tanto, existe la necesidad de ampliar la &quot;infraestructura Apache&quot;.

2. La configuración &quot;Heredada&quot; no proporcionaba redundancia a nivel de Dispatcher. Si el servidor Apache se desactivaba, todo el sitio web estaba inaccesible.

3. Los servidores Apache son baratos. Se basan en código abierto y, dado que tiene un centro de datos virtual, se pueden aprovisionar muy rápido.

4. Esta configuración proporciona una forma sencilla de realizar una actualización &quot;móvil&quot; o &quot;escalonada&quot;. Simplemente apague Dispatcher 1 mientras instala un nuevo paquete de software en Publish 1. Cuando finalice la instalación y haya probado suficientemente el humo Publish 1 de la red interna, limpie la caché de Dispatcher 1 y vuelva a iniciarla mientras elimina Dispatcher 2 para el mantenimiento de Publish 2.

5. La invalidación de la caché se vuelve muy fácil y determinista en esta configuración. Como solo hay un sistema de publicación conectado a un Dispatcher, solo hay un Dispatcher para invalidar. El orden y el momento de la invalidación es trivial.

### La configuración &quot;Escalar fuera&quot;

Los servidores Apache son baratos y fáciles de aprovisionar, ¿por qué no aumentar un poco más el escalado? ¿Por qué no tener dos o más Dispatcher delante de cada servidor de publicación?

![Configuración de &quot;Ampliación&quot;: tiene algunas áreas de aplicación, pero también limitaciones y advertencias](assets/chapter-2/scale-out-setup.png)

*Configuración de &quot;Ampliación&quot;: tiene algunas áreas de aplicación, pero también limitaciones y advertencias*

<br> 

¡Puedes hacer eso! Y hay muchos escenarios de aplicación válidos para esa configuración. Pero también hay algunas limitaciones y complejidades que debe tener en cuenta.

#### Invalidación

Cada sistema de publicación está conectado a una multitud de distribuidores, cada uno de los cuales debe invalidarse cuando se cambie el contenido.

#### Mantenimiento

Huelga decir que la configuración inicial de los sistemas Dispatcher y Publish es un poco más compleja. Pero también tenga en cuenta que el esfuerzo de una versión &quot;móvil&quot; también es un poco mayor. AEM Los sistemas de pueden y deben actualizarse mientras se ejecuta. Pero es aconsejable no hacerlo mientras estén atendiendo solicitudes activamente. Por lo general, solo desea actualizar una parte de los sistemas de publicación, mientras que los demás siguen sirviendo tráfico de forma activa y luego, después de probar, cambiar a la otra parte. Si tiene suerte y puede acceder al equilibrador de carga en su proceso de implementación, puede desactivar el enrutamiento a los servidores que se encuentran en mantenimiento aquí. Si está en un equilibrador de carga compartido sin acceso directo, preferiría cerrar los distribuidores de la publicación que desea actualizar. Cuanto más haya, más tendrás que cerrar. Si hay un gran número y planea actualizaciones frecuentes, se recomienda algo de automatización. Si no tiene herramientas de automatización, escalar es una mala idea de todos modos.

En un proyecto anterior utilizábamos un truco diferente para eliminar un sistema de publicación del equilibrio de carga sin tener acceso directo al propio equilibrador de carga.

El equilibrador de carga suele &quot;pings&quot;, una página en particular para ver si el servidor está en funcionamiento. Una opción trivial suele ser hacer ping en la página principal. Pero si desea utilizar el ping para indicar al equilibrador de carga que no equilibre el tráfico, debe elegir otra cosa. Puede crear una plantilla o servlet dedicado que se pueda configurar para responder con `"up"` o `"down"` (en el cuerpo o como código de respuesta http). La respuesta de esa página, por supuesto, no debe almacenarse en caché en Dispatcher; por lo tanto, siempre se obtiene del sistema de publicación. Ahora, si configura el equilibrador de carga para que compruebe esta plantilla o servlet, puede fácilmente dejar que el botón Publicar &quot;pretenda&quot; esté caído. No formaría parte del equilibrio de carga y se puede actualizar.

#### Distribución mundial

&quot;Distribución mundial&quot; es una configuración de &quot;Escalado horizontal&quot; en la que tiene varios Dispatcher delante de cada sistema de publicación, ahora distribuidos por todo el mundo para estar más cerca del cliente y proporcionar un mejor rendimiento. Por supuesto, en ese escenario no se tiene un equilibrador de carga central sino un esquema de equilibrio de carga basado en DNS y geo-IP.

>[!NOTE]
>
>En realidad, está creando una especie de red de distribución de contenido (CDN) con ese enfoque, por lo que debe considerar la posibilidad de comprar una solución de CDN comercial en lugar de crearla usted mismo. Crear y mantener una CDN personalizada no es una tarea trivial.

#### Escala horizontal

Incluso en un centro de datos local, una topología &quot;Escalar hacia fuera&quot; con varios Dispatcher delante de cada sistema de publicación tiene algunas ventajas. Si ve cuellos de botella de rendimiento en los servidores Apache debido al alto tráfico (y una buena tasa de aciertos de caché) y ya no puede escalar el hardware (agregando CPU, RAM y discos más rápidos), puede mejorar el rendimiento agregando Dispatchers. Esto se denomina &quot;escala horizontal&quot;. Sin embargo, esto tiene límites, especialmente cuando se invalida el tráfico con frecuencia. Describiremos el efecto en la siguiente sección.

#### Límites de la topología de escalado horizontal

La adición de servidores proxy debería aumentar el rendimiento. Sin embargo, hay escenarios en los que añadir servidores puede reducir el rendimiento. ¿Cómo? Considere que tiene un portal de noticias, donde introduce nuevos artículos y páginas cada minuto. Una instancia de Dispatcher invalida mediante &quot;invalidación automática&quot;: cada vez que se publica una página, todas las páginas de la caché del mismo sitio se invalidan. Esta es una función útil; hemos cubierto esto en [Capítulo 1](chapter-1.md) de esta serie, pero también significa que cuando tiene cambios frecuentes en su sitio web, invalida la caché con bastante frecuencia. Si solo tiene una instancia de Dispatcher por publicación, el primer visitante que solicite una página almacenará en déclencheur una nueva caché de esa página. El segundo visitante ya obtiene la versión en caché.

Si tiene dos instancias de Dispatcher, el segundo visitante tiene una probabilidad del 50 % de que la página no se almacene en caché y, a continuación, experimentaría una latencia mayor cuando se represente esa página. Tener aún más distribuidores por publicación empeora las cosas. Lo que sucede es que el servidor de publicación recibe más carga porque tiene que volver a procesar la página para cada Dispatcher por separado.

![Rendimiento disminuido en un escenario de escalado horizontal con vaciados frecuentes de la caché.](assets/chapter-2/decreased-performance.png)

*Rendimiento disminuido en un escenario de escalado horizontal con vaciados frecuentes de la caché.*

<br> 

#### Mitigación de problemas de sobreescala

Puede considerar la posibilidad de utilizar un almacenamiento compartido central para todos los Dispatcher o sincronizar los sistemas de archivos de los servidores Apache para mitigar los problemas. Podemos proporcionar solo una experiencia de primera mano limitada, pero estar preparados para que esto sume la complejidad del sistema y pueda introducir una nueva clase de errores.

Hemos realizado algunos experimentos con NFS, pero NFS presenta enormes problemas de rendimiento debido al bloqueo de contenido. De hecho, esto redujo el rendimiento general.

**Conclusión** - Compartir un sistema de archivos común entre varios distribuidores NO es un enfoque recomendado.

Si tiene problemas de rendimiento, escale las instancias de Publish y Dispatcher de la misma manera para evitar la carga máxima en las instancias de Publisher. No hay una regla de oro sobre la relación Publicación / Dispatcher: depende en gran medida de la distribución de las solicitudes y la frecuencia de las publicaciones y las invalidaciones de la caché.

Si también le preocupa la latencia que experimenta un visitante, considere la posibilidad de utilizar una red de distribución de contenido, recuperar la caché y calentarla de forma preventiva, estableciendo un tiempo de gracia como se describe en [Capítulo 1](chapter-1.md) de esta serie o consulte algunas ideas avanzadas de [Parte 3](chapter-3.md).

### La configuración de &quot;Conexión cruzada&quot;

Otra configuración que hemos visto de vez en cuando es la configuración de &quot;conexión cruzada&quot;: las instancias de publicación no tienen instancias de Dispatcher dedicadas, pero todas las instancias de Dispatcher están conectadas a todos los sistemas de publicación.

![Topología interconectada: mayor redundancia y mayor complejidad](assets/chapter-2/cross-connected-setup.png)

<br> 

*Topología interconectada: mayor redundancia y mayor complejidad.*

A primera vista, esto proporciona algo más de redundancia para un presupuesto relativamente pequeño. Cuando uno de los servidores Apache esté inactivo, aún puede tener dos sistemas de publicación haciendo el trabajo de procesamiento. Además, si uno de los sistemas de publicación se bloquea, aún tiene dos Dispatcher que sirven a la carga en caché.

Sin embargo, esto tiene un precio.

En primer lugar, sacar una pierna para el mantenimiento es bastante engorroso. En realidad, para esto fue diseñado este esquema; para ser más resiliente y mantenerse en marcha por todos los medios posibles. Hemos visto planes de mantenimiento complicados sobre cómo lidiar con esto. Vuelva a configurar primero Dispatcher 2 y elimine la conexión cruzada. Reiniciando Dispatcher 2. Cerrando Dispatcher 1, actualizando Publish 1, ... etc. Debe considerar cuidadosamente si eso se escala hasta más de dos patas. Llegarán a la conclusión de que en realidad aumenta la complejidad, los costos y es una fuente formidable de errores humanos. Lo mejor sería automatizar esto. Así que mejor verifíquelo, si realmente tiene los recursos humanos para incluir esta tarea de automatización en la programación de su proyecto. Aunque puede ahorrar algunos costes de hardware con esto, puede gastar el doble en personal de TI.

AEM En segundo lugar, es posible que tenga alguna aplicación de usuario en ejecución en la aplicación que requiere un inicio de sesión. AEM Las sesiones fijas se utilizan para garantizar que siempre se proporciona a un usuario desde la misma instancia de y que, de este modo, se pueda mantener el estado de la sesión en esa instancia. Con esta configuración de conexión cruzada, debe asegurarse de que las sesiones fijas funcionen correctamente en el equilibrador de carga y en los Dispatchers. No es imposible, pero debe tener en cuenta esto y añadir algunas horas de configuración y prueba adicionales, que - de nuevo - podrían nivelar los ahorros que había planeado ahorrando hardware.

### Conclusión

No se recomienda utilizar este esquema de conexión cruzada como opción predeterminada. Sin embargo, si decide usarlo, deberá evaluar cuidadosamente los riesgos y los costes ocultos y planificar la inclusión de la automatización de la configuración como parte del proyecto.

## Siguiente paso

* [3 - Temas avanzados de almacenamiento en caché](chapter-3.md)
