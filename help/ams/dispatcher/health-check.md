---
title: Comprobación de estado de AMS Dispatcher
description: AMS proporciona un script cgi-bin de comprobación de estado que los equilibradores de carga de la nube ejecutarán para ver si el AEM está en buen estado y debe permanecer en servicio para el tráfico público.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# Comprobación de estado de AMS Dispatcher

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Archivos de solo lectura](./immutable-files.md)

Cuando tiene una línea de base de AMS instalada, Dispatcher viene con algunos freebies.  Una de estas características es un conjunto de scripts de comprobación de estado.
Estos scripts permiten que el equilibrador de carga que delante de la pila de AEM sepa qué piernas están sanas y que las mantenga en servicio.

![GIF animado que muestra el flujo de tráfico](assets/load-balancer-healthcheck/health-check.gif "Pasos de comprobación de estado")

## Comprobación de estado del equilibrador de carga básico

Cuando el tráfico de los clientes llega a través de Internet para llegar a la instancia de AEM, estos pasan por un equilibrador de carga

![La imagen muestra el flujo de tráfico de Internet a aem a través de un equilibrador de carga](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "equilibrio de carga-tráfico-flujo")

Cada solicitud que se realice a través del equilibrador de carga redondeará el robo a cada instancia.  El equilibrador de carga tiene un mecanismo de comprobación de estado integrado para asegurarse de que envía tráfico a un host saludable.

La comprobación predeterminada suele ser una comprobación de puerto para ver si los servidores dirigidos en el equilibrador de carga están escuchando el tráfico del puerto que entra en funcionamiento (es decir, TCP 80 y 443)

> `Note:` Aunque esto funciona, no tiene una medición real si AEM saludable.  Solo comprueba si Dispatcher (servidor web Apache) está en funcionamiento.

## Comprobación de estado de AMS

Para evitar enviar tráfico a un Dispatcher saludable que está enviando una instancia de AEM no saludable, AMS creó algunos extras que evalúan el estado de la pierna y no solo el Dispatcher.

![La imagen muestra las diferentes piezas para que funcione la comprobación de estado](assets/load-balancer-healthcheck/health-check-pieces.png "comprobación de estado")

La comprobación de estado consta de las siguientes piezas
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

Cubriremos lo que cada pieza está configurada para hacer y su importancia

### AEM

Para indicar si AEM está funcionando, necesita que realice alguna compilación de página básica y sirva la página.  Adobe Managed Services ha creado un paquete básico que contiene la página de prueba.  La página prueba que el repositorio está activo y que los recursos y la plantilla de página pueden procesarse.

![La imagen muestra el paquete AMS en el gestor de paquetes CRX](assets/load-balancer-healthcheck/health-check-package.png "paquete de comprobación de estado")

Esta es la página.  Muestra el ID del repositorio de la instalación

![La imagen muestra la página Regente de AMS](assets/load-balancer-healthcheck/health-check-page.png "página de comprobación de estado")

> `Note:` Nos aseguramos de que la página no se pueda almacenar en caché.  No comprobaría el estado real si cada vez devolvía una página en caché.

Este es el punto final de peso ligero que podemos probar para ver que AEM está funcionando.

### Configuración del equilibrador de carga

Configuramos los equilibradores de carga para que apunten a un extremo CGI-BIN en lugar de utilizar una comprobación de puerto.

