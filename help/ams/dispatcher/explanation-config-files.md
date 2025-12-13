---
title: Explicación de los archivos de configuración de Dispatcher
description: Comprenda los archivos de configuración, las convenciones de nomenclatura y mucho más.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
duration: 379
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1694'
ht-degree: 0%

---

# Explicación de los archivos de configuración

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: diseño básico del archivo](./basic-file-layout.md)

Este documento desglosa y explica cada uno de los archivos de configuración implementados en un servidor de Dispatcher creado de forma estándar y aprovisionado en Adobe Managed Services. Su uso, convención de nomenclatura, etc...

## Convención de nomenclatura

Al servidor web Apache no le importa realmente la extensión de archivo de un archivo cuando se le dirige con una instrucción `Include` o `IncludeOptional`.  Nombrarlos apropiadamente con nombres que eliminen conflictos y confusión ayudará a <b>ton</b>. Los nombres utilizados describirán el ámbito de aplicación del archivo, lo que facilita las cosas. Si todo se llama `.conf`, esto puede resultar muy confuso. Queremos evitar archivos y extensiones con nombres incorrectos.  A continuación se muestra una lista de las diferentes extensiones de archivo personalizadas y convenciones de nomenclatura utilizadas en una configuración de Dispatcher típica de AMS.

## Archivos incluidos en conf.d/

| Archivo | Destino del archivo | Descripción |
| ---- | ---------------- | ----------- |
| NOMBRE DE ARCHIVO`.conf` | `/etc/httpd/conf.d/` | Una instalación predeterminada de Enterprise Linux utiliza esta extensión de archivo e incluye la carpeta como un lugar para anular la configuración declarada en httpd.conf y permitirle añadir funcionalidad adicional a nivel global en Apache. |
| NOMBRE DE ARCHIVO`.vhost` | En pruebas: `/etc/httpd/conf.d/available_vhosts/`<br>Activo: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><b>Nota:</b> Los archivos .vhost no se copiarán en la carpeta enabled_vhosts, sino que usarán enlaces simbólicos a una ruta relativa al archivo available_vhosts/\*.vhost</u><br><br> | Los archivos \*.vhost (host virtual) son `<VirtualHosts>`  Entradas para que coincidan los nombres de host y permitir que Apache gestione cada tráfico de dominio con reglas diferentes. Del archivo `.vhost` se incluirán otros archivos como `rewrites`, `whitelisting`, `etc`. |
| NOMBRE DE ARCHIVO`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` archivos almacenan `mod_rewrite` reglas que un archivo `vhost` debe incluir y consumir explícitamente |
| NOMBRE DE ARCHIVO`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` archivos se incluyen desde dentro de los `*.vhost` archivos. Contiene una expresión regular de IP o permite reglas de denegación para permitir la inclusión de direcciones IP en la lista blanca. Si intenta restringir la visualización de un host virtual en función de las direcciones IP, generará uno de estos archivos e lo incluirá en el archivo `*.vhost` |

## Archivos incluidos en conf.dispatcher.d/

| Archivo | Destino del archivo | Descripción |
| --- | --- | --- |
| NOMBRE DE ARCHIVO`.any` | `/etc/httpd/conf.dispatcher.d/` | El módulo AEM Dispatcher Apache obtiene su configuración a partir de `*.any` archivos. El archivo de inclusión principal predeterminado es `conf.dispatcher.d/dispatcher.any` |
| NOMBRE DE ARCHIVO`_farm.any` | Ensayado: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Activo: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><b>Nota:</b> estos archivos de granja no se van a copiar en la carpeta `enabled_farms`, pero usan `symlinks` para establecer una ruta relativa al archivo `available_farms/*_farm.any`. Dentro del archivo <br/>`*_farm.any` se incluyen `conf.dispatcher.d/dispatcher.any` archivos. Estos archivos de granja principales existen para controlar el comportamiento del módulo para cada tipo de representación o sitio web. Los archivos se crean en el directorio `available_farms` y se habilitan con un `symlink` en el directorio `enabled_farms`.  <br/>Los incluye automáticamente por nombre del archivo `dispatcher.any`.<br/><b>Los archivos de granja de servidores de línea de base</b> comienzan por `000_` para asegurarse de que se cargan primero.Los archivos de granja <br><b>Custom</b> deben cargarse después de iniciar su esquema numérico en `100_` para garantizar el comportamiento correcto de inclusión. | |
| NOMBRE DE ARCHIVO`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` archivos se incluyen desde dentro de los `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Cada granja tiene un conjunto de reglas que cambian el tráfico que debe filtrarse y no llegar a los procesadores. |
| NOMBRE DE ARCHIVO`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` archivos se incluyen desde dentro de los `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Estos archivos son una lista de nombres de host o rutas uri que deben coincidir con la coincidencia de blob para determinar qué procesador utilizar para atender esa solicitud |
| NOMBRE DE ARCHIVO`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` archivos se incluyen desde dentro de los `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Estos archivos especifican qué elementos se almacenan en caché y cuáles no |
| NOMBRE DE ARCHIVO`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` archivos se incluyen dentro de los `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Especifican qué direcciones IP pueden enviar solicitudes de vaciado e invalidación. |
| NOMBRE DE ARCHIVO`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` archivos se incluyen dentro de los `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Especifican los encabezados de cliente que deben pasarse a cada procesador. |
| NOMBRE DE ARCHIVO`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` archivos se incluyen dentro de los `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Estos especifican la configuración de IP, puerto y tiempo de espera para cada procesador. Un procesador adecuado puede ser un servidor de LiveCycle o cualquier sistema AEM desde el que Dispatcher pueda recuperar o proxy las solicitudes |

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

