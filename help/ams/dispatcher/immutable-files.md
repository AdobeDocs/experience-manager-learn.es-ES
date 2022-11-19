---
title: Archivos de sólo lectura o inmutables de AMS Dispatcher
description: Comprender por qué algunos archivos son de solo lectura o no editables y cómo realizar los cambios funcionales que desee
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 1%

---


# Archivos de solo lectura o inmutables en AMS

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Registros comunes](./common-logs.md)

## Descripción

Este documento describirá qué archivos están bloqueados y no se van a cambiar, y cómo hacer los ajustes de configuración deseados correctamente.

Cuando AMS aprovisiona un sistema, implementan una configuración de línea de base que hace que todo funcione y sea seguro.  Estas son cosas que AMS desea garantizar que permanezca como punto de referencia de funcionalidad y seguridad.  Para lograr esto, algunos archivos están marcados como de solo lectura e inmutables para evitar que los cambie.

El diseño no impide modificar su comportamiento y anular los cambios que necesite.  En lugar de cambiar estos archivos, superpondrá su propio archivo que reemplaza al original.

Esto también le permite estar seguro de que cuando AMS parches los despachantes con las últimas correcciones y mejoras de seguridad, no alterarán sus archivos.  A continuación, puede seguir beneficiándose de las mejoras y adoptar solo los cambios que desee.
![Muestra un carril de boliche con una bola que baja por el carril.  La pelota tiene una flecha con la palabra que te muestra.  Los parachoques se elevan y tienen las palabras archivos inmutables encima de ellos.](assets/immutable-files/bowling-file-immutability.png "Inmutabilidad de los archivos de bolos")
Como se ilustra en la imagen anterior, los archivos inmutables no te impiden jugar.  Simplemente te evitan dañar tu actuación y te mantienen en el carril.  Este método nos permite las pocas características clave:

- Las personalizaciones se gestionan en sus propios espacios seguros
- La superposición de cambios personalizados refleja la de los métodos de superposición en AEM
- La aplicación de parches a las configuraciones de AMS se puede realizar sin alterar las personalizaciones
- La prueba de la instalación de base frente a las configuraciones personalizadas se puede hacer simultáneamente para ayudar a discernir si los problemas son causados por personalizaciones o por algún otro motivo ¿Qué archivos?


Esta es una lista típica de archivos implementados con Dispatcher:

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

Para determinar qué archivos son inmutables, puede ejecutar el siguiente comando en un Dispatcher para ver:

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

Esta es una respuesta de ejemplo de los archivos que son inmutables:

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## Cómo realizar cambios

### Variables

Las variables permiten realizar cambios funcionales sin cambiar los propios archivos de configuración.  Algunos elementos de la configuración se pueden ajustar ajustando los valores de las variables.  Un ejemplo que podemos resaltar del archivo `/etc/httpd/conf.d/dispatcher_vhost.conf` se muestra aquí:

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

Vea cómo la directiva DispatcherLogLevel tiene una variable de `DISP_LOG_LEVEL` en lugar del valor normal que vería allí.  Encima de esa sección de código también verá una instrucción include en un archivo de variables.  El archivo de variable `/etc/httpd/conf.d/variables/ams_default.vars` es donde queremos ver a continuación.  A continuación se muestra el contenido del archivo de variables:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Verá más arriba que el valor actual de `DISP_LOG_LEVEL` es `info`.  Podemos ajustarlo para rastrear o depurar, o el número, valor o nivel que elija.  Ahora, en todas partes que controlen el nivel de registro, se ajustará automáticamente.

### Método de superposición

Por favor, comprenda los archivos include de nivel superior porque estos serán su lugar de inicio para realizar cualquier personalización.  Para empezar con un ejemplo sencillo, tenemos un escenario en el que queremos agregar un nuevo nombre de dominio que pretendemos señalar en este Dispatcher.  El ejemplo de dominio que usaremos es we-retail.adobe.com.  Empezaremos copiando un archivo de configuración existente en uno nuevo donde podemos añadir nuestros cambios:

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

Hemos copiado el archivo aem_publish.vhost existente porque ya tiene lo que necesitamos para que las cosas funcionen y no queremos reinventar un inicio ya fuerte.  Ahora editamos el nuevo archivo weretail.vhost y realizamos los cambios necesarios.

Antes:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Después:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Ahora hemos actualizado nuestra `ServerName` y `ServerAlias` para que coincida con los nuevos nombres de dominio, así como para actualizar otros encabezados de ruta de exploración.  Ahora habilitemos nuestro nuevo archivo para permitir que Apache sepa utilizar nuestro nuevo archivo:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Ahora, el servidor web Apache sabe que el dominio es algo para lo que debería generar tráfico, pero todavía necesitamos informar al módulo de Dispatcher que tiene un nuevo nombre de dominio que cumplir.  Empezaremos creando un nuevo `*_vhost.any` file `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` y dentro de ese archivo pondremos el nombre de dominio que queremos honrar:

```
"we-retail.adobe.com"
```

Ahora necesitamos crear un nuevo archivo de granja que use nuestro nuevo archivo de entrada vhost, y empezaremos copiando un archivo de inicio sólido a nuestro nuevo.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Mostremos los cambios que tendremos que realizar en este archivo de granja

Antes:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

Después:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

Ahora hemos actualizado el nombre de la granja de servidores, y la incluye que utiliza en la variable `/virtualhosts` de la configuración de granja.  Necesitamos habilitar este nuevo archivo de granja para que pueda utilizarlo en la configuración en ejecución:

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Ahora, simplemente volveríamos a cargar el servicio webserver y usaríamos nuestro nuevo dominio!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que solo cambiamos las piezas que necesitábamos para cambiar y aprovechamos las inclusiones existentes y el código que venía con los archivos de configuración de línea de base.  Solo tenemos que definir el elemento que necesitamos cambiar.  Facilita mucho las cosas y nos permite mantener menos código
</div>