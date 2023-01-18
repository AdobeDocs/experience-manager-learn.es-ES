---
title: Diseño de archivo básico de AMS Dispatcher
description: Comprender el diseño básico de los archivos de Apache y Dispatcher.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 1%

---


# Diseño de archivo básico

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: ¿Qué es Dispatcher?](./what-is-the-dispatcher.md)

Este documento explica el conjunto de archivos de configuración estándar de AMS y el pensamiento detrás de este estándar de configuración

## Estructura de carpetas predeterminada de Enterprise Linux

En AMS, la instalación base utiliza Enterprise Linux como sistema operativo base. Al instalar Apache Webserver, tiene un conjunto de archivos de instalación predeterminado. Estos son los archivos predeterminados que se instalan instalando los RPM básicos proporcionados por el repositorio yum

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

Al seguir y respetar el diseño/estructura de la instalación obtenemos las siguientes ventajas:

- Es más fácil admitir un diseño predecible
- Familiarizado automáticamente con cualquier persona que haya trabajado en instalaciones HTTPD de Enterprise Linux en el pasado
- Permite ciclos de aplicación de parches totalmente compatibles con el sistema operativo sin conflictos ni ajustes manuales
- Evita violaciones de SELinux de contextos de archivos mal etiquetados

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Las imágenes de los servidores de Adobe Managed Services suelen tener pequeñas unidades raíz del sistema operativo.  Ponemos nuestros datos en un volumen separado que normalmente se monta en `/mnt` Entonces usamos ese volumen en lugar de los valores predeterminados para los siguientes directorios predeterminados

`DocumentRoot`
- Predeterminado:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Predeterminado: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Tenga en cuenta que los directorios antiguos y nuevos se asignan de nuevo al punto de montaje original para eliminar la confusión.
Usar un volumen separado no es vital, pero vale la pena
</div>

## Complementos de AMS

AMS se añade a la instalación base de Apache Web Server.

### Raíces del documento

Raíces de documento predeterminadas de AMS:
- Autor:
   - `/mnt/var/www/author/`
- Publicación:
   - `/mnt/var/www/html/`
- Mantenimiento de Catch-All y Health Check
   - `/mnt/var/www/default/`

### Ensayo y habilitación de directorios VirtualHost

