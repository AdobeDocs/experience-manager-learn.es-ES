---
title: AEM Uso y comprensión de las variables en la configuración de Dispatcher de la
description: Aprenda a utilizar variables en los archivos de configuración de los módulos Apache y Dispatcher para llevarlas al siguiente nivel.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---

# Uso y comprensión de las variables

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Explicación de la caché](./understanding-cache.md)

Este documento explicará cómo puede aprovechar el poder de las variables en el servidor web Apache y en los archivos de configuración de su módulo Dispatcher.

## Variables

Apache admite variables y, desde la versión 4.1.9 del módulo Dispatcher, también las admite.

Podemos aprovechar esto para hacer un montón de cosas útiles como:

- Asegúrese de que todo lo que sea específico del entorno no esté en línea en las configuraciones, sino que se extraiga para garantizar que los archivos de configuración de dev funcionen en prod con la misma salida funcional.
- Alternar funciones y cambiar niveles de registro de archivos inmutables que proporciona AMS y que no le permite cambiar.
- Modificar que incluye usar en función de variables como `RUNMODE` y `ENV_TYPE`
- Hacer coincidir los nombres DNS de `DocumentRoot` y `VirtualHost` entre las configuraciones de Apache y las configuraciones de módulo.

## Uso de variables de línea base

Debido a que los archivos de línea de base de AMS son de solo lectura e inmutables, hay funciones que se pueden desactivar y activar, así como configurarse editando las variables que consumen.

### Variables de línea base

Las variables predeterminadas de AMS se declaran en el archivo `/etc/httpd/conf.d/variables/ootb.vars`.  Este archivo no se puede editar, pero existe para asegurarse de que las variables no tengan valores nulos.  Se incluyen primero y después de que incluyamos `/etc/httpd/conf.d/variables/ams_default.vars`.  Puede editar ese archivo para modificar los valores de estas variables o incluso incluir los mismos nombres y valores de variables en su propio archivo.

Este es un ejemplo del contenido del archivo `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Ejemplo 1: Forzar SSL

Las variables mostradas arriba `AUHOR_FORCE_SSL` o `PUBLISH_FORCE_SSL` se pueden establecer en 1 para activar reglas de reescritura que obliguen a los usuarios finales a entrar en una solicitud http a ser redirigidos a https

Esta es la sintaxis del archivo de configuración que permite que funcione esta opción:

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

Como puede ver, las reglas de reescritura incluyen es lo que tiene el código para redirigir el explorador de los usuarios finales, pero la variable que se establece en 1 es lo que permite utilizar el archivo o no

### Ejemplo 2 - Nivel de registro

Las variables `DISP_LOG_LEVEL` se pueden usar para establecer lo que desea tener para el nivel de registro que se utiliza realmente en la configuración en ejecución.

Este es el ejemplo de sintaxis que existe en los archivos de configuración de línea base de AEM:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Si necesita aumentar el nivel de registro de Dispatcher, actualice la variable `ams_default.vars` `DISP_LOG_LEVEL` al nivel que desee.

Ejemplo Los valores pueden ser un número entero o la palabra:

| Nivel de registro | Valor entero | Valor de Word |
| --- | --- | --- |
| Trazado | 4 | trazar |
| Depurar | 3 | depurar |
| Información | 2 | información |
| Advertencia | 1 | advertir |
| Error | 0 | error |

### Ejemplo 3 - Listas blancas

Las variables `AUTHOR_WHITELIST_ENABLED` y `PUBLISH_WHITELIST_ENABLED` se pueden establecer en 1 para activar reglas de reescritura que incluyan reglas para permitir o no permitir el tráfico del usuario final basado en la dirección IP.  Alternar esta función en debe combinarse con la creación de un archivo de reglas de lista blanca para que se incluya.

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

Como puede ver, `sample_whitelist.rules` aplica la restricción de IP, pero al cambiar la variable se puede incluir en `sample.vhost`

## Dónde colocar las variables

### Argumentos de inicio del servidor web

AMS colocará variables específicas del servidor/topología en los argumentos de inicio del proceso de Apache dentro del archivo `/etc/sysconfig/httpd`

Este archivo tiene variables predefinidas como se muestra aquí:

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

No es algo que pueda cambiar, pero puede aprovecharlo en sus archivos de configuración

>[!NOTE]
>
>Debido a que este archivo solo se incluye cuando se inicia el servicio.  Es necesario reiniciar el servicio para recoger los cambios.  Lo que significa que una recarga no es suficiente, pero en su lugar se necesita un reinicio

### Archivos de variables (`.vars`)

Las variables personalizadas proporcionadas por su código deben residir en `.vars` archivos dentro del directorio `/etc/httpd/conf.d/variables/`

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

Al crear sus propias variables, asigne nombres a los archivos en función de su contenido y siga los estándares de nomenclatura proporcionados en el manual [aquí](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  En el ejemplo anterior puede ver que el archivo de variables aloja las diferentes entradas DNS como variables para usarlas en los archivos de configuración.

## Uso de variables

Ahora que ha definido las variables dentro de los archivos de variables, querrá saber cómo utilizarlas correctamente dentro de los demás archivos de configuración.

Utilizaremos los archivos de ejemplo `.vars` de arriba para ilustrar un caso de uso adecuado.

Queremos incluir todas las variables basadas en el entorno globalmente. Crearemos el archivo `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Sabemos que cuando el servicio httpd se inicia, extrae las variables establecidas por AMS en `/etc/sysconfig/httpd` y tiene el conjunto de variables de `ENV_TYPE` y `RUNMODE`

Cuando se extraiga este archivo global `.conf`, se recuperará antes de tiempo, ya que el orden de inclusión de los archivos en `conf.d` es un orden de carga alfanumérico. El promedio 000 en el nombre de archivo garantiza que se carga antes que los demás archivos del directorio.

La instrucción include también utiliza una variable en el nombre del archivo.  Esto puede cambiar el archivo que realmente cargará basándose en el valor de las variables `ENV_TYPE` y `RUNMODE`.

Si el valor `ENV_TYPE` es `dev`, el archivo que se utiliza es:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Si el valor `ENV_TYPE` es `stage`, el archivo que se utiliza es:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Si el valor `RUNMODE` es `preview`, el archivo que se utiliza es:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Cuando se incluya ese archivo, nos permitirá usar los nombres de variables que se almacenaron dentro de.

En nuestro archivo `/etc/httpd/conf.d/available_vhosts/weretail.vhost` podemos intercambiar la sintaxis normal que solo funcionaba para dev:

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

En nuestro archivo `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` podemos intercambiar la sintaxis normal que solo funcionaba para dev:

```
"dev.weretail.com" 
"dev.weretail.net"
```

Con una nueva sintaxis que utiliza el poder de las variables para trabajar para dev, stage y prod:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Estas variables tienen una gran cantidad de reutilización para individualizar la configuración de ejecución sin tener que tener diferentes archivos implementados por entorno.  Básicamente, puede crear plantillas de archivos de configuración con el uso de variables e incluir archivos basados en variables.

## Visualización de valores de variables

A veces, al utilizar variables, tenemos que buscar para ver cuáles podrían ser los valores en nuestros archivos de configuración.  Existe una forma de ver las variables resueltas ejecutando los siguientes comandos en el servidor:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Aspecto de las variables en la configuración de Apache compilada:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Aspecto de las variables en la configuración de Dispatcher compilada:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

En la salida de los comandos verá las diferencias de la variable en el archivo de configuración frente a la salida compilada.

Ejemplo de configuración

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
```

Ahora ejecute los comandos para ver el resultado compilado

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
