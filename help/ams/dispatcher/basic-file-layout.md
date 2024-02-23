---
title: Diseño básico de archivo de AMS Dispatcher
description: Comprenda el diseño básico de archivos de Apache y Dispatcher.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 354
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 0%

---

# Diseño básico del archivo

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: ¿Qué es Dispatcher?](./what-is-the-dispatcher.md)

Este documento explica el conjunto de archivos de configuración estándar de AMS y las ideas subyacentes a este estándar de configuración

## Estructura de carpetas de Enterprise Linux predeterminada

En AMS, la instalación base utiliza Enterprise Linux como sistema operativo base. Al instalar el servidor web Apache, tiene un conjunto de archivos de instalación predeterminado. Estos son los archivos predeterminados que se instalan instalando los RPM básicos proporcionados por el repositorio yum

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

Al seguir y cumplir con el diseño / estructura de la instalación, obtenemos los siguientes beneficios:

- Es más fácil admitir un diseño predecible
- Automáticamente familiar para cualquiera que haya trabajado en instalaciones HTTPD de Enterprise Linux en el pasado
- Permite aplicar parches a los ciclos totalmente compatibles con el sistema operativo sin conflictos ni ajustes manuales
- Evita violaciones de SELinux de contextos de archivo etiquetados incorrectamente

>[!BEGINSHADEBOX &quot;Nota&quot;]

Las imágenes de los servidores Managed Services de Adobe suelen tener pequeñas unidades raíz del sistema operativo.  Colocamos nuestros datos en un volumen separado que normalmente se monta en `/mnt`
A continuación, utilizamos ese volumen en lugar de los valores predeterminados para los siguientes directorios predeterminados

`DocumentRoot`
- Predeterminado:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Predeterminado: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Tenga en cuenta que los directorios antiguos y nuevos se asignan de nuevo al punto de montaje original para eliminar confusiones.
Usar un volumen separado no es vital, pero vale la pena

>[!ENDSHADEBOX]

## Complementos de AMS

AMS se suma a la instalación base del servidor web Apache.

### Raíces de documento

Raíces de documento predeterminadas de AMS:
- Autor:
   - `/mnt/var/www/author/`
- Publicar:
   - `/mnt/var/www/html/`
- Mantenimiento de comprobación de estado y captador global
   - `/mnt/var/www/default/`

### Directorios VirtualHost de ensayo y habilitados