Si el archivo `vhost` se coloca accidentalmente en la carpeta `rewrites` y `rewrites file` se coloca en la carpeta `vhosts`.  Parece que el nombre de archivo lo ha implementado correctamente, pero Apache generará *ERROR* y el problema no se mostrará inmediatamente.

<b>Cómo se convierte esto en un problema</b>

Si se descargan los `two files` en la ubicación `same`, pueden `overwrite themselves` o no distinguirlos, lo que convierte el proceso de implementación en una pesadilla.

<b>Las extensiones de archivo son las mismas y propensas a incluirse automáticamente</b>

Las extensiones de archivo son las mismas y usan la extensión incluida automáticamente que Apache `auto include` cualquier `.conf` archivo en muchas de sus carpetas predeterminadas.

<b>Cómo se convierte esto en un problema</b>

Si el archivo vhost con la extensión `.conf` se coloca en la carpeta `/etc/httpd/conf.d/`, intentará cargarlo en la memoria en Apache, lo que suele ser correcto, pero si el archivo de reglas de reescritura con la extensión `.conf` se coloca en la carpeta `/etc/httpd/conf.d/`, se incluirá automáticamente y se aplicará globalmente, lo que causará resultados confusos y no deseados.

## Resolución

Asigne a los archivos un nombre basado en lo que hacen y de forma segura fuera del área de nombres de reglas de inclusión automática.

Si se trata de un archivo host virtual, póngale el nombre con `.vhost` como extensión.

Si se trata de un archivo de reglas de reescritura, póngale nombre con el sitio `_rewrite.rules` como sufijo y extensión. Esta convención de nombres aclarará para qué sitio es y que es un conjunto de reglas de reescritura.

Si se trata de un archivo de regla de lista blanca de IP, póngale nombre a description`_whitelist.rules` como sufijo y extensión. Esta convención de nombres le dará una descripción de para qué sirve y de que es un conjunto de reglas coincidentes de IP.

El uso de estas convenciones de nomenclatura evitará problemas si un archivo se mueve a un directorio de inclusión automática al que no pertenece.

Por ejemplo, poner un archivo con el nombre `.rules`, `.any` o `.vhost` en la carpeta de inclusión automática de `/etc/httpd/conf.d/` no tendría ningún efecto.

Si una solicitud de cambio de implementación indica &quot;implemente example_rewrite.rules en Dispatchers de producción&quot;, la persona que implementa los cambios ya puede saber que no está agregando un nuevo sitio, solo está actualizando las reglas de reescritura tal como indica el nombre de archivo.

### Incluir pedido

Al ampliar la funcionalidad y las configuraciones en el servidor web Apache instalado en Enterprise Linux, tiene algunos pedidos de inclusión importantes que querrá comprender

### La Línea Base Apache Incluye

