---
title: Dispatcher comprende el almacenamiento en caché
description: Comprenda cómo funciona el módulo de Dispatcher en su caché.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---

# Explicación del almacenamiento en caché

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Explicación de los archivos de configuración](./explanation-config-files.md)

Este documento explicará cómo se produce el almacenamiento en caché de Dispatcher y cómo se puede configurar

## Directorios de almacenamiento en caché

Utilizamos los siguientes directorios de caché predeterminados en nuestras instalaciones de línea de base

- Autor
   - `/mnt/var/www/author`
- Editor
   - `/mnt/var/www/html`

Cuando cada solicitud atraviesa Dispatcher, las solicitudes siguen las reglas configuradas para mantener una versión en caché local como respuesta a los elementos aptos

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

AEM Intencionalmente, mantenemos la carga de trabajo publicada separada de la carga de trabajo de autor porque cuando Apache busca un archivo en DocumentRoot, no sabe de qué instancia vino. Por lo tanto, aunque tenga la caché deshabilitada en el conjunto de servidores de creación, si el DocumentRoot del autor es el mismo que el editor, servirá los archivos de la caché cuando esté presente. Esto significa que servirá archivos de autor para desde la caché publicada y creará una experiencia de coincidencia de mezcla realmente horrible para sus visitantes.

Mantener directorios DocumentRoot separados para contenido publicado diferente también es una muy mala idea. Tendrá que crear varios elementos en caché que no difieran entre sitios, como clientlibs, así como tener que configurar un agente de vaciado de replicación para cada DocumentRoot que configure. Aumentar la cantidad de vaciado con cada activación de página. Confíe en el área de nombres de los archivos y sus rutas de acceso en caché completas y evite varios DocumentRoot para los sitios publicados.
</div>

## Archivos de configuración

