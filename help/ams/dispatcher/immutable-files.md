---
title: Archivos de solo lectura o inmutables de AMS Dispatcher
description: Explicación de por qué algunos archivos son de solo lectura o no editables y cómo realizar los cambios funcionales deseados
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---

# Archivos de solo lectura o inmutables en AMS

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Registros comunes](./common-logs.md)

## Descripción

Este documento describirá qué archivos están bloqueados y no se van a cambiar, y cómo realizar correctamente los ajustes de configuración deseados.

Cuando AMS aprovisiona un sistema, implementa una configuración de línea de base que hace que todo funcione y sea seguro.  Estas son cosas que AMS desea garantizar que se mantengan como línea de base de funcionalidad y seguridad.  Para lograr esto, algunos archivos están marcados como de solo lectura e inmutables para evitar que los cambie.

El diseño no impide modificar su comportamiento y anular los cambios que necesite.  En lugar de cambiar estos archivos, superpondrá su propio archivo, que reemplaza al original.

Esto también le permite asegurarse de que cuando AMS aplique parches a Dispatchers con las últimas correcciones y mejoras de seguridad, no alterarán sus archivos.  A continuación, puede seguir beneficiándose de las mejoras y adoptar solo los cambios que desee.
![Muestra una pista de bolos con una bola rodando por ella.  La bola tiene una flecha con la palabra que te muestra.  Los protectores de los carriles están elevados y tienen las palabras archivos inmutables encima.](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
Como se ilustra en la imagen anterior, los archivos inmutables no le impiden jugar.  Simplemente evitan que se perjudique su rendimiento y lo mantienen en el carril.  Este método nos permite disponer de unas cuantas características muy clave:

- Las personalizaciones se gestionan en sus propios espacios seguros
- AEM La superposición de cambios personalizados refleja la de los métodos de superposición en los informes de estado de la aplicación
- La aplicación de parches a las configuraciones de AMS se puede realizar sin alterar las personalizaciones
- Se puede probar simultáneamente la instalación base frente a las configuraciones personalizadas para ayudar a discernir si los problemas son causados por las personalizaciones o por otra cosa.


Esta es una lista típica de archivos implementados con un Dispatcher:

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

Vea cómo la directiva DispatcherLogLevel tiene una variable de `DISP_LOG_LEVEL` en lugar del valor normal que vería allí.  Encima de esa sección de código también verá una instrucción de inclusión en un archivo de variables.  El archivo de variables `/etc/httpd/conf.d/variables/ams_default.vars` es lo que queremos ver a continuación.  Este es el contenido del archivo de variables:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Más arriba puede ver el valor actual de `DISP_LOG_LEVEL` la variable es `info`.  Podemos ajustarlo para rastrear o depurar, o al valor numérico/nivel de su elección.  Ahora, en cualquier lugar que controle el nivel de registro, se ajustará automáticamente.

### Método de superposición

Comprenda los archivos de inclusión de nivel superior, ya que este será su punto de partida para realizar cualquier personalización.  Para empezar con un ejemplo sencillo, tenemos un escenario en el que queremos agregar un nuevo nombre de dominio que pretendemos apuntar a este Dispatcher.  El dominio de ejemplo que utilizaremos is we-retail.adobe.com.  Empezaremos copiando un archivo de configuración existente en uno nuevo donde podemos agregar nuestros cambios:

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

Ahora hemos actualizado nuestro `ServerName` y `ServerAlias` para que coincida con los nuevos nombres de dominio, así como para actualizar otros encabezados de ruta de exploración.  A continuación, habilitemos nuestro nuevo archivo para permitir que Apache sepa utilizarlo:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Ahora, el servidor web de Apache sabe que el dominio es algo para lo que debería generar tráfico, pero aún necesitamos informar al módulo de Dispatcher que tiene un nuevo nombre de dominio que respetar.  Empezaremos creando un nuevo `*_vhost.any` archivo `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` y dentro de ese archivo pondremos el nombre de dominio que queremos respetar:

```
"we-retail.adobe.com"
```

Ahora necesitamos crear un nuevo archivo de granja que use nuestro nuevo archivo de entrada vhost y empezaremos copiando un archivo de inicio sólido al nuevo.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Veamos los cambios que tendremos que realizar en este archivo de granja

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

Ahora hemos actualizado el nombre de la granja y la inclusión que utiliza en la `/virtualhosts` de la configuración de la granja.  Necesitamos habilitar este nuevo archivo de granja para que pueda utilizarlo en la configuración en ejecución:

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Ahora simplemente volveríamos a cargar el servicio webserver y usaríamos nuestro nuevo dominio.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que solo cambiamos las piezas que necesitábamos cambiar y aprovechamos las inclusiones existentes y el código que venía con los archivos de configuración de línea de base.  Solo tenemos que definir el elemento que necesitamos cambiar.  Facilita mucho las cosas y nos permite mantener menos código
</div>

[Siguiente -> Comprobación de estado de Dispatcher](./health-check.md)
