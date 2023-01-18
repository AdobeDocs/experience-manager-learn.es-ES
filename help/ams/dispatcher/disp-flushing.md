---
title: Vaciado de Dispatcher AEM
description: Comprender cómo AEM invalida los archivos de caché antiguos de Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '2223'
ht-degree: 0%

---


# URL mnemónicas de Dispatcher

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Uso y comprensión de las variables](./variables.md)

Este documento ofrece instrucciones sobre cómo se produce el vaciado y explica el mecanismo que ejecuta el vaciado y la invalidación de la caché.


## Funcionamiento

### Orden de funcionamiento

El flujo de trabajo típico se describe mejor cuando los autores de contenido activan una página, cuando el editor recibe el nuevo contenido, déclencheur una solicitud de vaciado a Dispatcher como se muestra en el diagrama siguiente:
![el autor activa el contenido, lo que hace que el editor de déclencheur envíe una solicitud de vaciado a Dispatcher](assets/disp-flushing/dispatcher-flushing-order-of-events.png "Dispatcher-flushing-order-of-events")
Esta cadena de eventos resalta que solo vaciamos los elementos cuando son nuevos o han cambiado.  Esto garantiza que el editor haya recibido el contenido antes de borrar la caché para evitar condiciones de carrera en las que el vaciado podría producirse antes de que los cambios se puedan recoger del editor.

## Agentes de replicación

En el autor, hay un agente de replicación configurado para indicar al editor que cuando algo se activa, se déclencheur enviar el archivo y todas sus dependencias al editor.

Cuando el editor recibe el archivo, tiene un agente de replicación configurado para señalar al Dispatcher que déclencheur en el evento de recepción.  Luego serializará una solicitud de vaciado y la publicará en Dispatcher.

### AGENTE DE REPLICACIÓN DEL AUTOR

