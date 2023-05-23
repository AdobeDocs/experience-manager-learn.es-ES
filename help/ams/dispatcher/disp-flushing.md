---
title: AEM Vaciado de Dispatcher
description: AEM Comprender cómo invalida los archivos de caché antiguos de Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 461873a1-1edf-43a3-b4a3-14134f855d86
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '2223'
ht-degree: 0%

---

# URL mnemónicas de Dispatcher

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Uso y comprensión de las variables](./variables.md)

Este documento le mostrará cómo se produce el vaciado y le explicará el mecanismo que ejecuta el vaciado y la invalidación de la caché.


## Funcionamiento

### Orden de las operaciones

El flujo de trabajo habitual se describe de la mejor manera cuando los autores de contenido activan una página, cuando el editor recibe el nuevo contenido, se déclencheur una solicitud de vaciado para Dispatcher como se muestra en el siguiente diagrama:
![El autor activa el contenido, lo que déclencheur al editor enviar una solicitud de vaciado a Dispatcher](assets/disp-flushing/dispatcher-flushing-order-of-events.png "dispatcher-flushing-order-of-events")
Esta sucesión de eventos demuestra que solo vaciamos elementos cuando son nuevos o han sido modificados.  Esto garantiza que el editor ha recibido el contenido antes de borrar la caché para evitar condiciones de carrera donde el vaciado podría ocurrir antes de que el editor pueda seleccionar los cambios.

## Agentes de replicación

En el autor, hay un agente de replicación configurado para indicar al editor que cuando algo se activa, tiene el déclencheur de enviar el archivo y todas sus dependencias al editor.

Cuando el editor recibe el archivo, tiene un agente de replicación configurado para indicar qué instancia de Dispatcher almacena en déclencheur el evento de recepción.  Luego serializa una solicitud de vaciado y la publica en Dispatcher.

### AGENTE DE REPLICACIÓN DE AUTOR

