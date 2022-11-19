---
title: Dispatcher entiende el almacenamiento en caché
description: Comprenda cómo funciona el módulo de Dispatcher en su caché.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
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

Cuando cada solicitud atraviesa Dispatcher, las solicitudes siguen las reglas configuradas para mantener una versión en caché local para responder a los elementos elegibles

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Intencionalmente mantenemos la carga de trabajo publicada separada de la carga de trabajo del autor porque cuando Apache busca un archivo en DocumentRoot no sabe de qué instancia AEM vino. Por lo tanto, incluso si tiene la caché deshabilitada en la granja de autores, si el DocumentRoot del autor es el mismo que el del editor, servirá archivos de la caché cuando estén presentes. Lo que significa que servirá archivos de autor desde la caché publicada y será una experiencia de combinación realmente horrible para sus visitantes.

Mantener directorios DocumentRoot separados para diferentes contenidos publicados también es una mala idea. Tendrá que crear varios elementos en caché que no difieran entre sitios como clientlibs, así como tener que configurar un agente de vaciado de replicación para cada DocumentRoot que configure. Aumentar la cantidad de vaciado sobre el cabezal con cada activación de página. Confíe en el espacio de nombres de los archivos y sus rutas en caché completas y evite el uso de DocumentRoot múltiples para los sitios publicados.
</div>

## Archivos de configuración

