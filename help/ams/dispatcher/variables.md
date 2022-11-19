---
title: Uso y comprensión de las variables en la configuración AEM Dispatcher
description: Obtenga información sobre cómo utilizar variables en los archivos de configuración de los módulos Apache y Dispatcher para llevarlas al siguiente nivel.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# Uso y comprensión de las variables

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Explicación de la caché](./understanding-cache.md)

Este documento explicará cómo puede aprovechar el poder de las variables en el servidor web Apache y en los archivos de configuración del módulo Dispatcher.

## Variables

Apache admite variables y desde la versión 4.1.9 del módulo Dispather también las admite.

Podemos aprovecharlas para hacer un montón de cosas útiles como:

- Asegúrese de que todo lo que sea específico del entorno no esté en línea en las configuraciones, sino extraído para asegurarse de que los archivos de configuración de dev funcionen en prod con la misma salida funcional.
- Alternar características y niveles de registro de cambios de archivos inmutables que proporciona AMS y que no le permitirán cambiar.
- Modificar qué inclusiones utilizar en función de variables como `RUNMODE` y `ENV_TYPE`
- Coincidencia `DocumentRoot`&#39;s y `VirtualHost` Nombres DNS entre configuraciones de Apache y configuraciones de módulos.

## Uso de variables de línea de base

Debido al hecho de que los archivos de línea de base de AMS son de solo lectura e inmutables, hay funciones que se pueden desactivar y activar, así como configurarse editando las variables que consumen.

### Variables de línea de base

Las variables predeterminadas de AMS se declaran en el archivo `/etc/httpd/conf.d/variables/ootb.vars`.  Este archivo no es editable pero existe para asegurarse de que las variables no tengan valores nulos.  Se incluyen primero y después de que se incluyen `/etc/httpd/conf.d/variables/ams_default.vars`.  Puede editar ese archivo para modificar los valores de estas variables o incluso incluir los mismos nombres y valores de las variables en su propio archivo.

Este es un ejemplo del contenido del archivo `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Ejemplo 1: Forzar SSL

Las variables que se muestran arriba `AUHOR_FORCE_SSL`o `PUBLISH_FORCE_SSL` se puede establecer en 1 para establecer reglas de reescritura que obliguen a los usuarios finales a redirigir a https cuando accedan a una solicitud http

Esta es la sintaxis del archivo de configuración que permite que funcione este conmutador:

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

Como puede ver, las reglas de reescritura incluyen lo que tiene el código para redirigir el navegador del usuario final, pero la variable que se establece en 1 es lo que permite que el archivo se utilice o no

### Ejemplo 2 - Nivel de registro

Las variables `DISP_LOG_LEVEL` se puede utilizar para configurar lo que desea tener para el nivel de registro que se utiliza realmente en la configuración en ejecución.

Este es el ejemplo de sintaxis que existe en los archivos de configuración de la línea base de ams:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Si necesita aumentar el nivel de registro de Dispatcher, simplemente actualice la variable `ams_default.vars` variable `DISP_LOG_LEVEL` hasta el nivel que desee.

Los valores de ejemplo pueden ser un número entero o la palabra:

| Nivel de registro | Valor entero | Valor de palabra |
| --- | --- | --- |
| Seguimiento | 4 | trace |
| Depurar | 3 | depurar |
| Información | 2 | información |
| Advertencia | 1 | avisar |
| Error | 0 | error |

### Ejemplo 3: Listas blancas

Las variables `AUTHOR_WHITELIST_ENABLED` y `PUBLISH_WHITELIST_ENABLED` se puede establecer en 1 para activar reglas de reescritura que incluyan reglas para permitir o no el tráfico del usuario final en función de la dirección IP.  La activación de esta función debe combinarse con la creación de un archivo de reglas de la lista blanca para que se incluya.

A continuación se muestran algunos ejemplos de sintaxis de cómo la variable habilita las inclusiones de los archivos de la lista blanca y un ejemplo de archivo de la lista blanca

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

Como puede ver, la variable `sample_whitelist.rules` aplica la restricción de IP, pero al alternar la variable se permite incluirla en la variable `sample.vhost`

## Dónde colocar las variables

### Argumentos de inicio del servidor web

AMS colocará variables específicas del servidor/topología en los argumentos de inicio del proceso Apache dentro del archivo `/etc/sysconfig/httpd`

Este archivo tiene variables predefinidas como las que se muestran aquí:

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

No es algo que pueda cambiar, pero es bueno aprovechar en sus archivos de configuración

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Debido al hecho de que este archivo solo se incluye cuando se inicia el servicio.  Es necesario reiniciar el servicio para recoger los cambios.  Lo que significa que una recarga no es suficiente, pero se necesita un reinicio
</div>

### Archivos de variables (`.vars`)

Las variables personalizadas proporcionadas por su código deben incluirse en `.vars` archivos dentro del directorio `/etc/httpd/conf.d/variables/`

Estos archivos pueden tener cualquier variable personalizada que desee y se pueden ver algunos ejemplos de sintaxis en los siguientes archivos de ejemplo

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

Al crear sus propias variables, nombre los archivos según su contenido y siga los estándares de nomenclatura que se proporcionan en el manual [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  En el ejemplo anterior puede ver que el archivo de variables aloja las diferentes entradas DNS como variables para usar en los archivos de configuración.

## Uso de variables

Ahora que ha definido las variables dentro de los archivos de variables, querrá saber cómo usarlas correctamente dentro de los demás archivos de configuración.

Usaremos el ejemplo `.vars` archivos de arriba para ilustrar un caso de uso adecuado.

Queremos incluir todas las variables basadas en el entorno globalmente, crearemos el archivo `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Sabemos que cuando el servicio httpd se inicia, extrae las variables establecidas por AMS en `/etc/sysconfig/httpd` y tiene el conjunto de variables de `ENV_TYPE` y `RUNMODE`