Estas son algunas capturas de pantalla de un agente de replicación estándar configurado
![AEM captura de pantalla de un agente de replicación estándar de la página web de la /etc/replication.html](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

Normalmente, hay uno o dos agentes de replicación configurados en el autor para cada editor para el que replican contenido.

Primero, tenemos el agente de replicación estándar que genera activaciones del contenido.

Segundo, el agente inverso.  Esto es opcional y está configurado para comprobar la bandeja de salida de cada editor con el fin de ver si hay contenido nuevo que introducir en la interfaz de autor como actividad de replicación inversa

### AGENTE DE REPLICACIÓN DEL EDITOR

Estas son algunas capturas de pantalla de un agente de replicación de vaciado estándar
![AEM captura de pantalla de un agente de replicación de vaciado estándar de la página web de /etc/replication.html](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### DISPATCHER VACÍA LA REPLICACIÓN RECIBIENDO HOST VIRTUAL

El módulo de Dispatcher busca encabezados particulares para saber cuándo una solicitud de POST AEM se tiene que pasar a los procesamientos o si está serializada como una solicitud de vaciado y la debe administrar el propio controlador de Dispatcher.

A continuación, se muestra una captura de pantalla de la página de configuración que muestra estos valores:
![imagen de la pestaña de configuración principal con el tipo de serialización de vaciado del Dispatcher](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

La página de configuración predeterminada muestra `Serialization Type` as `Dispatcher Flush` y establece el nivel de error

![Captura de pantalla de la pestaña de transporte del agente de replicación.  Esto muestra el URI para publicar solicitudes de vaciado en.  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

En el `Transport` puede ver la pestaña `URI` está configurado para indicar la dirección IP de Dispatcher que recibirá las solicitudes de vaciado.  La ruta `/dispatcher/invalidate.cache` no es la forma en que el módulo determina si hay vaciado. Solo es un punto final obvio que se puede ver en el registro de acceso para saber si se produjo una solicitud de vaciado.  En el `Extended` vamos a revisar las cosas que están ahí para clasificar que esto es una solicitud de vaciado para el módulo de Dispatcher.

![Captura de pantalla de la pestaña Ampliado del agente de replicación.  Tenga en cuenta los encabezados que se envían con la solicitud del POST para indicar al distribuidor que se debe vaciar](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

El `HTTP Method` para solicitudes de vaciado es solo una `GET` solicitud con algunos encabezados de solicitud especiales:
- CQ-Action
   - AEM Esto utiliza una variable de basada en la solicitud y el valor es normalmente *activar o eliminar*
- CQ-Handle
   - AEM Esto utiliza una variable de basada en la solicitud y el valor es normalmente toda la ruta al elemento vaciado, por ejemplo `/content/dam/logo.jpg`
- CQ-Path
   - AEM Esto utiliza una variable de basada en la solicitud y el valor es normalmente toda la ruta al elemento que se está vaciando, por ejemplo `/content/dam`
- Host
   - Aquí es donde la variable `Host` El encabezado se suplanta para dirigirse a un `VirtualHost` que está configurado en el servidor web Apache del distribuidor (`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`).  Es un valor codificado que coincide con una entrada de `aem_flush.vhost` del archivo `ServerName` o `ServerAlias`

![Captura de pantalla de un agente de replicación que muestra que el agente de replicación reacciona y déclencheur cuando se reciben elementos nuevos de un evento de replicación desde el contenido de publicación del autor](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

En el `Triggers` pestaña tomaremos nota de los déclencheur alternados que utilizamos y de cuáles son

- `Ignore default`
   - Esto está habilitado para que el agente de replicación no se active cuando se activa una página.  Esto es algo que, cuando una instancia de autor tenía que realizar un cambio en una página, almacenaba en déclencheur un vaciado.  Déclencheur Como este es un editor, no queremos desactivar ese tipo de evento.
- `On Receive`
   - Cuando se recibe un nuevo archivo, queremos almacenar en déclencheur un vaciado.  Así que cuando el autor nos envíe un archivo actualizado, almacenaremos en déclencheur y enviaremos una solicitud de vaciado a Dispatcher.
- `No Versioning`
   - Comprobamos esto para evitar que el editor genere nuevas versiones porque se recibió un nuevo archivo.  Solo reemplazaremos el archivo que tenemos y confiaremos en que el autor mantenga un seguimiento de las versiones en lugar del editor.

Ahora, si miramos el aspecto habitual de una solicitud de vaciado en forma de `curl` mando

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

Este ejemplo de vaciado vaciaría el `/content/dam` ruta actualizando el `.stat` en ese directorio.

## El `.stat` archivo

El mecanismo de vaciado es sencillo y queremos explicar la importancia de la `.stat` archivos que se generan en la raíz del documento donde se crean los archivos de caché.

Dentro de `.vhost` y `_farm.any` archivos configuramos una directiva raíz de documento para especificar dónde se encuentra la caché y dónde almacenar o entregar los archivos cuando llega una solicitud de un usuario final.

Si tuviera que ejecutar el siguiente comando en el servidor de Dispatcher, empezaría por buscar `.stat` archivos

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

Este diagrama muestra el aspecto de la estructura de este archivo cuando tenga elementos en la caché y haya enviado una solicitud de vaciado y procesado el módulo de Dispatcher

![archivos .stat mezclados con contenido y fechas que se muestran con los niveles de .stat](assets/disp-flushing/dispatcher-statfiles.png "dispatcher-statfiles")

### NIVEL DE ARCHIVO ESTADÍSTICO

Observe que en cada directorio había un `.stat` archivo presente.  Esto indica que se ha producido un vaciado.  En el ejemplo anterior, la variable `statfilelevel` la configuración se estableció en `3` dentro del archivo de configuración de granja correspondiente.

El `statfilelevel` la configuración indica cuántas carpetas profundas atravesará el módulo y actualizará una `.stat` archivo.  El archivo .stat está vacío. No es más que un nombre de archivo con marca de fecha y podría crearse incluso manualmente ejecutando el comando táctil en la línea de comandos del servidor de Dispatcher.

Si la configuración del nivel del archivo .stat está establecida demasiado alta, cada solicitud de vaciado atravesará el árbol de directorios tocando los archivos .stat.  Esto puede convertirse en una gran disminución del rendimiento en árboles de caché grandes y afectar al rendimiento general de Dispatcher.

Configurar este nivel de archivo demasiado bajo puede provocar que una solicitud de vaciado limpie más de lo necesario.  Lo que, a su vez, haría que la caché se perdiera con más frecuencia, con menos solicitudes recibidas desde la caché y podría causar problemas de rendimiento.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Configure las variables `statfilelevel` a un nivel razonable.  Observe la estructura de carpetas y asegúrese de que está configurada para permitir vaciados concisos sin tener que atravesar demasiados directorios.   Pruébelo y asegúrese de que se adapta a sus necesidades durante una prueba de rendimiento del sistema.

Un buen ejemplo es un sitio que admite diferentes idiomas.  El típico árbol de contenido tendría los siguientes directorios

`/content/brand1/en/us/`

En este ejemplo, utilice una configuración de nivel de archivo .stat de 4.  Esto le garantizará que cuando vacíe contenido que se encuentre debajo de <b>`us`</b> que no hará que las carpetas de idioma se vacíen también.
</div>

### PROTOCOLO DE ENLACE CON MARCA DE TIEMPO DEL ARCHIVO STAT

Cuando llega una solicitud de contenido, se repite el mismo proceso

1. Marca de tiempo del `.stat` se compara con la marca de tiempo del archivo solicitado
2. Si la variable `.stat` AEM El archivo es más reciente que el archivo solicitado. Elimina el contenido de la caché y obtiene uno nuevo de la caché de los archivos y los almacena en la caché de los archivos.  A continuación, se sirve el contenido
3. Si la variable `.stat` es anterior al archivo solicitado, entonces sabe que el archivo es nuevo y puede servir el contenido.

### PROTOCOLO DE ENLACE DE CACHÉ: EJEMPLO 1

En el ejemplo anterior, una solicitud para el contenido `/content/index.html`

La hora de la `index.html` file is 2019-11-01 @ 18:21 h

La hora de la más cercana `.stat` file is 2019-11-01 @ 12:22 h

Si tenemos en cuenta lo que hemos leído anteriormente, verá que el archivo de índice es más reciente que el `.stat` y el archivo se entregaría desde la caché al usuario final que lo solicitó

### PROTOCOLO DE ENLACE DE CACHÉ: EJEMPLO 2

En el ejemplo anterior, una solicitud para el contenido `/content/dam/logo.jpg`

La hora de la `logo.jpg` file is 2019-10-31 @ 13:13 h

La hora de la más cercana `.stat` file is 2019-11-01 @ 12:22 h

Como puede ver en este ejemplo, el archivo es más antiguo que el `.stat` AEM y se eliminarán, y se extraerá uno nuevo de la caché para reemplazarlo, antes de que se envíe al usuario final que lo solicitó.

## Configuración de archivo de granja

Puede acceder a toda la documentación sobre el conjunto completo de opciones de configuración aquí: [https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es)

Nos gustaría destacar algunas de ellas que están relacionadas con el vaciado de caché

### Vaciar granjas

Hay dos claves `document root` directorios que almacenarán en caché archivos del tráfico de autor y editor.  Para mantener esos directorios actualizados con contenido nuevo, tendremos que vaciar la caché.  Estas solicitudes de vaciado no desean enredarse con las configuraciones normales del conjunto de servidores de tráfico de clientes que podrían rechazar la solicitud o hacer algo no deseado.  En su lugar, proporcionamos dos granjas de vaciado para esta tarea:

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

### Raíz de documento

Esta entrada de configuración se encuentra en la siguiente sección del archivo de granja:

```
/myfarm { 
    /cache { 
        /docroot
```

Especifique el directorio en el que desea que Dispatcher se propague y se administre como directorio de caché.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Este directorio debe coincidir con la configuración de la raíz del documento de Apache para el dominio que su servidor web pueda usar.

Tener carpetas docroot anidadas para cada granja que se encuentra en subcarpetas de la raíz del documento de Apache es una idea terrible por muchas razones.
</div>

### Nivel de archivos estáticos

Esta entrada de configuración se encuentra en la siguiente sección del archivo de granja:

```
/myfarm { 
    /cache { 
        /statfileslevel
```

Esta configuración indica la profundidad `.stat` los archivos deberán generarse cuando aparezca una solicitud de vaciado.

`/statfileslevel` establezca en el siguiente número con la raíz del documento de `/var/www/html/` tendría los siguientes resultados al vaciar `/content/dam/brand1/en/us/logo.jpg`

- 0: Se crearían los siguientes archivos .stat.
   - `/var/www/html/.stat`
- 1: Se crearían los siguientes archivos .stat.
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2: Se crearían los siguientes archivos .stat.
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3: Se crearían los siguientes archivos .stat.
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4: Se crearían los siguientes archivos .stat.
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5: Se crearían los siguientes archivos .stat.
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Tenga en cuenta que cuando el protocolo de enlace de marca de tiempo se produce, busca lo más cercano `.stat` archivo.

teniendo un `.stat` nivel de archivo 0 y un archivo stat solo en `/var/www/html/.stat` significa que el contenido que se encuentra debajo de `/var/www/html/content/dam/brand1/en/us/` buscaría la más cercana `.stat` y atraviese 5 carpetas para encontrar la única `.stat` que existe en el nivel 0 y compare las fechas con eso.  Lo que significa que un vaciado a un nivel tan alto sencillamente invalidaría todos los elementos almacenados en la caché.
</div>

### Invalidación permitida

Esta entrada de configuración se encuentra en la siguiente sección del archivo de granja:

```
/myfarm { 
    /cache { 
        /allowedClients {
```

Dentro de esta configuración es donde se coloca una lista de direcciones IP permitidas para enviar solicitudes de vaciado.  Si una solicitud de vaciado llega a Dispatcher debe hacerlo desde una IP de confianza.  Si tiene esto mal configurado o envía una solicitud de vaciado desde una dirección IP que no es de confianza, verá el siguiente error en el archivo de registro:

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

Estas reglas indican normalmente qué archivos se pueden invalidar con una solicitud de vaciado.

Para evitar que archivos importantes se invaliden con una activación de página, puede poner en juego reglas que especifiquen qué archivos son adecuados para invalidar y cuáles deben invalidarse manualmente.  Este es un ejemplo de conjunto de configuración que solo permite invalidar los archivos HTML:

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## Pruebas/solución de problemas

Cuando activa una página y se asegura de que la activación de la página se ha realizado correctamente, debería esperar que el contenido que ha activado se vaya a vaciar también de la caché.

Actualiza la página y ve el contenido antiguo. ¿qué!? había una luz verde?!

Vamos a seguir algunos pasos manuales del proceso de vaciado para obtener información sobre qué puede estar mal.  Desde el shell del editor, ejecute la siguiente solicitud de vaciado usando curl:

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

Una vez que haya enviado el comando de solicitud a Dispatcher, querrá ver qué ha sucedido en los registros y en el `.stat files`.  Siga el archivo de registro y debería ver las siguientes entradas para confirmar que la solicitud de vaciado llegue al módulo del distribuidor

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

Ahora que vemos que el módulo recibió y confirmó la solicitud de vaciado, necesitamos saber cómo afectó a la `.stat` archivos.  Ejecute el siguiente comando y observe cómo se actualizan las marcas de tiempo mientras emite otro vaciado:

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

Como puede ver en el resultado del comando, las marcas de tiempo del `.stat` archivos

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

Ahora, si volvemos a ejecutar el vaciado, verá cómo se actualizan las marcas de tiempo

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

Vamos a comparar las marcas de tiempo del contenido con nuestras `.stat` archivos marcas de tiempo

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

Si mira cualquiera de las marcas de tiempo verá que el contenido tiene una hora más reciente que la `.stat` que indica al módulo que sirva el archivo desde la caché porque es más reciente que el `.stat` archivo.

En pocas palabras, algo actualizó las marcas de tiempo de este archivo que no cumple los requisitos para &quot;vaciarlo&quot; o reemplazarlo.

[Siguiente -> URL de vanidad](./disp-vanity-url.md)
