---
title: Vaciado de AEM Dispatcher
description: Comprenda cómo AEM invalida los archivos de caché antiguos de Dispatcher.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 461873a1-1edf-43a3-b4a3-14134f855d86
duration: 520
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2225'
ht-degree: 0%

---

# URL mnemónicas de Dispatcher

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Uso y comprensión de las variables](./variables.md)

Este documento le mostrará cómo se produce el vaciado y le explicará el mecanismo que ejecuta el vaciado y la invalidación de la caché.


## Funcionamiento

### Orden de las operaciones

El flujo de trabajo habitual se describe de la mejor manera cuando los autores de contenido activan una página, cuando el editor recibe el nuevo contenido, se déclencheur una solicitud de vaciado para la Dispatcher, como se muestra en el siguiente diagrama:
![el autor activa el contenido, lo que déclencheur al editor enviar una solicitud de vaciado a Dispatcher](assets/disp-flushing/dispatcher-flushing-order-of-events.png "dispatcher-flushing-order-of-events")
Esta sucesión de eventos demuestra que solo vaciamos elementos cuando son nuevos o han sido modificados.  Esto garantiza que el editor ha recibido el contenido antes de borrar la caché para evitar condiciones de carrera donde el vaciado podría ocurrir antes de que el editor pueda seleccionar los cambios.

## Agentes de replicación

En el autor, hay un agente de replicación configurado para indicar al editor que cuando algo se activa, tiene el déclencheur de enviar el archivo y todas sus dependencias al editor.

Cuando el editor recibe el archivo, tiene un agente de replicación configurado para indicar qué Dispatcher está en déclencheur con el evento de recepción.  Entonces, se serializa una solicitud de vaciado y se publica en Dispatcher.

### AGENTE DE REPLICACIÓN DE AUTOR

