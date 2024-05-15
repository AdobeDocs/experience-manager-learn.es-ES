---
title: Explicación de los archivos de configuración de Dispatcher
description: Comprenda los archivos de configuración, las convenciones de nomenclatura y mucho más.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
duration: 379
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1688'
ht-degree: 0%

---

# Explicación de los archivos de configuración

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: diseño básico del archivo](./basic-file-layout.md)

Este documento desglosará y explicará cada uno de los archivos de configuración implementados en un servidor de Dispatcher creado de forma estándar y aprovisionado en Adobe Managed Services. Su uso, convención de nomenclatura, etc...

## Convención de nomenclatura

Al servidor web Apache no le importa realmente qué extensión de archivo es de un archivo al segmentarlo con un `Include` o `IncludeOptional` declaración.  Nombrarlos apropiadamente con nombres que eliminen conflictos y confusión ayuda a <b>tonelada</b>. Los nombres utilizados describirán el ámbito de aplicación del archivo, lo que facilita las cosas. Si todo se llama `.conf` esto se vuelve muy confuso. Queremos evitar archivos y extensiones con nombres incorrectos.  A continuación se muestra una lista de las diferentes extensiones de archivo personalizadas y convenciones de nomenclatura utilizadas en una instancia de Dispatcher configurada de AMS típica.

## Archivos incluidos en conf.d/

| Archivo | Destino del archivo | Descripción |
| ---- | ---------------- | ----------- |
| FILENAME`.conf` | `/etc/httpd/conf.d/` | Una instalación predeterminada de Enterprise Linux utiliza esta extensión de archivo e incluye la carpeta como un lugar para anular la configuración declarada en httpd.conf y permitirle añadir funcionalidad adicional a nivel global en Apache. |
| FILENAME`.vhost` | Ensayado: `/etc/httpd/conf.d/available_vhosts/`<br>Activo: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><b>Nota:</b> Los archivos .vhost no se copian en la carpeta enabled_vhosts sino que utilizan enlaces simbólicos a una ruta relativa al archivo available_vhosts/\*.vhost</u><br><br> | Los archivos \*.vhost (host virtual) son `<VirtualHosts>`  Entradas para que coincidan los nombres de host y permitir que Apache gestione cada tráfico de dominio con reglas diferentes. Desde el `.vhost` archivo, otros archivos como `rewrites`, `whitelisting`, `etc` se incluirán. |
| FILENAME`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` almacén de archivos `mod_rewrite` reglas que debe incluir y consumir explícitamente un `vhost` archivo |
| FILENAME`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` Los archivos de se incluyen desde dentro de `*.vhost` archivos. Contiene una expresión regular de IP o permite reglas de denegación para permitir la inclusión de direcciones IP en la lista blanca. Si intenta restringir la visualización de un host virtual en función de las direcciones IP, generará uno de estos archivos e lo incluirá en la `*.vhost` archivo |

## Archivos incluidos en conf.dispatcher.d/

| Archivo | Destino del archivo | Descripción |
| --- | --- | --- |
| FILENAME`.any` | `/etc/httpd/conf.dispatcher.d/` | AEM El módulo Apache de Dispatcher de origen de la configuración de `*.any` archivos. El archivo de inclusión principal predeterminado es `conf.dispatcher.d/dispatcher.any` |
| FILENAME`_farm.any` | Ensayado: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Activo: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><b>Nota:</b> estos archivos de granja de servidores no se copian en `enabled_farms` carpeta pero utilizar `symlinks` a una ruta relativa a `available_farms/*_farm.any` archivo <br/>`*_farm.any` Los archivos de se incluyen dentro de `conf.dispatcher.d/dispatcher.any` archivo. Estos archivos de granja principales existen para controlar el comportamiento del módulo para cada tipo de representación o sitio web. Los archivos se crean en `available_farms` y habilitado con un `symlink` en el `enabled_farms` directorio.  <br/>Los incluye automáticamente por nombre desde el `dispatcher.any` archivo.<br/><b>Línea base</b> los archivos de granja comienzan por `000_` para asegurarse de que se cargan primero.<br><b>Personalizado</b> los archivos de granja de servidores deben cargarse después de iniciar su esquema numérico en `100_` para garantizar el comportamiento adecuado de inclusión. |
| FILENAME`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` Los archivos de se incluyen desde dentro de `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Cada granja tiene un conjunto de reglas que cambian el tráfico que debe filtrarse y no llegar a los procesadores. |
| FILENAME`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` Los archivos de se incluyen desde dentro de `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Estos archivos son una lista de nombres de host o rutas uri que deben coincidir con la coincidencia de blob para determinar qué procesador utilizar para atender esa solicitud |
| FILENAME`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` Los archivos de se incluyen desde dentro de `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Estos archivos especifican qué elementos se almacenan en caché y cuáles no |
| FILENAME`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` Los archivos de se incluyen dentro de `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Especifican qué direcciones IP pueden enviar solicitudes de vaciado e invalidación. |
| FILENAME`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` Los archivos de se incluyen dentro de `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Especifican los encabezados de cliente que deben pasarse a cada procesador. |
| FILENAME`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` Los archivos de se incluyen dentro de `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Estos especifican la configuración de IP, puerto y tiempo de espera para cada procesador. AEM Un procesador adecuado puede ser un servidor de LiveCycle o cualquier sistema en el que la instancia de Dispatcher pueda recuperar o proxy las solicitudes de |