Cuando esto sea global `.conf` el archivo se incorpora se extraerá antes de tiempo porque el orden de inclusión de archivos en `conf.d` es orden alfanumérico de carga media 000 en el nombre del archivo asegurará que se carga antes que los otros archivos en el directorio.

La sentencia include también está usando una variable en el nombre del archivo.  Esto puede cambiar el archivo que realmente cargará en función del valor que hay en la variable `ENV_TYPE` y `RUNMODE` variables.

Si la variable `ENV_TYPE` el valor es `dev` a continuación, el archivo que se utiliza es:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Si la variable `ENV_TYPE` el valor es `stage` a continuación, el archivo que se utiliza es:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Si la variable `RUNMODE` el valor es `preview` a continuación, el archivo que se utiliza es:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Cuando ese archivo se incluya, nos permitirá usar los nombres de las variables que se almacenaron dentro de.

En nuestra `/etc/httpd/conf.d/available_vhosts/weretail.vhost` podemos intercambiar la sintaxis normal que solo funcionaba para dev:

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Con una nueva sintaxis que utiliza el poder de las variables para trabajar para dev, stage y prod:

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

En nuestra `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` podemos intercambiar la sintaxis normal que solo funcionaba para dev:

```
"dev.weretail.com" 
"dev.weretail.net"
```

Con una nueva sintaxis que utiliza el poder de las variables para trabajar para dev, stage y prod:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Estas variables tienen una gran cantidad de reutilización para individualizar la configuración de ejecución sin tener que tener diferentes archivos implementados por entorno.  Básicamente, puede crear plantillas de los archivos de configuración mediante el uso de variables e incluir archivos basados en variables.

## Visualización de los valores de las variables

A veces, al utilizar variables, tenemos que buscar para ver cuáles podrían ser los valores en nuestros archivos de configuración.  Existe una forma de ver las variables resueltas ejecutando los siguientes comandos en el servidor:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

El aspecto de las variables en la configuración compilada de Apache:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

El aspecto de las variables en la configuración compilada de Dispatcher:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

Desde la salida de los comandos verá las diferencias de la variable en el archivo de configuración frente a la salida compilada.

Configuración de ejemplo

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

Ahora ejecute los comandos para ver la salida compilada

Configuración de Apache compilada:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Configuración de Dispatcher compilada:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Siguiente -> Vaciado](./disp-flushing.md)