![La imagen muestra la configuración de mantenimiento del equilibrador de carga de AWS](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![La imagen muestra la configuración de la comprobación de estado del equilibrador de carga de Azure](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Hosts virtuales de comprobación de estado de Apache

#### Host virtual CGI-BIN `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

Esta es la `<VirtualHost>` Archivo de configuración Apache que permite ejecutar los archivos CGI-Bin.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` los archivos cgi-bin son secuencias de comandos que se pueden ejecutar.  Esto puede ser un vector de ataque vulnerable y estos scripts que utiliza AMS no son accesibles al público solo están disponibles para el equilibrador de carga con el que realizar pruebas.


#### Hosts Virtuales De Mantenimiento No Sanos

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

Estos archivos tienen el nombre `000_` como prefijo a propósito.  Está configurado intencionalmente para utilizar el mismo nombre de dominio que el sitio activo.  La intención es que este archivo se habilite cuando la comprobación de estado detecte que hay un problema con uno de los backends de AEM.  A continuación, ofrezca una página de error en lugar de simplemente un código de respuesta HTTP 503 sin página.  Robará tráfico de lo normal `.vhost` porque está cargado antes de que `.vhost` mientras comparte el mismo `ServerName` o `ServerAlias`.  Como resultado, las páginas destinadas a un dominio en particular van al host no saludable en lugar del predeterminado por el que fluye el tráfico normal.

Cuando se ejecutan las secuencias de comandos de comprobación de estado, se cierra la sesión con el estado actual.  Una vez al minuto, se ejecuta un cronjob en el servidor que busca entradas no saludables en el registro.  Si detecta que la instancia de autor AEM no está en buen estado, habilitará el enlace simbólico:

Entrada de registro:

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron recopiló el error y reaccionó:

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

Puede controlar si los sitios de autor o publicados pueden tener este error al cargar la página configurando el modo de recarga en `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

Opciones válidas:
- author
   - Esta es la opción predeterminada.
   - Esto creará una página de mantenimiento para el autor cuando no esté en buen estado
- instancias de publicación
   - Esta opción creará una página de mantenimiento para el editor cuando no esté en buen estado
- all
   - Esta opción creará una página de mantenimiento para el autor o el editor, o ambos, si no están en buen estado
- ninguno
   - Esta opción omite esta característica de la comprobación de estado

Al consultar la `VirtualHost` para estos ajustes, verá que cargan el mismo documento que una página de error para cada solicitud que se produce cuando está habilitada:

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

El código de respuesta sigue siendo un `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

En lugar de una página en blanco, obtendrán esta página.

![La imagen muestra la página de mantenimiento predeterminada](assets/load-balancer-healthcheck/unhealthy-page.png "página no saludable")

### Scripts CGI-Bin

Hay 5 scripts diferentes que su CSE puede configurar en la configuración del equilibrador de carga que cambian el comportamiento o los criterios cuando se extrae un Dispatcher del equilibrador de carga.

#### /bin/checkauthor

Cuando se utilice, este script comprobará y registrará todas las instancias a las que esté enviando, pero solo devolverá un error si la variable `author` AEM instancia no está en buen estado

> `Note:` Tenga en cuenta que si la instancia de AEM de publicación no está en buen estado, Dispatcher permanecería en servicio para permitir que el tráfico fluya a la instancia de AEM de autor

#### /bin/checkpublish (predeterminado)

Cuando se utilice, este script comprobará y registrará todas las instancias a las que esté enviando, pero solo devolverá un error si la variable `publish` AEM instancia no está en buen estado

> `Note:` Tenga en cuenta que si la instancia de AEM del autor no está en buen estado, Dispatcher permanecería en servicio para permitir que el tráfico fluya a la instancia de AEM de publicación

#### /bin/checkor

Cuando se utilice, este script comprobará y registrará todas las instancias a las que esté enviando, pero solo devolverá un error si la variable `author` o `publisher` AEM instancia no está en buen estado

> `Note:` Tenga en cuenta que si la instancia de publicación AEM o la instancia de autor AEM no estaban en buen estado, Dispatcher se retiraría del servicio.  Significa que si uno de ellos estaba en buen estado, tampoco recibiría tráfico

#### /bin/checktwo

Cuando se utilice, este script comprobará y registrará todas las instancias a las que esté enviando, pero solo devolverá un error si la variable `author` y `publisher` AEM instancia no está en buen estado

> `Note:` Tenga en cuenta que si la instancia de publicación AEM o de autor AEM no estaba en buen estado, Dispatcher no se retiraría del servicio.  Lo que significa que si uno de ellos no está en buen estado, seguirá recibiendo tráfico y dará errores a las personas que soliciten recursos.

#### /bin/healthy

Cuando se utilice, este script comprobará y registrará todas las instancias a las que va a enviar, pero solo devolverá un estado saludable independientemente de si AEM devuelve un error o no.

> `Note:` Esta secuencia de comandos se utiliza cuando la comprobación de estado no funciona como se desea y permite una anulación para mantener AEM instancias en el equilibrador de carga.

[Siguiente -> Enlaces de GIT](./git-symlinks.md)