Dispatcher controla lo que califica como caché en la variable `/cache {` de cualquier archivo de granja. 
En las granjas de configuración de línea de base de AMS, encontrará nuestras órdenes de inclusión como se muestra a continuación:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Al crear las reglas para lo que se debe almacenar en caché o no, consulte la documentación [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Creador de almacenamiento en caché

Hay muchas implementaciones que hemos visto en las que la gente no almacena en caché el contenido del autor. 
Están perdiendo una gran actualización en rendimiento y capacidad de respuesta para sus autores.

Hablemos de la estrategia tomada al configurar nuestra granja de autores para que almacene en caché correctamente.

Aquí hay un autor base `/cache {` de nuestro archivo de granja de autores:


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

Lo importante a tener en cuenta aquí es que la variable `/docroot` está configurado en el directorio de caché para el autor.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Asegúrese de que `DocumentRoot` en el informe `.vhost` coincide con las granjas `/docroot` parameter
</div>

La sentencia include de las reglas de caché incluye el archivo `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` que contiene estas reglas:

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

En un escenario de autor, el contenido cambia todo el tiempo y a propósito. Solo desea almacenar en caché los elementos que no van a cambiar con frecuencia.
Tenemos reglas para almacenar en caché `/libs` porque forman parte de la instalación de línea de base AEM y cambiarían hasta que haya instalado un Service Pack, un Cumulative Fix Pack, una actualización o una revisión. Así que el almacenamiento en caché de estos elementos tiene mucho sentido y realmente tiene grandes beneficios de la experiencia de autor de los usuarios finales que utilizan el sitio.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que estas reglas también almacenan en caché <b>`/apps`</b> aquí es donde se encuentra el código de aplicación personalizado. Si está desarrollando su código en esta instancia, resultará muy confuso al guardar el archivo y no verá si se refleja en la interfaz de usuario debido a que proporciona una copia en caché. La intención aquí es que si realiza una implementación del código en AEM, también sería poco frecuente y parte de los pasos de implementación debería ser borrar la caché del autor. Una vez más, el beneficio es enorme, ya que su código almacenable en caché se ejecuta más rápido para los usuarios finales.
</div>


## ServeOnStale (también conocido como Serve on Stale / SOS)

Esta es una de esas gemas de una función de Dispatcher. Si el editor está en carga o no responde, normalmente arrojará un código de respuesta http 502 o 503. Si esto sucede y esta función está habilitada, se pedirá a Dispatcher que siga sirviendo el contenido que todavía esté en la caché como mejor esfuerzo, incluso si no es una copia nueva. Es mejor mostrar algo si lo tiene en lugar de mostrar un mensaje de error que no ofrece ninguna funcionalidad.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que si el procesador del editor tiene un tiempo de espera de socket o un mensaje de error 500, esta función no se déclencheur. Si AEM es inaccesible, esta función no hace nada
</div>

Esta configuración se puede establecer en cualquier granja, pero solo tiene sentido aplicarla en los archivos de granja de publicación. Este es un ejemplo de sintaxis de la función habilitada en un archivo de granja:

```
/cache { 
    /serveStaleOnError "1"
```

## Almacenamiento en caché de páginas con parámetros/argumentos de consulta

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Uno de los comportamientos normales del módulo Dispatcher es que si una solicitud tiene un parámetro de consulta en el URI (normalmente se muestra como `/content/page.html?myquery=value`) omitirá el almacenamiento en caché del archivo e irá directamente a la instancia de AEM. Está considerando esta solicitud como una página dinámica y no debería almacenarse en caché. Esto puede tener efectos negativos en la eficacia de la caché.
</div>
<br/>

Consulte esta [article](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) mostrar la importancia que los parámetros de consulta pueden afectar al rendimiento del sitio.

De forma predeterminada, desea configurar la variable `ignoreUrlParams` reglas que permitan `*`.  Esto significa que todos los parámetros de consulta se ignoran y permiten que todas las páginas se almacenen en caché independientemente de los parámetros utilizados.

Aquí hay un ejemplo donde alguien ha creado un mecanismo de referencia de vínculo profundo de medios sociales que utiliza la referencia de argumento en la URI para saber de dónde viene la persona.

<b>Ejemplo ignorable:</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

La página se puede almacenar al 100% en caché, pero no en caché porque los argumentos están presentes. 
Configurar las `ignoreUrlParams` as a lista de permitidos ayudará a solucionar este problema:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Ahora, cuando Dispatcher vea la solicitud, ignorará el hecho de que la solicitud tiene el valor `query` parámetro de `?` referencia y almacenar en caché la página

<b>Ejemplo dinámico:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Tenga en cuenta que si tiene parámetros de consulta que hacen que una página cambie su salida representada, deberá eliminarlos de la lista ignorada y hacer que la página no se pueda almacenar en caché de nuevo.  Por ejemplo, una página de búsqueda que utiliza un parámetro de consulta cambia el html sin procesar procesado.

Esta es la fuente html de cada búsqueda:

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

Si ha visitado `/search.html?q=fruit` primero, almacenaría en caché el html con resultados que mostraran resultados.

Luego visita `/search.html?q=vegetables` en segundo lugar, pero mostraría resultados de fruta.
Esto se debe a que el parámetro de consulta de `q` se ignora con respecto al almacenamiento en caché.  Para evitar este problema, tendrá que tomar nota de las páginas que presentan diferentes HTML en función de los parámetros de consulta y denegar el almacenamiento en caché para ellos.

Ejemplo:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Las páginas que utilizan parámetros de consulta a través de JavaScript seguirán funcionando completamente e ignorando los parámetros de esta configuración.  Porque no cambian el archivo html en reposo.  Utilizan javascript para actualizar los navegadores en tiempo real en el navegador local.  Esto significa que si consume los parámetros de consulta con javascript, es muy probable que pueda ignorar este parámetro para el almacenamiento en caché de la página.  Permita que esa página se almacene en caché y disfrute de la mejora de rendimiento.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Realizar un seguimiento de estas páginas requiere cierta conservación, pero vale la pena mejorar el rendimiento.  Puede pedir a su CSE que ejecute un informe sobre el tráfico de sus sitios web para que le proporcione una lista de todas las páginas utilizando parámetros de consulta durante los últimos 90 días para que analice y asegúrese de saber qué páginas desea ver y qué parámetros de consulta no deben ignorar
</div>
<br/>

## Almacenamiento en caché de encabezados de respuesta

Es bastante obvio que Dispatcher almacena en caché `.html` páginas y clientlibs (p. ej. `.js`, `.css`), pero sabía que también puede almacenar en caché encabezados de respuesta específicos junto al contenido en un archivo con el mismo nombre pero un `.h` extensión de archivo. Esto permite que la siguiente respuesta no solo sea el contenido, sino también los encabezados de respuesta que deberían ir con él desde la caché.

AEM puede manejar algo más que la codificación UTF-8

A veces, los elementos tienen encabezados especiales que ayudan a controlar los detalles de codificación de TTL de la caché y las últimas marcas de tiempo modificadas.

Estos valores cuando se almacenan en caché se eliminan de forma predeterminada y el servidor web httpd de Apache hará su propio trabajo de procesar el recurso con sus métodos normales de gestión de archivos, que normalmente se limita a adivinar el tipo mime en función de las extensiones de archivo.

Si tiene la caché de Dispatcher del recurso y los encabezados deseados, puede exponer la experiencia adecuada y garantizar que todos los detalles lleguen al explorador del cliente.

A continuación, se muestra un ejemplo de una granja con los encabezados que se van a almacenar en caché especificados:

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


En el ejemplo, han configurado AEM para mostrar encabezados que la CDN busca para saber cuándo invalidar su caché. Esto significa que ahora AEM puede dictar correctamente qué archivos se invalidan en función de los encabezados.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que no puede utilizar expresiones regulares ni la coincidencia global. Es una lista literal de los encabezados que se van a almacenar en caché. Solo escriba una lista de los encabezados literales que desea que almacenen en caché.
</div>


## Invalidar período de gracia automáticamente

En AEM sistemas que tienen mucha actividad de autores que realizan muchas activaciones de página, puede tener una condición de carrera en la que se producen invalidaciones repetidas. Las solicitudes de vaciado muy repetidas no son necesarias y puede incorporar cierta tolerancia para no repetir una descarga hasta que el período de gracia se haya borrado.

### Ejemplo de cómo funciona esto:

Si tiene 5 solicitudes para invalidar `/content/exampleco/en/` todo ocurre en un periodo de 3 segundos.

Con esta función desactivada, invalidaría el directorio de caché `/content/exampleco/en/` 5 veces

Con esta función activada y configurada en 5 segundos, invalidaría el directorio de caché `/content/exampleco/en/` <b>once</b>

A continuación se muestra un ejemplo de sintaxis de esta función que se está configurando para un período de gracia de 5 segundos:

```
/cache { 
    /gracePeriod "5"
```

## Invalidación basada en TTL

Una característica más reciente del módulo Dispatcher era `Time To Live (TTL)` opciones de invalidación basadas en elementos en caché. Cuando un elemento se almacena en caché, busca la presencia de encabezados de control de caché y genera un archivo en el directorio de caché con el mismo nombre y un `.ttl` extensión.

Este es un ejemplo de la función que se está configurando en el archivo de configuración de la granja:

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Tenga en cuenta que aún AEM necesita configurarse para enviar encabezados TTL a Dispatcher para respetarlos. Alternar esta función solo permite que Dispatcher sepa cuándo eliminar los archivos para los que AEM ha enviado encabezados de control de caché. Si AEM no comienza a enviar encabezados TTL, Dispatcher no hará nada especial aquí.
</div>

## Reglas de filtro de caché

A continuación, se muestra un ejemplo de una configuración de línea de base para los elementos que se van a almacenar en caché en un publicador:

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

Queremos hacer que nuestro sitio publicado sea lo más ambicioso posible y almacenar en caché todo.

Si hay elementos que rompen la experiencia cuando se almacenan en caché, puede agregar reglas para eliminar la opción de almacenar ese elemento en caché. Como puede ver en el ejemplo anterior, los tokens csrf nunca deben almacenarse en caché y se han excluido. Encontrará más información sobre estas reglas [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Siguiente -> Uso y comprensión de las variables](./variables.md)