Dispatcher controla lo que califica como caché en la `/cache {` de cualquier archivo de granja. 
En las granjas de configuración de línea de base de AMS, encontrará nuestras inclusiones como se muestra a continuación:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Al crear las reglas para qué almacenar en caché o no, consulte la documentación [aquí](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Autor de almacenamiento en caché

Hay muchas implementaciones que hemos visto en las que las personas no almacenan en caché el contenido del autor. 
Se están perdiendo una gran actualización en rendimiento y capacidad de respuesta a sus autores.

Hablemos de la estrategia utilizada al configurar nuestra granja de autores para almacenar correctamente la caché.

Aquí tiene un autor base `/cache {` de nuestro archivo de granja de autores:


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    } 
    /invalidate { 
        /0000 { 
            /glob "*" 
            /type "allow" 
        } 
    } 
    /allowedClients { 
        /0000 { 
            /glob "*.*.*.*" 
            /type "deny" 
        } 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

Lo importante que hay que tener en cuenta aquí es que la `/docroot` se establece en el directorio de caché del autor.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Asegúrese de que su `DocumentRoot` en el del autor `.vhost` el archivo coincide con las granjas `/docroot` parámetro
</div>

Las reglas de caché incluyen una instrucción que incluye el archivo `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` que contiene estas reglas:

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

En el caso de un autor, el contenido cambia todo el tiempo y a propósito. Solo desea almacenar en caché elementos que no cambiarán con frecuencia.
Tenemos reglas para almacenar en caché `/libs` AEM porque forman parte de la instalación de línea de base de la aplicación y cambiarían hasta que haya instalado un paquete de servicio, un paquete de correcciones acumulativas, una actualización o una revisión. Por lo tanto, el almacenamiento en caché de estos elementos tiene mucho sentido y realmente tiene grandes beneficios de la experiencia de creación de los usuarios finales que utilizan el sitio.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que estas reglas también almacenan en caché <b>`/apps`</b> aquí es donde reside el código de aplicación personalizado. Si está desarrollando su código en esta instancia, resultará muy confuso cuando guarde el archivo y no vea si se refleja en la interfaz de usuario debido a que sirve una copia en caché. AEM La intención aquí es que si realiza una implementación del código en la que también lo haga con poca frecuencia y parte de los pasos de implementación deberían ser borrar la caché de creación. Una vez más, la ventaja es enorme, lo que hace que su código almacenable en caché se ejecute más rápido para los usuarios finales.
</div>


## ServeOnStale (también conocido como Serve on Stale / SOS)

Esta es una de esas joyas de una funcionalidad de Dispatcher. Si el editor está bajo carga o no responde, normalmente genera un código de respuesta 502 o 503 http. Si eso sucede y esta función está habilitada, se le indicará a Dispatcher que siga mostrando el contenido que aún esté en la caché como mejor esfuerzo, aunque no sea una copia nueva. Es mejor servir algo si lo tiene en lugar de mostrar un mensaje de error que no ofrece funcionalidad.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que si el procesador del editor tiene un tiempo de espera de socket o un mensaje de error 500, esta función no se almacenará en déclencheur. AEM Si no se puede acceder a la aplicación, esta función no hace nada
</div>

Esta configuración se puede establecer en cualquier conjunto de servidores, pero solo tiene sentido aplicarla en los archivos del conjunto de servidores de publicación. Este es un ejemplo de sintaxis de la función habilitada en un archivo de granja:

```
/cache { 
    /serveStaleOnError "1"
```

## Almacenamiento en caché de páginas con parámetros/argumentos de consulta

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Uno de los comportamientos normales del módulo de Dispatcher es que si una solicitud tiene un parámetro de consulta en el URI (normalmente se muestra como `/content/page.html?myquery=value`AEM ), omitirá el almacenamiento en caché del archivo e irá directamente a la instancia de. Está considerando esta solicitud como una página dinámica y no debería almacenarse en la caché. Esto puede causar efectos adversos en la eficacia de la caché.
</div>
<br/>

Ver esto [artículo](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) mostrar la importancia de los parámetros de consulta para el rendimiento del sitio.

De forma predeterminada, desea establecer la variable `ignoreUrlParams` reglas para permitir `*`.  Esto significa que todos los parámetros de consulta se ignoran y permiten que todas las páginas se almacenen en caché, independientemente de los parámetros utilizados.

A continuación se muestra un ejemplo en el que alguien ha creado un mecanismo de referencia de vínculos profundos de medios sociales que utiliza la referencia de argumento en la URI para saber de dónde procede la persona.

<b>Ejemplo ignorable:</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

La página se puede almacenar en caché al 100 %, pero no lo hace porque los argumentos están presentes. 
Configuración de su `ignoreUrlParams` as a lista de permitidos le ayudará a solucionar este problema:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Ahora, cuando Dispatcher vea la solicitud, ignorará el hecho de que la solicitud tiene el `query` parámetro de `?` hacer referencia a la página y seguir almacenándola en caché

<b>Ejemplo dinámico:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Tenga en cuenta que si tiene parámetros de consulta que realizan un cambio de página, se procesa el resultado, entonces deberá exonerarlos de la lista ignorada y hacer que la página no se pueda almacenar en caché de nuevo.  Por ejemplo, una página de búsqueda que utiliza un parámetro de consulta cambia el HTML sin procesar procesado.

Por lo tanto, esta es la fuente html de cada búsqueda:

`/search.html?q=fruit`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

`/search.html?q=vegetables`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

Si ha visitado `/search.html?q=fruit` primero, almacenaría en caché el html con los resultados mostrando fruta.

Luego visita `/search.html?q=vegetables` segundo, pero mostraría resultados de fruta.
Esto se debe al parámetro de consulta de `q` se ignora con respecto al almacenamiento en caché.  Para evitar este problema, tendrá que tomar nota de las páginas que representan diferentes HTML según los parámetros de la consulta y deniegan el almacenamiento en caché para ellos.

Ejemplo:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Las páginas que utilizan parámetros de consulta a través de Javascript seguirán funcionando completamente sin tener en cuenta los parámetros de esta configuración.  Porque no cambian el archivo html en reposo.  Utilizan JavaScript para actualizar los navegadores dom en tiempo real en el navegador local.  Esto significa que si consume los parámetros de consulta con javascript, es muy probable que pueda ignorar este parámetro para el almacenamiento en caché de la página.  Permita que esa página almacene en caché y disfrute de la ganancia de rendimiento.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Realizar un seguimiento de estas páginas requiere algún mantenimiento, pero vale la pena mejorar el rendimiento.  Puede pedir a su CSE que ejecute un informe sobre el tráfico de sus sitios web para proporcionarle una lista de todas las páginas que utilizan parámetros de consulta durante los últimos 90 días, y así poder analizar y asegurarse de que sabe qué páginas ver y qué parámetros de consulta no ignorar
</div>
<br/>

## Almacenamiento en caché de encabezados de respuesta

Es bastante obvio que Dispatcher almacena en caché `.html` páginas y clientlibs (es decir, `.js`, `.css`), pero ¿sabía que también puede almacenar en caché encabezados de respuesta particulares junto con el contenido en un archivo con el mismo nombre pero un `.h` extensión de archivo. Esto permite la siguiente respuesta no solo al contenido, sino a los encabezados de respuesta que deben ir con él desde la caché.

AEM Puede gestionar algo más que la codificación UTF-8

A veces, los elementos tienen encabezados especiales que ayudan a controlar los detalles de codificación del TTL de la caché y las marcas de tiempo de la última modificación.

Estos valores cuando se almacenan en caché se eliminan de forma predeterminada y el servidor web Apache httpd hará su propio trabajo de procesar el recurso con sus métodos normales de administración de archivos, que normalmente se limita a adivinar el tipo MIME en función de las extensiones de archivo.

Si tiene Dispatcher en la caché del recurso y los encabezados deseados, puede exponer la experiencia adecuada y garantizar que todos los detalles lleguen al explorador de los clientes.

Este es un ejemplo de una granja con los encabezados para almacenar en caché especificados:

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


AEM En el ejemplo, se han configurado las para servir encabezados que la CDN busca para saber cuándo invalidar su caché. AEM Lo que significa que ahora puede dictar correctamente qué archivos se invalidan en función de los encabezados.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que no puede utilizar expresiones regulares ni la coincidencia glob. Es una lista literal de los encabezados que se van a almacenar en caché. Coloque solo en una lista de los encabezados literales que desea que almacene en caché.
</div>


## Invalidar automáticamente el período de gracia

AEM En los sistemas que tienen mucha actividad de los autores que realizan muchas activaciones de la página, puede tener una condición de carrera en la que se producen invalidaciones repetidas. Las solicitudes de vaciado muy repetidas no son necesarias y se puede incorporar cierta tolerancia para no repetir un vaciado hasta que se haya borrado el período de gracia.

### Ejemplo de cómo funciona esto:

Si tiene 5 solicitudes para invalidar `/content/exampleco/en/` todas ocurren en un período de 3 segundos.

Con esta función desactivada, se invalidaría el directorio de caché `/content/exampleco/en/` 5 veces

Con esta función activada y configurada en 5 segundos, se invalidaría el directorio de caché `/content/exampleco/en/` <b>una vez</b>

Esta es una sintaxis de ejemplo de esta función que se está configurando para un período de gracia de 5 segundos:

```
/cache { 
    /gracePeriod "5"
```

## Invalidación basada en TTL

Una característica más reciente del módulo de Dispatcher fue `Time To Live (TTL)` opciones de invalidación basadas en para elementos en caché. Cuando un elemento se almacena en caché, busca la presencia de encabezados de control de caché y genera un archivo en el directorio de caché con el mismo nombre y un `.ttl` extensión.

Este es un ejemplo de la función que se está configurando en el archivo de configuración de la granja:

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
AEM Tenga en cuenta que aún debe configurarse el envío de encabezados TTL para que Dispatcher los respete. AEM Alternar esta función solo permite a Dispatcher saber cuándo quitar los archivos para los que tiene que enviar encabezados de control de caché. AEM Si no comienza a enviar encabezados TTL, Dispatcher no hará nada especial aquí.
</div>

## Reglas de filtro de caché

A continuación se muestra un ejemplo de una configuración de línea de base para qué elementos almacenar en caché en un editor:

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

Queremos que nuestro sitio publicado sea lo más voraz posible y almacenar todo en caché.

Si hay elementos que rompen la experiencia cuando se almacenan en caché, puede agregar reglas para eliminar la opción de almacenar en caché ese elemento. Como puede ver en el ejemplo anterior, los tokens csrf nunca deberían almacenarse en caché y se han excluido. Puede encontrar más información sobre cómo escribir estas reglas [aquí](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Siguiente -> Uso y comprensión de las variables](./variables.md)