Estas son algunas capturas de pantalla de un agente de replicación estándar configurado
![captura de pantalla del agente de replicación estándar de la página web de AEM /etc/replication.html](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

Normalmente, hay uno o dos agentes de replicación configurados en el autor para cada editor para el que replican contenido.

Primero, tenemos el agente de replicación estándar que genera activaciones del contenido.

Segundo, el agente inverso.  Esto es opcional y está configurado para comprobar la bandeja de salida de cada editor con el fin de ver si hay contenido nuevo que introducir en la interfaz de autor como actividad de replicación inversa

### AGENTE DE REPLICACIÓN DEL EDITOR

Estas son algunas capturas de pantalla de un agente de replicación de vaciado estándar
![captura de pantalla del agente de replicación de vaciado estándar de la página web de AEM /etc/replication.html](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### VACIAR REPLICACIÓN DE DISPATCHER RECIBIENDO HOST VIRTUAL

El módulo de Dispatcher busca encabezados particulares para saber cuándo una solicitud de POST se tiene que pasar a los procesamientos de AEM o si está serializada como una solicitud de vaciado y la debe administrar el propio controlador de Dispatcher.

A continuación, se muestra una captura de pantalla de la página de configuración que muestra estos valores:
![imagen de la pestaña de configuración principal con el tipo de serialización Dispatcher Flush](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

La página de configuración predeterminada muestra `Serialization Type` como `Dispatcher Flush` y establece el nivel de error

![Captura de pantalla de la ficha de transporte del agente de replicación.  Esto muestra el URI para publicar solicitudes de vaciado en.  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

En la ficha `Transport` puede ver que `URI` se ha configurado para que apunte a la dirección IP del Dispatcher que recibirá las solicitudes de vaciado.  La ruta de acceso `/dispatcher/invalidate.cache` no es la forma en que el módulo determina si hay vaciado. Solo es un punto final obvio que se puede ver en el registro de acceso para saber si se produjo una solicitud de vaciado.  En la ficha `Extended` veremos qué elementos permiten comprobar que se trata de una solicitud de vaciado para el módulo Dispatcher.

![Captura de pantalla de la ficha Ampliado del agente de replicación.  Observe los encabezados que se envían con la petición POST enviada para indicar a Dispatcher que vacíe &#x200B;](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

El `HTTP Method` para solicitudes de vaciado es solo una solicitud de `GET` con algunos encabezados especiales:
- CQ-Action
   - Utiliza una variable de AEM basada en la solicitud y su valor es normalmente *activar o eliminar*
- CQ-Handle
   - Esto utiliza una variable de AEM basada en la solicitud y el valor es normalmente toda la ruta al elemento vaciado, por ejemplo `/content/dam/logo.jpg`
- CQ-Path
   - Esto utiliza una variable de AEM basada en la solicitud y el valor es normalmente toda la ruta al elemento que se está vaciando, por ejemplo `/content/dam`
- Host
   - Aquí es donde el encabezado `Host` se suplanta para asignarse a un `VirtualHost` específico configurado en el servidor web Apache del distribuidor (`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`).  Es un valor codificado que coincide con una entrada en `ServerName` o `ServerAlias` del archivo `aem_flush.vhost`

![Pantalla de un agente de replicación que muestra que el agente de replicación reaccionó y generó déclencheur cuando se recibieron elementos nuevos de un evento de replicación desde el contenido de publicación del autor](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

En la ficha `Triggers` veremos cuáles son los déclencheur alternados que usamos y cuáles son

- `Ignore default`
   - Esto está habilitado para que el agente de replicación no se active cuando se activa una página.  Esto es algo que, cuando una instancia de autor tenía que realizar un cambio en una página, almacenaba en déclencheur un vaciado.  Déclencheur Como este es un editor, no queremos desactivar ese tipo de evento.
- `On Receive`
   - Cuando se recibe un nuevo archivo, queremos almacenar en déclencheur un vaciado.  Por lo tanto, cuando el autor nos envíe un archivo actualizado, almacenaremos en déclencheur y enviaremos una solicitud de vaciado a Dispatcher.
- `No Versioning`
   - Comprobamos esto para evitar que el editor genere nuevas versiones porque se recibió un nuevo archivo.  Solo reemplazaremos el archivo que tenemos y confiaremos en que el autor mantenga un seguimiento de las versiones en lugar del editor.

Ahora, si miramos el aspecto habitual de una solicitud de vaciado en forma de comando `curl`

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

Este ejemplo vaciaría la ruta de acceso `/content/dam` al actualizar el archivo `.stat` en ese directorio.

## El archivo `.stat`

El mecanismo de vaciado es sencillo y nos gustaría explicar la importancia de los `.stat` archivos que se generan en la raíz del documento donde se crean los archivos de la caché.

Dentro de los archivos `.vhost` y `_farm.any`, configuramos una directiva raíz de documento para especificar dónde se encuentra la caché y dónde almacenar o entregar los archivos cuando llega una solicitud de un usuario final.

Si tuviera que ejecutar el siguiente comando en el servidor de Dispatcher, empezaría por buscar `.stat` archivos

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

Este diagrama muestra el aspecto de la estructura de este archivo cuando tenga elementos en la caché y haya enviado una solicitud de vaciado y procesado el módulo de Dispatcher

![archivos de estado mezclados con contenido y fechas que se muestran con los niveles de estado mostrados](assets/disp-flushing/dispatcher-statfiles.png "archivos de estado del distribuidor")

### NIVEL DE ARCHIVO ESTADÍSTICO

Observe que en cada directorio había un archivo `.stat`.  Esto indica que se ha producido un vaciado.  En el ejemplo anterior, la configuración de `statfilelevel` se estableció en `3` dentro del archivo de configuración de granja correspondiente.

La configuración `statfilelevel` indica cuántas carpetas profundas en el módulo atravesarán y actualizarán un archivo `.stat`.  El archivo .stat está vacío. No es más que un nombre de archivo con marca de fecha y podría crearse incluso manualmente ejecutando el comando táctil en la línea de comandos del servidor de Dispatcher.

Si la configuración del nivel del archivo .stat está establecida demasiado alta, cada solicitud de vaciado atravesará el árbol de directorios tocando los archivos .stat.  Esto puede convertirse en una gran disminución del rendimiento en árboles de caché grandes y afectar al rendimiento general de su Dispatcher.

Configurar este nivel de archivo demasiado bajo puede provocar que una solicitud de vaciado limpie más de lo necesario.  Lo que, a su vez, haría que la caché se perdiera con más frecuencia, con menos solicitudes recibidas desde la caché y podría causar problemas de rendimiento.

>[!BEGINSHADEBOX &quot;Nota&quot;]

Establezca `statfilelevel` en un nivel razonable. Observe la estructura de carpetas y asegúrese de que está configurada para permitir vaciados concisos sin tener que atravesar demasiados directorios. Pruébelo y asegúrese de que se adapta a sus necesidades durante una prueba de rendimiento del sistema.

Un buen ejemplo es un sitio que admite diferentes idiomas. El típico árbol de contenido tendría los siguientes directorios

`/content/brand1/en/us/`

En este ejemplo, utilice una configuración de nivel de archivo .stat de 4. Esto le asegurará al vaciar contenido que se encuentre debajo de la carpeta **`us`** que no se vaciarán también las carpetas de idioma.

>[!ENDSHADEBOX]

### PROTOCOLO DE ENLACE CON MARCA DE TIEMPO DEL ARCHIVO STAT

Cuando llega una solicitud de contenido, se repite el mismo proceso

1. La marca de tiempo del archivo `.stat` se compara con la del archivo solicitado
2. Si el archivo `.stat` es más reciente que el archivo solicitado, se eliminará el contenido de la caché y se recuperará uno nuevo de AEM, que se almacenará en la caché.  A continuación, se sirve el contenido
3. Si el archivo `.stat` es más antiguo que el archivo solicitado, entonces se sabe que el archivo es reciente y se puede servir el contenido.

### PROTOCOLO DE ENLACE DE CACHÉ: EJEMPLO 1

En el ejemplo anterior, una solicitud para el contenido `/content/index.html`

La fecha del archivo `index.html` es 2019-11-01 @ 18:21 h

La fecha del archivo `.stat` más cercano es 2019-11-01 @ 12:22 h

Si tenemos en cuenta lo que hemos leído anteriormente, verá que el archivo de índice es más reciente que el archivo `.stat` y se entregará desde la caché al usuario final que lo solicitó

### PROTOCOLO DE ENLACE DE CACHÉ: EJEMPLO 2

En el ejemplo anterior, una solicitud para el contenido `/content/dam/logo.jpg`

La fecha del archivo `logo.jpg` es 2019-10-31 @ 13:13 h

La fecha del archivo `.stat` más cercano es 2019-11-01 @ 12:22 h

Como puede ver en este ejemplo, el archivo es más antiguo que el archivo de `.stat` y se eliminará. Uno más reciente se sacará de AEM para reemplazarlo en la caché antes de entregárselo al usuario final que lo solicitó.

## Configuración de archivo de granja

Toda la documentación está aquí para obtener el conjunto completo de opciones de configuración: [https://docs.adobe.com/content/help/es-ES/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es)

Nos gustaría destacar algunas de ellas que están relacionadas con el vaciado de caché

### Vaciar granjas

Hay dos directorios de claves `document root` que almacenarán en caché los archivos del tráfico de autores y editores.  Para mantener esos directorios actualizados con contenido nuevo, tendremos que vaciar la caché.  Estas solicitudes de vaciado no desean enredarse con las configuraciones normales del conjunto de servidores de tráfico de clientes que podrían rechazar la solicitud o hacer algo no deseado.  En su lugar, proporcionamos dos granjas de vaciado para esta tarea:

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

Especifique el directorio en el que desea que Dispatcher se rellene y administre como directorio de caché.

>[!NOTE]
>
>Este directorio debe coincidir con la configuración de la raíz del documento de Apache para el dominio que su servidor web pueda usar.
>
>Tener carpetas docroot anidadas para cada granja que se encuentra en subcarpetas de la raíz del documento de Apache es una idea terrible por muchas razones.

### Nivel de archivos estáticos

Esta entrada de configuración se encuentra en la siguiente sección del archivo de granja:

```
/myfarm { 
    /cache { 
        /statfileslevel
```

Esta configuración indica la profundidad con la que se deberán generar los archivos de `.stat` cuando aparezca una solicitud de vaciado.

`/statfileslevel` establecido en el siguiente número con la raíz del documento de `/var/www/html/` tendría los siguientes resultados al vaciar `/content/dam/brand1/en/us/logo.jpg`

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

>[!NOTE]
>
>Tenga en cuenta que cuando el protocolo de enlace de marca de tiempo se produce, busca el archivo `.stat` más cercano.
>
>Tener un archivo de `.stat` nivel 0 y un archivo .stat solamente en `/var/www/html/.stat` significa que el contenido que se encuentra por debajo de `/var/www/html/content/dam/brand1/en/us/` buscará el archivo de `.stat` más cercano y atravesará 5 carpetas para encontrar el único archivo de `.stat` que existe en el nivel 0 y comparará las fechas con eso. Lo que significa que un vaciado a un nivel tan alto sencillamente invalidaría todos los elementos almacenados en la caché.

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

Una vez que haya enviado el comando de solicitud a Dispatcher, querrá ver qué ha ocurrido en los registros y en `.stat files`.  Siga el archivo de registro y debería ver las siguientes entradas para confirmar que la solicitud de vaciado llegue al módulo de Dispatcher

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

Ahora que vemos que el módulo recibió y confirmó la solicitud de vaciado, necesitamos saber de qué manera ha afectado a los `.stat` archivos.  Ejecute el siguiente comando y observe cómo se actualizan las marcas de tiempo mientras emite otro vaciado:

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

Como puede ver en el resultado del comando, las marcas de tiempo de los `.stat` archivos actuales

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

Vamos a comparar las marcas de tiempo del contenido con las de nuestros `.stat` archivos

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

Si mira cualquiera de las marcas de tiempo verá que el contenido tiene una hora más reciente que el archivo `.stat`, lo que indica al módulo ofrecer el archivo de la caché porque es más reciente que el archivo `.stat`.

En pocas palabras, algo actualizó las marcas de tiempo de este archivo que no cumple los requisitos para &quot;vaciarlo&quot; o reemplazarlo.

[Siguiente -> URL de vanidad](./disp-vanity-url.md)