Estas son algunas capturas de pantalla de un agente de replicación estándar configurado
![captura de pantalla del agente de replicación estándar de la página web AEM /etc/replication.html](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

Por lo general, hay 1 o 2 agentes de replicación configurados en el autor para cada publicador al que replican el contenido.

En primer lugar, el agente de replicación estándar que impulsa las activaciones de contenido.

Segundo, el agente inverso.  Esto es opcional y está configurado para comprobar la bandeja de salida de cada editor y ver si hay contenido nuevo que extraer al autor como una actividad de replicación inversa

### AGENTE DE REPLICACIÓN DEL EDITOR

A continuación se muestra un ejemplo de capturas de pantalla de un agente de replicación de vaciado estándar configurado
![captura de pantalla del agente de replicación de vaciado estándar de la página web AEM /etc/replication.html](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### REPLICACIÓN DE VACIADO DE DISPATCHER QUE RECIBE EL HOST VIRTUAL

El módulo de Dispatcher busca encabezados particulares para saber cuándo una solicitud de POST es algo para pasar a AEM renderizaciones o si está serializada como una solicitud de vaciado y debe ser manejada por el propio controlador de Dispatcher.

Esta es una captura de pantalla de la página de configuración que muestra estos valores:
![imagen de la pestaña de configuración principal con el tipo de serialización mostrado como Dispatcher Flush](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

La página de configuración predeterminada muestra la variable `Serialization Type` como `Dispatcher Flush` y establece el nivel de error

![Captura de pantalla de la pestaña de transporte del agente de replicación.  Esto muestra el URI al que se va a enviar la solicitud de vaciado.  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

En el `Transport` puede ver la `URI` se configura para que apunte a la dirección IP del Dispatcher que recibirá las solicitudes de vaciado.  La ruta `/dispatcher/invalidate.cache` no es como el módulo determina si es una descarga, es solo un punto final obvio que puede ver en el registro de acceso para saber que fue una solicitud de vaciado.  En el `Extended` pasaremos por los elementos que están allí para calificar que se trata de una solicitud de vaciado al módulo de Dispatcher.

![Captura de pantalla de la pestaña Ampliado del agente de replicación.  Tenga en cuenta los encabezados que se envían con la solicitud del POST para indicar a Dispatcher que debe vaciar](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

La variable `HTTP Method` para las solicitudes de vaciado, solo es necesario `GET` solicitud con algunos encabezados de solicitud especiales:
- CQ-Action
   - Esto utiliza una variable AEM basada en la solicitud y el valor es normalmente *activar o eliminar*
- CQ-Handle
   - Esto utiliza una variable AEM basada en la solicitud y el valor es normalmente la ruta completa al elemento vaciado, por ejemplo `/content/dam/logo.jpg`
- CQ-Path
   - Esto utiliza una variable AEM basada en la solicitud y el valor es normalmente la ruta completa al elemento que se va a vaciar, por ejemplo `/content/dam`
- Host
   - Aquí es donde la variable `Host` El encabezado se falsifica para dirigirse a un `VirtualHost` que está configurado en el servidor web Apache de Dispatcher (`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`).  Es un valor codificado que coincide con una entrada en la variable `aem_flush.vhost` del archivo `ServerName` o `ServerAlias`

![Pantalla de un agente de replicación estándar que muestra que el agente de replicación con reacción y déclencheur cuando se han recibido nuevos elementos de un evento de replicación desde el contenido de publicación de autor](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

En el `Triggers` tomaremos nota de los déclencheur de alternancia que usamos y de lo que son

- `Ignore default`
   - Esto está habilitado para que el agente de replicación no se active al activar una página.  Esto es algo que, cuando una instancia de autor realizaba un cambio en una página, déclencheur un vaciado.  Debido a que este es un editor, no queremos almacenar en déclencheur ese tipo de evento.
- `On Receive`
   - Cuando se recibe un nuevo archivo, queremos almacenar en déclencheur un vaciado.  Por lo tanto, cuando el autor nos envíe un archivo actualizado, procederemos a enviar una solicitud de vaciado a Dispatcher.
- `No Versioning`
   - Comprobamos esto para evitar que el editor genere nuevas versiones porque se recibió un nuevo archivo.  Solo reemplazaremos el archivo que tenemos y dependeremos del autor para realizar un seguimiento de las versiones en lugar del editor.

Ahora, si observamos cómo se ve una solicitud de vaciado típica en forma de `curl` command

```
$ curl \ 
-H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/dam/logo.jpg" \ 
-H "CQ-Path: /content/dam/" \ 
-H "Content-Length: 0" \  
-H "Content-Type: application/octect-stream" \ 
-H "Host: flush" \ 
http://10.43.0.32:80/dispatcher/invalidate.cache
```

Este ejemplo de vaciado vaciaría el `/content/dam` actualizando la `.stat` en ese directorio.

## La variable `.stat` file

El mecanismo de vaciado es de naturaleza simple y queremos explicar la importancia de la `.stat` archivos que se generan en la raíz del documento donde se crean los archivos de caché.

Dentro de `.vhost` y `_farm.any` archivos configuramos una directiva raíz de documento para especificar dónde se encuentra la caché y dónde almacenar/servir archivos cuando ingresa una solicitud de un usuario final.

Si ejecutara el siguiente comando en su servidor Dispatcher, empezaría a encontrar `.stat` files

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

Aquí hay un diagrama de cómo se verá esta estructura de archivos cuando tenga elementos en la caché y haya enviado y procesado una solicitud de vaciado por el módulo Dispatcher

![archivos de estado combinados con contenido y fechas que se muestran con los niveles de estadísticas mostrados](assets/disp-flushing/dispatcher-statfiles.png "dispatcher-statfiles")

### NIVEL DE ARCHIVO DE ESTADO

Observe que en cada directorio había un `.stat` presente.  Este es un indicador de que se ha producido un vaciado.  En el ejemplo anterior a `statfilelevel` se estableció en `3` dentro del archivo de configuración de granja correspondiente.

La variable `statfilelevel` La configuración indica cuántas carpetas hay en la profundidad del módulo que atravesarán y actualizarán un `.stat` archivo.  El archivo .stat está vacío, no es más que un nombre de archivo con una marca de fecha y podría crearse manualmente pero ejecutando el comando touch en la línea de comandos del servidor Dispatcher.

Si la configuración del nivel del archivo .stat está establecida demasiado alta, cada solicitud de vaciado atraviesa el árbol de directorios tocando los archivos .stat.  Esto puede convertirse en una visita de rendimiento importante en árboles de caché grandes y puede afectar al rendimiento general de Dispatcher.

Si este nivel de archivo es demasiado bajo, una solicitud de vaciado puede borrar más de lo previsto.  Lo que a su vez causaría que la caché se reprodujera más a menudo con menos solicitudes servidas desde la caché y podría causar problemas de rendimiento.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Configure las variables `statfilelevel` a un nivel razonable.  Observe la estructura de carpetas y asegúrese de que está configurada para permitir vaciados concisos sin tener que atravesar demasiados directorios.   Pruébelo y asegúrese de que se ajusta a sus necesidades durante una prueba de rendimiento del sistema.

Un buen ejemplo es un sitio que admite idiomas.  El árbol de contenido típico tendría los siguientes directorios

`/content/brand1/en/us/`

En este ejemplo, utilice una configuración de nivel de archivo .stat de 4.  Esto le asegurará cuando limpie el contenido que se encuentra debajo de la variable <b>`us`</b> que no hará que las carpetas de idioma también se vacíen.
</div>

### SHAKE DE MANO DE MARCA DE TIEMPO DEL ARCHIVO DE ESTADO

Cuando llega una solicitud de contenido, ocurre la misma rutina

1. Marca de tiempo del `.stat` se compara con la marca de tiempo del archivo solicitado
2. Si la variable `.stat` es más reciente que el archivo solicitado, elimina el contenido almacenado en caché y busca uno nuevo de AEM y lo almacena en caché.  A continuación, proporciona el contenido
3. Si la variable `.stat` es anterior al archivo solicitado, sabe que el archivo es nuevo y puede servir el contenido.

### SHAKE DE MANO DE CACHÉ: EJEMPLO 1

En el ejemplo anterior, una solicitud para el contenido `/content/index.html`

La hora del `index.html` es 2019-11-01 @ 18:21 h

La hora de la `.stat` El archivo es 2019-11-01 @ 22:22 PM

Comprendiendo lo que hemos leído anteriormente, puede ver que el archivo de índice es más reciente que el `.stat` y el archivo se servirían desde la caché al usuario final que lo solicitó

### SHAKE DE MANO DE CACHÉ: EJEMPLO 2

En el ejemplo anterior, una solicitud para el contenido `/content/dam/logo.jpg`

La hora del `logo.jpg` es 2019-10-31 @ 13:13 h

La hora de la `.stat` El archivo es 2019-11-01 @ 22:22 PM

Como puede ver en este ejemplo, el archivo es más antiguo que el `.stat` y se eliminará y se extraerá uno nuevo de AEM para reemplazarlo en la caché antes de entregarlo al usuario final que lo solicitó.

## Configuración de archivos de granja

La documentación está disponible aquí para el conjunto completo de opciones de configuración: [https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es)

Queremos destacar algunos de ellos que se refieren al vaciado de caché

### Granjas de descarga

Hay dos claves `document root` directorios que almacenarán en caché los archivos del tráfico de creador y editor.  Para mantener esos directorios actualizados con contenido nuevo, necesitaremos vaciar la caché.  Estas solicitudes de vaciado no desean enredarse en las configuraciones normales de la granja de tráfico del cliente que podrían rechazar la solicitud o hacer algo no deseado.  En su lugar, proporcionamos dos granjas de descarga para esta tarea:

- `/etc/httpd.conf.d/available_farms/001_ams_author_flush_farm.any`
- `/etc/httpd.conf.d/available_farms/001_ams_publish_flush_farm.any`

Estos archivos de granja no hacen nada más que vaciar los directorios raíz del documento.

```
/publishflushfarm {  
    /virtualhosts {
        "flush"
    }
    /cache {
        /docroot "${PUBLISH_DOCROOT}"
        /statfileslevel "${DEFAULT_STAT_LEVEL}"
        /rules {
            $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any"
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
            $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any"
        }
    }
}
```

### Raíz del documento

Esta entrada de configuración se encuentra en la siguiente sección del archivo de granja:

```
/myfarm { 
    /cache { 
        /docroot
```

Especifique el directorio en el que desea que Dispatcher rellene y administre como directorio de caché.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Este directorio debe coincidir con la configuración de la raíz del documento de Apache para el dominio que su servidor web está configurado para usar.

Tener carpetas docroot anidadas por cada granja que vive una subcarpeta de la raíz del documento Apache es una idea terrible por muchas razones.
</div>

### Nivel de archivos .stat

Esta entrada de configuración se encuentra en la siguiente sección del archivo de granja:

```
/myfarm { 
    /cache { 
        /statfileslevel
```

Esta configuración mide la profundidad `.stat` Los archivos deberán generarse cuando entre en vigor una solicitud de vaciado.

`/statfileslevel` se establece en el siguiente número con la raíz del documento de `/var/www/html/` tendría los siguientes resultados al vaciar `/content/dam/brand1/en/us/logo.jpg`

- 0: Se crearían los siguientes archivos .stat
   - `/var/www/html/.stat`
- 1: Se crearían los siguientes archivos .stat
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2: Se crearían los siguientes archivos .stat
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3: Se crearían los siguientes archivos .stat
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4: Se crearían los siguientes archivos .stat
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5: Se crearían los siguientes archivos .stat
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que cuando se produce el protocolo de enlace de marca de tiempo, busca lo más cercano `.stat` archivo.

tener un `.stat` nivel de archivo 0 y un archivo .stat solo en `/var/www/html/.stat` significa que el contenido en el que reside `/var/www/html/content/dam/brand1/en/us/` buscaría la `.stat` y atraviese 5 carpetas para encontrar la única `.stat` archivo que existe en el nivel 0 y compara fechas con eso.  Lo que significa que un vaciado en ese nivel alto básicamente invalidaría todos los elementos almacenados en la caché.
</div>

### Invalidación permitida

Esta entrada de configuración se encuentra en la siguiente sección del archivo de granja:

```
/myfarm { 
    /cache { 
        /allowedClients {
```

Dentro de esta configuración, puede colocar una lista de direcciones IP permitidas para enviar solicitudes de vaciado.  Si una solicitud de vaciado entra en Dispatcher, debe proceder de una IP de confianza.  Si tiene esto mal configurado o envía una solicitud de vaciado desde una dirección IP que no es de confianza, verá el siguiente error en el archivo de registro:

```
[Mon Nov 11 22:43:05 2019] [W] [pid 3079 (tid 139859875088128)] Flushing rejected from 10.43.0.57
```

### Reglas de invalidación

Esta entrada de configuración se encuentra en la siguiente sección del archivo de granja:

```
/myfarm { 
    /cache { 
        /invalidate {
```

Estas reglas suelen indicar qué archivos se pueden invalidar con una solicitud de vaciado.

Para evitar que se invaliden archivos importantes con una activación de página, puede poner reglas en juego que especifiquen qué archivos se pueden invalidar y cuáles se deben invalidar manualmente.  Este es un conjunto de configuración de ejemplo que solo permite invalidar archivos html:

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## Pruebas/solución de problemas

Al activar una página y obtener la luz verde que indica que la activación de la página se ha realizado correctamente, debe esperar que el contenido que ha activado también se limpie de la caché.

¡Actualiza tu página y ves lo viejo! ¿qué? ¡¿había luz verde?!

Vamos a seguir algunos pasos manuales en el proceso de vaciado para darnos una idea de lo que podría estar mal.  Desde el shell del editor, ejecute la siguiente solicitud de vaciado utilizando curl:

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "CQ-Path: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://<DISPATCHER IP ADDRESS>/dispatcher/invalidate.cache
```

Ejemplo de solicitud de vaciado de prueba

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/customer/en-us" \ 
-H "CQ-Path: /content/customer/en-us" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://169.254.196.222/dispatcher/invalidate.cache
```

Una vez que haya activado el comando de solicitud al Dispatcher, querrá ver qué se hace en los registros y qué se hace con el `.stat files`.  Siga el archivo de registro y debería ver las siguientes entradas para confirmar que la solicitud de vaciado ha llegado al módulo de Dispatcher

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

Ahora que vemos que el módulo tomó y reconoció la solicitud de vaciado necesitamos ver cómo afectó a la variable `.stat` archivos.  Ejecute el siguiente comando y observe cómo se actualizan las marcas de tiempo mientras emite otro vaciado:

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

Como puede ver en la salida del comando, las marcas de tiempo de la `.stat` files

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

Ahora, si volvemos a ejecutar el vaciado, verá la actualización de las marcas de tiempo

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

Comparemos nuestras marcas de tiempo de contenido con nuestras `.stat` marcas de tiempo de archivos

```
$ stat /mnt/var/www/html/content/customer/en-us/.stat 
  File: `.stat' 
  Size: 0           Blocks: 0          IO Block: 4096   regular empty file 
Device: ca90h/51856d    Inode: 17154125    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-13 16:22:31.000000000 -0400 
Modify: 2019-11-13 16:22:31.000000000 -0400 
Change: 2019-11-13 16:22:31.000000000 -0400 
 
$ stat /mnt/var/www/html/content/customer/en-us/logo.jpg 
File: `logo.jpg' 
  Size: 15856           Blocks: 32          IO Block: 4096   regular file 
Device: ca90h/51856d    Inode: 9175290    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-11 22:41:59.642450601 +0000 
Modify: 2019-11-11 22:41:59.642450601 +0000 
Change: 2019-11-11 22:41:59.642450601 +0000
```

Si observa cualquiera de las marcas de tiempo, verá que el contenido tiene una hora más reciente que el `.stat` que indica al módulo que sirva el archivo desde la caché porque es más reciente que el `.stat` archivo.

En pocas palabras, algo actualizó las marcas de tiempo de este archivo que no califican para ser &quot;vaciado&quot; o reemplazado.

[Siguiente -> URL mnemónicas](./disp-vanity-url.md)