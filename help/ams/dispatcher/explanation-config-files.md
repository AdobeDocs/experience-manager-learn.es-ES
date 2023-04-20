---
title: Explicación de los archivos de configuración de Dispatcher
description: Comprender los archivos de configuración, las convenciones de nomenclatura y mucho más.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: cc085af90b9b8ea0e650546c251fbf14cc222989
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---


# Explicación de los archivos de configuración

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Diseño de archivo básico](./basic-file-layout.md)

Este documento desglosará y explicará cada uno de los archivos de configuración implementados en un servidor de Dispatcher creado estándar aprovisionado en Adobe Managed Services. Su uso, convención de nombres, etc...

## Convención de nomenclatura

El servidor web Apache no se preocupa realmente de qué es la extensión de archivo de un archivo cuando se segmenta con un `Include` o `IncludeOptional` instrucción.  Nombrarlos apropiadamente con nombres que eliminen conflictos y confusión ayuda a <b>ton</b>. Los nombres utilizados describirán el ámbito en el que se aplica el archivo, lo que facilita las cosas. Si se nombra todo `.conf` esto se vuelve realmente confuso. Queremos evitar los archivos y las extensiones con nombres incorrectos.  A continuación se muestra una lista de las diferentes extensiones de archivo personalizadas y convenciones de nomenclatura utilizadas en un Dispatcher configurado con AMS típico.

## Archivos contenidos en conf.d/

| Archivo | Destino del archivo | Descripción |
| ---- | ---------------- | ----------- |
| NOMBRE DE ARCHIVO`.conf` | `/etc/httpd/conf.d/` | Una instalación predeterminada de Enterprise Linux utiliza esta extensión de archivo e incluye una carpeta como lugar para anular la configuración declarada en httpd.conf y permitirle agregar funcionalidad adicional a nivel global en Apache. |
| NOMBRE DE ARCHIVO`.vhost` | Ensayo: `/etc/httpd/conf.d/available_vhosts/`<br>Activo: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b> Los archivos .vhost no deben copiarse en la carpeta enabled_vhosts sino que deben utilizarse enlaces simbólicos a una ruta relativa al archivo available_vhosts/\*.vhost</div></u><br><br> | Los archivos \*.vhost (host virtual) son `<VirtualHosts>`  para que coincidan con los nombres de host y permitir que Apache gestione cada tráfico de dominio con reglas diferentes. En el `.vhost` archivo, otros archivos como `rewrites`, `whitelisting`, `etc` se incluirá. |
| NOMBRE DE ARCHIVO`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` almacén de archivos `mod_rewrite` reglas que se incluirán y consumirán explícitamente mediante `vhost` file |
| NOMBRE DE ARCHIVO`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` los archivos se incluyen desde el interior de la variable `*.vhost` archivos. Contiene direcciones IP regex o permite reglas de denegación para permitir listas blancas de IP. Si está intentando restringir la visualización de un host virtual basado en direcciones IP, generará uno de estos archivos y lo incluirá desde su `*.vhost` file |

## Archivos contenidos en conf.dispatcher.d/