Los siguientes directorios le permiten crear archivos de configuración con un área de ensayo en la que puede trabajar y sólo habilitarlos cuando estén listos.
- `/etc/httpd/conf.d/available_vhosts/`
   - Esta carpeta aloja todos sus VirtualHost / archivos llamados `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Cuando esté listo para usar el `.vhost` archivos, tiene dentro de `available_vhosts` enlace simbólico de carpeta mediante una ruta relativa a la `enabled_vhosts` directorio

### Adicional `conf.d` Directorios

Hay partes adicionales que son comunes en las configuraciones de Apache y hemos creado subdirectorios para permitir una manera limpia de separar esos archivos y no tener todos los archivos en un directorio

#### Reescribe el directorio

Este directorio puede contener todos los `_rewrite.rules` archivos que cree que contengan la sintaxis típica de RewriteRules que involucre a los servidores web Apache [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) módulo

- `/etc/httpd/conf.d/rewrites/`

#### Directorio de listas blancas

Este directorio puede contener todos los `_whitelist.rules` archivos que crea y que contienen sus `IP Allow` o `Require IP`sintaxis que involucra a los servidores web Apache [controles de acceso](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### Directorio de variables

Este directorio puede contener todos los `.vars` archivos que crea y que contienen variables que puede consumir en sus archivos de configuración

- `/etc/httpd/conf.d/variables/`

### Directorio de configuración específico del módulo de Dispatcher

El servidor web Apache es muy extensible y cuando un módulo tiene muchos archivos de configuración, es recomendable crear su propio directorio de configuración bajo el directorio base de instalación en lugar de agrupar el predeterminado.

Seguimos las mejores prácticas y creamos las nuestras

#### Directorio de archivos de configuración del módulo

- `/etc/httpd/conf.dispatcher.d/`

#### Ensayo y granja habilitada

Los siguientes directorios le permiten crear archivos de configuración con un área de ensayo en la que puede trabajar y sólo habilitarlos cuando estén listos.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Esta carpeta aloja todos sus `/myfarm {` archivos llamados `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Cuando esté listo para utilizar el archivo de granja, tiene dentro de la carpeta available_farms un enlace simbólico que utiliza una ruta relativa al directorio enabled_farms

### Adicional `conf.dispatcher.d` Directorios

Hay partes adicionales que son subsecciones de las configuraciones de archivos de la granja de Dispatcher y hemos creado subdirectorios para permitir una manera limpia de separar esos archivos y no tener todos los archivos en un directorio

#### Directorio de caché

Este directorio contiene todos los `_cache.any`, `_invalidate.any` AEM los archivos que cree que contengan sus reglas sobre cómo desea que el módulo gestione los elementos de almacenamiento en caché que provienen de la sintaxis de reglas de invalidación, así como los elementos de caché que provienen de la sintaxis de reglas de invalidación.  Encontrará más detalles sobre esta sección aquí [aquí](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Directorio de encabezados de cliente

Este directorio puede contener todos los `_clientheaders.any` AEM los archivos que cree que contengan listas de los Client Headers que desea pasar a los clientes cuando se reciba una solicitud de acceso a los mismos.  Encontrará más información sobre esta sección [aquí](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Directorio de filtros

Este directorio puede contener todos los `_filters.any` AEM los archivos que cree que contengan todas las reglas de filtro para bloquear o permitir que el tráfico a través de Dispatcher llegue a los archivos de destino que se encuentran en la interfaz de usuario de Dispatcher

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Directorio de procesamientos

Este directorio puede contener todos los `_renders.any` archivos que cree que contengan los detalles de conectividad con cada servidor back-end desde el que Dispatcher consumirá contenido

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Directorio Vhosts

Este directorio puede contener todos los `_vhosts.any` los archivos que cree que contengan una lista de los nombres de dominio y las rutas de acceso que coincidirán con una granja concreta en un servidor back-end concreto

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Completar estructura de carpetas

AMS ha estructurado cada uno de los archivos con extensiones de archivo personalizadas y con la intención de evitar problemas/conflictos de área de nombres y cualquier confusión.

Este es un ejemplo de un conjunto de archivos estándar de una implementación predeterminada de AMS:

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

## Mantenerlo ideal

Enterprise Linux tiene ciclos de parches para el paquete Apache Webserver (httpd).

Los archivos predeterminados menos instalados cambian mejor, por motivos que si hay correcciones de seguridad parcheadas o mejoras de configuración aplicadas a través del comando RPM / Yum, no se aplicarán las correcciones sobre un archivo alterado.

En su lugar, crea un `.rpmnew` junto al original.  Esto significa que se perderán algunos cambios que podría haber deseado y se habrá creado más elementos no utilizados en las carpetas de configuración.

Es decir, el RPM durante la instalación de la actualización se fijará en `httpd.conf` si está en el `unaltered` diga que lo hará *replace* el archivo y usted recibirá las actualizaciones vitales.  Si la variable `httpd.conf` era `altered` entonces *no se reemplazará* el archivo y, en su lugar, creará un archivo de referencia llamado `httpd.conf.rpmnew` y las muchas correcciones deseadas estarán en ese archivo que no se aplica al inicio del servicio.

Enterprise Linux se ha configurado correctamente para gestionar mejor este caso de uso.  Proporcionan áreas en las que se pueden ampliar o anular los valores predeterminados que establecen.  Dentro de la instalación base de httpd encontrará el archivo `/etc/httpd/conf/httpd.conf`y tiene una sintaxis similar a:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

La idea es que Apache quiera que amplíe los módulos y las configuraciones al añadir nuevos archivos a `/etc/httpd/conf.d/` y `/etc/httpd/conf.modules.d/` directorios con una extensión de archivo de `.conf`

Como ejemplo perfecto, al agregar el módulo de Dispatcher a Apache, crearía un módulo `.so` archivo en ` /etc/httpd/modules/` y luego inclúyalo agregando un archivo en `/etc/httpd/conf.modules.d/02-dispatcher.conf` con el contenido para cargar el módulo `.so` archivo

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

>[!NOTE]
>
>No modificamos ningún archivo ya existente que Apache haya proporcionado. En lugar de eso, agregamos los nuestros a los directorios a los que iban a ir.

Ahora consumimos nuestro módulo en nuestro archivo <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> que inicializa nuestro módulo y carga el archivo de configuración inicial específico del módulo

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Nuevamente notará que hemos agregado archivos y módulos, pero no alterado ningún archivo original.  Esto nos proporciona la funcionalidad deseada y nos protege de la falta de correcciones de parches deseadas, así como de mantener al máximo nivel de compatibilidad con cada actualización del paquete.

[Siguiente -> Explicación de los archivos de configuración](./explanation-config-files.md)