## Problemas evitados

Al seguir la convención de nombres, puede evitar algunos errores bastante fáciles de cometer que pueden tener resultados catastróficos.  Vamos a ver algunos ejemplos.

### Ejemplo de problema

Como ejemplo de sitio para ExampleCo, los desarrolladores de las configuraciones de Dispatcher crearon dos archivos de configuración.

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

Si la variable `vhost` el archivo se coloca accidentalmente en `rewrites` y la carpeta `rewrites file` se introduce en el `vhosts` carpeta.  Parece que se implementa por nombre de archivo correctamente, pero Apache generará un *ERROR* y el problema no será evidente de inmediato.

<b>Cómo esto suele convertirse en un problema</b>

Si la variable `two files` se descargan en el `same` la ubicación puede ser `overwrite themselves` o hacer que sea indistinguible, haciendo que el proceso de implementación sea una pesadilla.

<b>Las extensiones de archivo son las mismas y propensas a la inclusión automática</b>

Las extensiones de archivo son las mismas y utilizan la extensión incluida automáticamente que utilizará Apache `auto include` cualquiera `.conf` archivos en muchas de sus carpetas predeterminadas.

<b>Cómo esto suele convertirse en un problema</b>

Si el archivo vhost con la extensión de `.conf` se pone en el `/etc/httpd/conf.d/` intentará cargarlo en la memoria en Apache, lo que suele ser correcto, pero si el archivo de reglas de reescritura con la extensión de `.conf` se coloca en la `/etc/httpd/conf.d/` , se incluirá automáticamente y se aplicará globalmente, lo que causará resultados confusos y no deseados.

## Resolución

Asigne a los archivos un nombre basado en lo que hacen y de forma segura fuera del área de nombres de reglas de inclusión automática.

Si se trata de un archivo host virtual, póngale nombre con `.vhost` como extensión.

Si se trata de un archivo de reglas de reescritura, asígnele el nombre sitio`_rewrite.rules` como el sufijo y la extensión. Esta convención de nombres aclarará para qué sitio es y que es un conjunto de reglas de reescritura.

Si se trata de un archivo de regla de lista blanca de IP, asígnele un nombre descripción`_whitelist.rules` como el sufijo y la extensión. Esta convención de nombres le dará una descripción de para qué sirve y de que es un conjunto de reglas coincidentes de IP.

El uso de estas convenciones de nomenclatura evitará problemas si un archivo se mueve a un directorio de inclusión automática al que no pertenece.

Por ejemplo, colocar un archivo denominado con `.rules`, `.any`, o `.vhost` en la carpeta de inclusión automática de `/etc/httpd/conf.d/` no tendría ningún efecto.

Si una solicitud de cambio de implementación indica &quot;implemente example_rewrite.rules en Dispatchers de producción&quot;, la persona que implementa los cambios ya puede saber que no está agregando un nuevo sitio, solo está actualizando las reglas de reescritura tal como indica el nombre de archivo.

### Incluir pedido

Al ampliar la funcionalidad y las configuraciones en el servidor web Apache instalado en Enterprise Linux, tiene algunos pedidos de inclusión importantes que querrá comprender

### La Línea Base Apache Incluye

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Como se ve en el diagrama anterior, el binario httpd solo busca el archivo httpd.conf como su archivo de configuración.  Ese archivo contiene las siguientes instrucciones:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### El nivel superior de AMS incluye

Al aplicar nuestro estándar, añadimos algunos tipos de archivo adicionales e incluimos los nuestros.