![La línea de base del servidor web Apache HTTPD incluye](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Como se ve en el diagrama anterior, el binario httpd solo busca el archivo httpd.conf como su archivo de configuración.  Ese archivo contiene las siguientes instrucciones:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### El nivel superior de AMS incluye

Al aplicar nuestro estándar, añadimos algunos tipos de archivo adicionales e incluimos los nuestros.

Estos son los directorios de línea de base de AMS y las inclusiones de nivel superior
![Las órdenes de inclusión de línea de base de AMS Baseline comienzan con un dispatcher_vhost.conf que incluirá cualquier archivo con el *.vhost del directorio /etc/httpd/conf.d/enabled_vhosts/.  Los elementos del directorio /etc/httpd/conf.d/enabled_vhosts/ son enlaces simbólicos al archivo de configuración real que se encuentra en /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Basándonos en la línea de base de Apache, mostramos cómo AMS creó algunas carpetas adicionales e inclusiones de nivel superior para `conf.d` carpetas, así como directorios específicos de módulos anidados en `/etc/httpd/conf.dispatcher.d/`

Cuando Apache se cargue, se abrirá `/etc/httpd/conf.modules.d/02-dispatcher.conf` y ese archivo incluirá el archivo binario `/etc/httpd/modules/mod_dispatcher.so` en su estado de ejecución.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Para usar el módulo en nuestro `<VirtualHost />`, soltamos un archivo de configuración en `/etc/httpd/conf.d/` llamado `dispatcher_vhost.conf` y dentro de este archivo verá el uso de setup para que el módulo funcione:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Como puede ver arriba, esto incluye el archivo de nivel superior `dispatcher.any` para que nuestro módulo de Dispatcher recoja sus archivos de configuración de `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Preste atención al contenido de este archivo:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

El archivo de nivel superior `dispatcher.any` incluye todos los archivos de granja habilitados que se encuentran en `/etc/httpd/conf.dispatcher.d/enabled_farms/` con el nombre de archivo de `FILENAME_farm.any`, que sigue nuestra convención de nomenclatura estándar.

Más adelante en el archivo `dispatcher_vhost.conf` mencionado anteriormente también se hace una instrucción de inclusión para habilitar cada archivo host virtual habilitado que se encuentre en `/etc/httpd/conf.d/enabled_vhosts/` con el nombre de archivo de `FILENAME.vhost`, que sigue nuestra convención de nomenclatura estándar.

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

![Esta imagen muestra cómo un archivo .vhost incluye archivos de variables, listas blancas y reescribe carpetas](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Cuando cualquier archivo `.vhost` del directorio `/etc/httpd/conf.d/availabled_vhosts/` se enlace simbólicamente al directorio `/etc/httpd/conf.d/enabled_vhosts/`, se utilizará en la configuración en ejecución.

Los archivos de `.vhost` tienen subinclusiones basadas en elementos comunes que hemos encontrado.  Cosas como variables, listas blancas y reglas de reescritura.

El archivo `.vhost` tendrá instrucciones de inclusión para cada archivo en función de dónde se deban incluir en el archivo `.vhost`.  Esta es una sintaxis de ejemplo de un archivo `.vhost` como buena referencia:

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

Dentro del archivo `/etc/httpd/conf.d/variables/weretail.vars` podemos ver qué variables están definidas:

```
Define MAIN_DOMAIN dev.weretail.com
```

También puede ver una línea que incluye una lista de `_whitelist.rules` archivos que restringen quién puede ver este contenido en función de diferentes criterios de la lista blanca.  Veamos el contenido de uno de los archivos de la lista blanca `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

También puede ver una línea que incluye un conjunto de reglas de reescritura.  Vamos a echar un vistazo al contenido del archivo `weretail_rewrite.rules`:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### La granja de AMS incluye

![<FILENAME>_farms.any incluirá los archivos sub.any para completar la configuración de una granja.  En esta imagen puede ver que una granja incluirá cada caché de archivos de sección de nivel superior, encabezados de cliente, filtros, procesamientos y vhosts .any archivos](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Cuando cualquier archivo FILENAME_farm.any del directorio `/etc/httpd/conf.dispatcher.d/available_farms/` se enlace simbólicamente al directorio `/etc/httpd/conf.dispatcher.d/enabled_farms/`, se utilizará en la configuración en ejecución.

Los archivos de granja tienen subinclusiones basadas en [secciones de nivel superior de la granja](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms), como caché, clientheaders, filtros, procesamientos y vhosts.

Los `FILENAME_farm.any` archivos tendrán instrucciones de inclusión para cada archivo en función de dónde deban incluirse en el archivo de granja.  Esta es una sintaxis de ejemplo de un archivo `FILENAME_farm.any` como buena referencia:

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

A continuación, veamos `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Siguiente -> Explicación de la caché](./understanding-cache.md)