Los siguientes directorios le permiten crear archivos de configuración con un área de ensayo en la que puede trabajar con archivos y habilitarlos únicamente cuando estén listos.
- `/etc/httpd/conf.d/available_vhosts/`
   - Esta carpeta aloja todos sus VirtualHost / archivos llamados `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Cuando esté listo para usar la variable `.vhost` archivos, tiene dentro de la variable `available_vhosts` carpeta simbólica enlazarlas utilizando una ruta relativa en la `enabled_vhosts` directory

### Adicional `conf.d` Directorios

Hay piezas adicionales que son comunes en las configuraciones de Apache y hemos creado subdirectorios para permitir una manera limpia de separar esos archivos y no tener todos los archivos en un directorio

#### Directorio de reescrituras

Este directorio puede contener todos los `_rewrite.rules` archivos que cree que contengan la sintaxis típica de RewriteRulesyntax que activen los servidores web de Apache [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) módulo

- `/etc/httpd/conf.d/rewrites/`

#### Directorio de listas blancas

Este directorio puede contener todos los `_whitelist.rules` archivos que cree que contengan la `IP Allow` o `Require IP`sintaxis que atrae servidores web Apache [controles de acceso](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### Directorio de variables

Este directorio puede contener todos los `.vars` archivos creados que contienen variables que puede consumir en sus archivos de configuración

- `/etc/httpd/conf.d/variables/`

### Directorio de configuración específico del módulo Dispatcher

Apache Web Server es muy extensible y cuando un módulo tiene muchos archivos de configuración, es recomendable crear su propio directorio de configuración bajo el directorio base de instalación en lugar de saturar el predeterminado.

Seguimos la mejor práctica y creamos nuestra propia

#### Directorio de archivos de configuración de módulos

- `/etc/httpd/conf.dispatcher.d/`

#### Ensayo y granja habilitada

Los siguientes directorios le permiten crear archivos de configuración con un área de ensayo en la que puede trabajar con archivos y habilitarlos únicamente cuando estén listos.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Esta carpeta aloja todas sus `/myfarm {` archivos llamados `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Cuando está listo para usar el archivo de granja, tiene dentro de la carpeta available_farms enlaces simbólicos usando una ruta relativa al directorio enabled_farms

### Adicional `conf.dispatcher.d` Directorios

Hay piezas adicionales que son subsecciones de las configuraciones de archivos de la granja de Dispatcher y hemos creado subdirectorios para permitir una manera limpia de separar esos archivos y no tener todos los archivos en un directorio

#### Directorio de caché

Este directorio contiene todos los `_cache.any`, `_invalidate.any` archivos creados que contienen sus reglas sobre cómo desea que el módulo gestione los elementos de caché que provienen de AEM, así como la sintaxis de las reglas de invalidación.  Más información sobre esta sección aquí [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Directorio de encabezados de cliente

Este directorio puede contener todos los `_clientheaders.any` archivos creados que contienen listas de encabezados de cliente que desea pasar a AEM cuando entre en vigor una solicitud.  Más información sobre esta sección son [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Directorio de filtros

Este directorio puede contener todos los `_filters.any` archivos creados que contienen todas las reglas de filtro para bloquear o permitir que el tráfico a través de Dispatcher llegue a AEM

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Directorio de renders

Este directorio puede contener todos los `_renders.any` archivos creados que contienen los detalles de conectividad de cada servidor back-end desde el que Dispatcher consumirá contenido

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Directorio de vhosts

Este directorio puede contener todos los `_vhosts.any` archivos creados que contienen una lista de los nombres de dominio y rutas que coinciden con una granja en particular con un servidor back-end en particular

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Estructura de carpetas completa

AMS ha estructurado cada uno de los archivos con extensiones de archivo personalizadas y con la intención de evitar problemas/conflictos de área de nombres y cualquier confusión.

A continuación, se muestra un ejemplo de un conjunto de archivos estándar de una implementación predeterminada de AMS:

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
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## Manteniéndolo ideal

Enterprise Linux tiene ciclos de revisiones para el paquete Apache Webserver Package (httpd).

Cuantos menos archivos predeterminados estén instalados, mejor será, por razones de que si hay correcciones de seguridad o mejoras de configuración se aplican a través del comando RPM / Yum, no aplicará las correcciones sobre la parte superior de un archivo alterado.

En su lugar, crea un `.rpmnew` junto al original.  Esto significa que se perderán algunos cambios que podría haber deseado y se habrá creado más basura en las carpetas de configuración.

Es decir, el RPM durante la instalación de la actualización observará `httpd.conf` si está en el `unaltered` state it will *replace* el archivo y obtendrá las actualizaciones vitales.  Si la variable `httpd.conf` was `altered` luego *no reemplaza* el archivo y, en su lugar, creará un archivo de referencia llamado `httpd.conf.rpmnew` y las muchas correcciones deseadas estarán en ese archivo que no se aplique al inicio del servicio.

Enterprise Linux se configuró correctamente para manejar este caso de uso de una mejor manera.  Le proporcionan áreas en las que puede ampliar o anular los valores predeterminados que establecen para usted.  Dentro de la instalación base de httpd encontrará el archivo `/etc/httpd/conf/httpd.conf`y tiene una sintaxis similar a:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

La idea es que Apache quiere que amplíe los módulos y configuraciones al agregar nuevos archivos al `/etc/httpd/conf.d/` y `/etc/httpd/conf.modules.d/` directorios con extensión de archivo de `.conf`

Como el ejemplo perfecto al agregar el módulo Dispatcher a Apache, debe crear un módulo `.so` en ` /etc/httpd/modules/` y luego inclúyalo añadiendo un archivo en `/etc/httpd/conf.modules.d/02-dispatcher.conf` con el contenido para cargar el módulo `.so` file

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Aviso:</b>
no modificamos ningún archivo ya existente que Apache haya proporcionado.  En su lugar, simplemente añadimos el nuestro a los directorios a los que estaban destinados.
</div><br/>

Ahora consumimos nuestro módulo en nuestro archivo <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> que inicializa nuestro módulo y carga el archivo de configuración inicial específico del módulo

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Nuevamente notará que hemos agregado archivos y módulos pero no hemos alterado ningún archivo original.  Esto nos da la funcionalidad deseada y nos protege de la falta de correcciones de parches deseadas, así como de mantener el más alto nivel de compatibilidad con cada actualización del paquete.

[Siguiente -> Explicación de los archivos de configuración](./explanation-config-files.md)