| Archivo | Destino del archivo | Descripción |
| --- | --- | --- |
| NOMBRE DE ARCHIVO`.any` | `/etc/httpd/conf.dispatcher.d/` | El módulo Apache de Dispatcher de AEM obtiene su configuración de `*.any` archivos. El archivo de inclusión principal predeterminado es `conf.dispatcher.d/dispatcher.any` |
| NOMBRE DE ARCHIVO`_farm.any` | Ensayo: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Activo: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b> estos archivos de granja no deben copiarse en la `enabled_farms` carpeta pero utilice `symlinks` a una ruta relativa a la variable `available_farms/*_farm.any` file </div> <br/>`*_farm.any` los archivos se incluyen dentro de la variable `conf.dispatcher.d/dispatcher.any` archivo. Estos archivos de granja principales existen para controlar el comportamiento del módulo para cada tipo de renderizado o sitio web. Los archivos se crean en la variable `available_farms` directorio y habilitado con un `symlink` en el `enabled_farms` directorio.  <br/>Los incluye automáticamente por nombre desde el `dispatcher.any` archivo.<br/><b>Línea de base</b> los archivos de granja comienzan por `000_` para asegurarse de que se cargan primero.<br><b>Personalizado</b> los archivos de granja deben cargarse después de iniciar su esquema de números en `100_` para garantizar el comportamiento de inclusión adecuado. |
| NOMBRE DE ARCHIVO`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` los archivos se incluyen desde el interior de la variable `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Cada granja tiene un conjunto de reglas que cambian qué tráfico debe filtrarse y no llegar a los procesadores. |
| NOMBRE DE ARCHIVO`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` los archivos se incluyen desde el interior de la variable `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Estos archivos son una lista de nombres de host o rutas uri a las que se debe hacer coincidir con la coincidencia de blob para determinar qué procesador utilizar para atender esa solicitud |
| NOMBRE DE ARCHIVO`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` los archivos se incluyen desde el interior de la variable `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Estos archivos especifican qué elementos se almacenan en caché y cuáles no |
| NOMBRE DE ARCHIVO`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` los archivos se incluyen dentro de la variable `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Indican qué direcciones IP pueden enviar solicitudes de vaciado e invalidación. |
| NOMBRE DE ARCHIVO`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` los archivos se incluyen dentro de la variable `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Indican qué encabezados de cliente deben pasarse a través de cada renderizador. |
| NOMBRE DE ARCHIVO`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` los archivos se incluyen dentro de la variable `conf.dispatcher.d/enabled_farms/*_farm.any` archivos. Especifican la configuración de IP, puerto y tiempo de espera para cada renderizador. Un renderizador adecuado puede ser un servidor de ciclo de vida o cualquier sistema de AEM desde el que Dispatcher pueda recuperar o proxy las solicitudes |

## Problemas evitados

Al seguir la convención de nomenclatura puede evitar errores bastante fáciles de cometer que pueden tener resultados catastróficos.  Explicaremos algunos ejemplos.

### Ejemplo de problema

Como sitio Ejemplo para ExampleCo, los desarrolladores de las configuraciones de Dispatcher crearon dos archivos de configuración.

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

Si la variable `vhost` se coloca accidentalmente en el `rewrites` y `rewrites file` se introduce en la variable `vhosts` carpeta.  Parece que se implementa correctamente por nombre de archivo, pero Apache lanzará un *ERROR* y el problema no será evidente de inmediato.

<b>Generalmente, se convierte en un problema</b>

Si la variable `two files` se descargan en el `same` ubicación que pueden `overwrite themselves` o hacer que sea indistinguible hacer que el proceso de implementación sea una pesadilla.

<b>Las extensiones de archivo son las mismas y son propensas a la autoinclusión</b>

Las extensiones de archivo son las mismas y utiliza la extensión autoincluida que Apache `auto include` any `.conf` en muchas de sus carpetas predeterminadas.

<b>Generalmente, se convierte en un problema</b>

Si el archivo vhost con la extensión de `.conf` se coloca en la variable `/etc/httpd/conf.d/` carpeta intentará cargarla en la memoria en Apache, que suele estar bien, pero si el archivo de reglas de reescritura con la extensión de `.conf` se coloca en la variable `/etc/httpd/conf.d/` , se incluye automáticamente y se aplica globalmente, lo que provoca resultados confusos y no deseados.

## Resolución

Asigne un nombre a los archivos en función de lo que hagan y de forma segura fuera del espacio de nombres de reglas de inclusión automática.

Si es un nombre de archivo host virtual, `.vhost` como extensión.

Si es un archivo de reglas de reescritura, asígnele un nombre al sitio`_rewrite.rules` como sufijo y extensión. Esta convención de nombres dejará claro para qué sitio está y que es un conjunto de reglas de reescritura.

Si es un archivo de regla de lista blanca de IP, asígnele un nombre a la descripción`_whitelist.rules` como sufijo y extensión. Esta convención de nomenclatura le dará una descripción de para qué sirve y de que es un conjunto de reglas de coincidencia de IP.

El uso de estas convenciones de nomenclatura evitará problemas si un archivo se mueve a un directorio de inclusión automática al que no pertenece.

Por ejemplo, colocar un archivo con el nombre `.rules`, `.any`o `.vhost` en la carpeta de inclusión automática de `/etc/httpd/conf.d/` no tendría ningún efecto.

Si una solicitud de cambio de implementación indica &quot;implemente exampleco_rewrite.rules en los distribuidores de producción&quot;, la persona que implementa los cambios ya puede saber que no están agregando un nuevo sitio, solo están actualizando las reglas de reescritura como se indica con el nombre del archivo.

### Incluir orden

Al ampliar la funcionalidad y las configuraciones en Apache Webserver instalado en Enterprise Linux, tiene algunos pedidos de inclusión importantes que desea comprender

### Órdenes de inclusión de línea de base de Apache

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Como se ve en el diagrama de arriba, el binario httpd solo mira al archivo httpd.conf como su archivo de configuración.  Ese archivo contiene las siguientes instrucciones:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Incluye el nivel superior de AMS

Cuando aplicamos nuestro estándar, agregamos algunos tipos de archivos adicionales e incluimos los nuestros.

Aquí están los directorios de línea de base de AMS y el nivel superior incluye
![La línea de base de AMS incluye el inicio con un dispatcher_vhost.conf que incluirá cualquier archivo con el *.vhost del directorio /etc/httpd/conf.d/enabled_vhosts/.  Los elementos del directorio /etc/httpd/conf.d/enabled_vhosts/ son enlaces simbólicos al archivo de configuración real que se encuentra en /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Basándonos en la base de Apache mostramos cómo AMS creó algunas carpetas adicionales y el nivel superior incluye para `conf.d` carpetas, así como directorios específicos de módulos anidados en `/etc/httpd/conf.dispatcher.d/`

Cuando Apache cargue, se extraerá de la `/etc/httpd/conf.modules.d/02-dispatcher.conf` y ese archivo incluirá el archivo binario `/etc/httpd/modules/mod_dispatcher.so` en su estado de ejecución.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Para usar el módulo en nuestra `<VirtualHost />` soltamos un archivo de configuración en `/etc/httpd/conf.d/` named `dispatcher_vhost.conf` y dentro de este archivo verá use setup los parámetros básicos necesarios para que funcione el módulo:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Como puede ver arriba, esto incluye el nivel superior `dispatcher.any` para que nuestro módulo Dispatcher recoja sus archivos de configuración de `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Preste atención al contenido de este archivo:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

El nivel superior `dispatcher.any` incluye todos los archivos de granja habilitados que residen en `/etc/httpd/conf.dispatcher.d/enabled_farms/` con el nombre de archivo de `FILENAME_farm.any` que sigue nuestra convención de nomenclatura estándar.

Más adelante, en la sección `dispatcher_vhost.conf` archivo mencionado anteriormente también hacemos una declaración include para habilitar cada uno de los archivos host virtuales habilitados que residen en `/etc/httpd/conf.d/enabled_vhosts/` con el nombre de archivo de `FILENAME.vhost` que sigue nuestra convención de nomenclatura estándar.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

En cada uno de nuestros archivos .vhost notará que el módulo Dispatcher se inicializa como un controlador de archivos predeterminado para un directorio.  A continuación se muestra un ejemplo de archivo .vhost para mostrar la sintaxis:

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

Después de que el nivel superior incluye la resolución, tienen otras subinclusiones que vale la pena mencionar.  Aquí hay un diagrama de alto nivel sobre cómo los archivos de granjas y vhosts incluyen otros subelementos

### Incluye host virtual de AMS

![Esta imagen muestra cómo un archivo .vhost incluye archivos de variables, listas blancas y carpetas de reescritura](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Cuando `.vhost` archivos de `/etc/httpd/conf.d/availabled_vhosts/` el directorio se enlaza simbólicamente a `/etc/httpd/conf.d/enabled_vhosts/` se utilizarán en la configuración en ejecución.

La variable `.vhost` los archivos tienen subinclusiones basadas en piezas comunes que hemos encontrado.  Cosas como variables, listas blancas y reglas de reescritura.

La variable `.vhost` tendrá instrucciones include para cada archivo en función de dónde deban incluirse en la variable `.vhost` archivo.  A continuación se muestra un ejemplo de sintaxis de un `.vhost` como buena referencia:

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

También puede ver una línea que incluye una lista de `_whitelist.rules` archivos que restringen quién puede ver este contenido en función de diferentes criterios de listas blancas.  Veamos el contenido de uno de los archivos de la lista blanca `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

También puede ver una línea que incluye un conjunto de reglas de reescritura.  Veamos el contenido del `weretail_rewrite.rules` archivo:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### Inclusiones de granja de AMS

![<FILENAME>_farms.any incluirá archivos sub.any para completar una configuración de granja.  En esta imagen puede ver que una granja incluirá cada caché de archivos de sección de nivel superior, encabezados de clientes, filtros, renderizadores y archivos vhosts.any](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Cuando cualquier archivo FILENAME_farm.any `/etc/httpd/conf.dispatcher.d/available_farms/` el directorio se enlaza simbólicamente a `/etc/httpd/conf.dispatcher.d/enabled_farms/` se utilizarán en la configuración en ejecución.

Los archivos de granja tienen subinclusiones basadas en [secciones de nivel superior de la granja](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) como caché, encabezados de clientes, filtros, renderizadores y vhosts.

La variable `FILENAME_farm.any` los archivos tendrán instrucciones include para cada archivo en función de dónde deban incluirse en el archivo de granja.  A continuación se muestra un ejemplo de sintaxis de un `FILENAME_farm.any` como buena referencia:

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

Como puede ver en cada sección de la granja de weretail en lugar de tener toda la sintaxis necesaria, se utiliza una instrucción include.

Veamos la sintaxis de algunas de estas inclusiones para tener la idea de cómo sería cada subinclusión

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Como puede ver, es una nueva lista de nombres de dominio separados por líneas que debe renderizarse desde esta granja sobre los demás.

A continuación, veamos el `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Siguiente -> Comprender la caché](./understanding-cache.md)