Estos son los directorios de línea de base de AMS y las inclusiones de nivel superior
![Las órdenes de inclusión de línea de base de AMS Baseline comienzan con un dispatcher_vhost.conf que incluirá cualquier archivo con el *.vhost del directorio /etc/httpd/conf.d/enabled_vhosts/.  Los elementos del directorio /etc/httpd/conf.d/enabled_vhosts/ son enlaces simbólicos al archivo de configuración real que se encuentra en /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Basándonos en la línea de base de Apache, mostramos cómo AMS creó algunas carpetas adicionales e inclusiones de nivel superior para `conf.d` carpetas, así como directorios específicos de módulo anidados en `/etc/httpd/conf.dispatcher.d/`

Cuando Apache se cargue, se abrirá el `/etc/httpd/conf.modules.d/02-dispatcher.conf` y ese archivo incluirá el archivo binario `/etc/httpd/modules/mod_dispatcher.so` en su estado de funcionamiento.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Para utilizar el módulo en nuestro `<VirtualHost />` soltamos un archivo de configuración en `/etc/httpd/conf.d/` nombrado `dispatcher_vhost.conf` y dentro de este archivo verá el uso de configurar los parámetros básicos necesarios para que funcione el módulo:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Como puede ver arriba, esto incluye el nivel superior `dispatcher.any` para que nuestro módulo de Dispatcher recoja sus archivos de configuración de `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Preste atención al contenido de este archivo:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

El nivel superior `dispatcher.any` incluye todos los archivos de granja habilitados que residen en `/etc/httpd/conf.dispatcher.d/enabled_farms/` con el nombre de archivo `FILENAME_farm.any` que sigue nuestra convención de nomenclatura estándar.

Más adelante, en `dispatcher_vhost.conf` archivo mencionado anteriormente también hacemos una declaración de inclusión para habilitar cada archivo host virtual habilitado que se encuentre en `/etc/httpd/conf.d/enabled_vhosts/` con el nombre de archivo de `FILENAME.vhost` que sigue nuestra convención de nomenclatura estándar.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

En cada uno de nuestros archivos .vhost verá que el módulo Dispatcher se inicializa como controlador de archivo predeterminado para un directorio.  Este es un ejemplo de archivo .vhost para mostrar la sintaxis:

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

Después de que el nivel superior incluye la resolución, tienen otras sub-inclusiones que vale la pena mencionar.  Este es un diagrama de alto nivel sobre cómo los archivos farms y vhosts incluyen otros subelementos

### El host virtual de AMS incluye

![Esta imagen muestra cómo un archivo .vhost incluye archivos de variables, listas blancas y carpetas de reescritura](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Si existe `.vhost` archivos de `/etc/httpd/conf.d/availabled_vhosts/` directorio obtener enlace simbólico en el `/etc/httpd/conf.d/enabled_vhosts/` se utilizarán en la configuración en ejecución.

El `.vhost` Los archivos de tienen subinclusiones basadas en elementos comunes que hemos encontrado.  Cosas como variables, listas blancas y reglas de reescritura.

El `.vhost` tendrá instrucciones de inclusión para cada archivo en función de dónde deban incluirse en el `.vhost` archivo.  Este es un ejemplo de sintaxis de un `.vhost` como buena referencia:

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

Como puede ver en el ejemplo anterior, hay una inclusión para las variables necesarias en este archivo de configuración que se utilizan más adelante.

Dentro del archivo `/etc/httpd/conf.d/variables/weretail.vars` podemos ver qué variables están definidas:

```
Define MAIN_DOMAIN dev.weretail.com
```

También puede ver una línea que incluye una lista de `_whitelist.rules` archivos que limitan quién puede ver este contenido en función de diferentes criterios de la lista blanca.  Veamos el contenido de uno de los archivos de la lista blanca `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

También puede ver una línea que incluye un conjunto de reglas de reescritura.  Vamos a echar un vistazo al contenido de la `weretail_rewrite.rules` archivo:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### La granja de AMS incluye

![<FILENAME>_farms.any incluirá los archivos sub.any para completar la configuración de una granja.  En esta imagen puede ver que una granja incluirá cada caché de archivos de sección de nivel superior, encabezados de cliente, filtros, procesamientos y archivos vhosts .any](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Cuando cualquier archivo FILENAME_farm.any de `/etc/httpd/conf.dispatcher.d/available_farms/` directorio obtener enlace simbólico en el `/etc/httpd/conf.dispatcher.d/enabled_farms/` se utilizarán en la configuración en ejecución.

Los archivos de granja tienen subinclusiones basadas en [secciones de nivel superior de la granja](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) como cache, clientheaders, filters, renders y vhosts.

El `FILENAME_farm.any` Los archivos tendrán instrucciones de inclusión para cada archivo en función de dónde deban incluirse en el archivo de granja.  Este es un ejemplo de sintaxis de un `FILENAME_farm.any` como buena referencia:

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

Como puede ver en cada sección de la granja de servidores de correo electrónico, en lugar de tener toda la sintaxis necesaria, se utiliza una instrucción de inclusión.

Veamos la sintaxis de algunas de estas inclusiones para tener la idea de cómo se vería cada subcomponente

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Como puede ver, es una nueva lista separada por líneas de nombres de dominio que deben procesarse desde esta granja sobre los demás.

A continuación, veamos el `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Siguiente -> Explicación de la caché](./understanding-cache.md)
